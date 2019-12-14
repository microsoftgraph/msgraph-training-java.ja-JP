---
ms.openlocfilehash: 377db05d9b238d3f6f6e45032f0b85b9217df3db
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018835"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0bf47-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="0bf47-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="0bf47-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="0bf47-103">この手順では、 [Java 用 Microsoft 認証ライブラリ (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java)をアプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

<span data-ttu-id="0bf47-104">**/Graphtutorial/src/main**ディレクトリに、 **resources**という名前の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-104">Create a new directory named **resources** in the **./graphtutorial/src/main** directory.</span></span> <span data-ttu-id="0bf47-105">その後、 **resources**ディレクトリに**com**という名前の新しいディレクトリを作成し、 **com**ディレクトリに**contoso**という名前の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-105">Then create a new directory named **com** in the **resources** directory, then a new directory named **contoso** in the **com** directory.</span></span> <span data-ttu-id="0bf47-106">最後に、 **/graphtutorial/src/main/resources/com/contoso**ディレクトリに**oAuth. プロパティ**という名前の新しいファイルを作成し、そのファイルに次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-106">Finally, create a new file in the **./graphtutorial/src/main/resources/com/contoso** directory named **oAuth.properties**, and add the following text in that file.</span></span>

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

<span data-ttu-id="0bf47-107">を`YOUR_APP_ID_HERE` Azure ポータルで作成したアプリケーション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0bf47-107">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0bf47-108">Git などのソース管理を使用している場合は、アプリ ID を誤ってリークしないように、ソース管理から**oAuth**ファイルを除外することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0bf47-108">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="0bf47-109">**App.xaml**を開き、次`import`のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-109">Open **App.java** and add the following `import` statements.</span></span>

```java
import java.io.IOException;
import java.util.Properties;
```

<span data-ttu-id="0bf47-110">その後、次のコードを行`Scanner input = new Scanner(System.in);`の直前に追加して、 **oAuth プロパティ**ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0bf47-110">Then add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

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

## <a name="implement-sign-in"></a><span data-ttu-id="0bf47-111">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="0bf47-111">Implement sign-in</span></span>

<span data-ttu-id="0bf47-112">**/Graphtutorial/src/main/java/com/contoso**という名前のディレクトリに新しいファイルを作成し、次のコードを**追加します**。</span><span class="sxs-lookup"><span data-stu-id="0bf47-112">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Authentication.java** and add the following code.</span></span>

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
    private final static String authority = "https://login.microsoftonline.com/common/";

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

<span data-ttu-id="0bf47-113">**App.xaml**で、次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、アクセストークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-113">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

<span data-ttu-id="0bf47-114">コメントの`// Display access token`後に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-114">Then add the following line after the `// Display access token` comment.</span></span>

```java
System.out.println("Access token: " + accessToken);
```

<span data-ttu-id="0bf47-115">アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-115">Build and run the app.</span></span> <span data-ttu-id="0bf47-116">アプリケーションには、URL とデバイスコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0bf47-116">The application displays a URL and device code.</span></span>

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

<span data-ttu-id="0bf47-117">ブラウザーを開き、表示されている URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-117">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="0bf47-118">提供されたコードを入力し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="0bf47-118">Enter the provided code and sign in.</span></span> <span data-ttu-id="0bf47-119">完了したら、アプリケーションに戻り、1を選択し**ます。** アクセストークンを表示するためのアクセストークンオプションを表示します。</span><span class="sxs-lookup"><span data-stu-id="0bf47-119">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
