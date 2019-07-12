---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634746"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8fd5e-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="8fd5e-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="8fd5e-103">この手順では、 [Java 用 Microsoft 認証ライブラリ (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java)をアプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

<span data-ttu-id="8fd5e-104">**/Graphtutorial/src/main/java/com/contoso**ディレクトリに、 **oAuth**という名前の新しいファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-104">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **oAuth.properties**.</span></span> <span data-ttu-id="8fd5e-105">そのファイルに次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-105">Add the following text in that file.</span></span>

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

<span data-ttu-id="8fd5e-106">を`YOUR_APP_ID_HERE` Azure ポータルで作成したアプリケーション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8fd5e-107">Git などのソース管理を使用している場合は、アプリ ID を誤ってリークしないように、ソース管理から**oAuth**ファイルを除外することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="8fd5e-108">**App.xaml**を開き、次`import`のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-108">Open **App.java** and add the following `import` statements.</span></span>

```java
import java.io.IOException;
import java.util.Properties;
```

<span data-ttu-id="8fd5e-109">その後、次のコードを行`Scanner input = new Scanner(System.in);`の直前に追加して、 **oAuth プロパティ**ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-109">Then add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a><span data-ttu-id="8fd5e-110">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="8fd5e-110">Implement sign-in</span></span>

<span data-ttu-id="8fd5e-111">**/Graphtutorial/src/main/java/com/contoso**という名前のディレクトリに新しいファイルを\*\*\*\* 作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-111">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Authentication.java** and add the following code.</span></span>

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/organizations/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

<span data-ttu-id="8fd5e-112">**App.xaml**で、次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、アクセストークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

<span data-ttu-id="8fd5e-113">コメントの`// Display access token`後に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-113">Then add the following line after the `// Display access token` comment.</span></span>

```java
System.out.println("Access token: " + accessToken);
```

<span data-ttu-id="8fd5e-114">アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-114">Build and run the app.</span></span> <span data-ttu-id="8fd5e-115">アプリケーションには、URL とデバイスコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-115">The application displays a URL and device code.</span></span>

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

<span data-ttu-id="8fd5e-116">ブラウザーを開き、表示されている URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="8fd5e-117">提供されたコードを入力し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="8fd5e-118">完了したら、アプリケーションに戻り、1を選択し**ます。** アクセストークンを表示するためのアクセストークンオプションを表示します。</span><span class="sxs-lookup"><span data-stu-id="8fd5e-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
