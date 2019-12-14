---
ms.openlocfilehash: 377db05d9b238d3f6f6e45032f0b85b9217df3db
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018835"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。 これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。 この手順では、 [Java 用 Microsoft 認証ライブラリ (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java)をアプリケーションに統合します。

**/Graphtutorial/src/main**ディレクトリに、 **resources**という名前の新しいディレクトリを作成します。 その後、 **resources**ディレクトリに**com**という名前の新しいディレクトリを作成し、 **com**ディレクトリに**contoso**という名前の新しいディレクトリを作成します。 最後に、 **/graphtutorial/src/main/resources/com/contoso**ディレクトリに**oAuth. プロパティ**という名前の新しいファイルを作成し、そのファイルに次のテキストを追加します。

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

を`YOUR_APP_ID_HERE` Azure ポータルで作成したアプリケーション ID に置き換えます。

> [!IMPORTANT]
> Git などのソース管理を使用している場合は、アプリ ID を誤ってリークしないように、ソース管理から**oAuth**ファイルを除外することをお勧めします。

**App.xaml**を開き、次`import`のステートメントを追加します。

```java
import java.io.IOException;
import java.util.Properties;
```

その後、次のコードを行`Scanner input = new Scanner(System.in);`の直前に追加して、 **oAuth プロパティ**ファイルを読み込みます。

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

## <a name="implement-sign-in"></a>サインインの実装

**/Graphtutorial/src/main/java/com/contoso**という名前のディレクトリに新しいファイルを作成し、次のコードを**追加します**。

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

**App.xaml**で、次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、アクセストークンを取得します。

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

コメントの`// Display access token`後に次の行を追加します。

```java
System.out.println("Access token: " + accessToken);
```

アプリをビルドして実行します。 アプリケーションには、URL とデバイスコードが表示されます。

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

ブラウザーを開き、表示されている URL を参照します。 提供されたコードを入力し、サインインします。 完了したら、アプリケーションに戻り、1を選択し**ます。** アクセストークンを表示するためのアクセストークンオプションを表示します。
