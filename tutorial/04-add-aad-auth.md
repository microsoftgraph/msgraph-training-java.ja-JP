---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612062"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。 これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。 この手順では、 [Java 用 Microsoft 認証ライブラリ (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java)をアプリケーションに統合します。

1. **/Src/main/resources**ディレクトリに、 **graphtutorial**という名前の新しいディレクトリを作成します。

1. **/Src/main/resources/graphtutorial**ディレクトリに**oAuth. プロパティ**という名前の新しいファイルを作成し、そのファイルに次のテキストを追加します。

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    を`YOUR_APP_ID_HERE` Azure ポータルで作成したアプリケーション ID に置き換えます。

    > [!IMPORTANT]
    > Git などのソース管理を使用している場合は、アプリ ID を誤ってリークしないように、ソース管理から**oAuth**ファイルを除外することをお勧めします。

1. **App.xaml**を開き、次`import`のステートメントを追加します。

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. 次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、 **oAuth プロパティ**ファイルを読み込みます。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>サインインの実装

1. **/Graphtutorial/src/main/java/graphtutorial**という名前のディレクトリに新しいファイルを作成し、次のコードを**追加します**。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. **App.xaml**で、次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、アクセストークンを取得します。

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. コメントの`// Display access token`後に次の行を追加します。

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. アプリを実行します。 アプリケーションには、URL とデバイスコードが表示されます。

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. ブラウザーを開き、表示されている URL を参照します。 提供されたコードを入力し、サインインします。 完了したら、アプリケーションに戻り、1を選択し**ます。** アクセストークンを表示するためのアクセストークンオプションを表示します。
