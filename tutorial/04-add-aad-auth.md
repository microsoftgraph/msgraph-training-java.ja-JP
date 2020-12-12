---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661068"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、前の演習のアプリケーションを拡張して、Azure AD での認証をサポートします。 これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。 この手順では、Microsoft [Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) アプリケーションに統合します。

1. ./src/main/resources ディレクトリに **graphtu読み** 込 **みという名前の新しいディレクトリを作成** します。

1. **oAuth.properties****という名前の ./src/main/resources/graphtutorial** ディレクトリに新しいファイルを作成し、そのファイルに次のテキストを追加します。 `YOUR_APP_ID_HERE`Azure portal で作成したアプリケーション ID に置き換える。

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    値には、 `app.scopes` アプリケーションが必要とするアクセス許可スコープが含まれる。

    - **User.Read を** 使うと、アプリはユーザーのプロファイルにアクセスできます。
    - **MailboxSettings.Read** を使用すると、アプリはユーザーのメールボックスの設定 (ユーザーの構成されたタイム ゾーンを含む) にアクセスできます。
    - **Calendars.ReadWrite** を使うと、アプリはユーザーの予定表を一覧表示し、予定表に新しいイベントを追加できます。

    > [!IMPORTANT]
    > git などのソース管理を使用している場合は **、oAuth.properties** ファイルをソース管理から除外して、アプリ ID が誤って漏洩しないようにする良い時期です。

1. **App.java を開** き、次のステートメント `import` を追加します。

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. oAuth.properties ファイルを読み込む行の直前に次の `Scanner input = new Scanner(System.in);` **コードを追加** します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>サインインの実装

1. **Authentication.java** という名前の **./graphtu graphl/src/main/java/graphtutu graphl ディレクトリ** に新しいファイルを作成し、次のコードを追加します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. **App.java で、** 行の直前に次のコードを追加して、アクセス `Scanner input = new Scanner(System.in);` トークンを取得します。

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. コメントの後に次の行を追加 `// Display access token` します。

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. アプリを実行します。 アプリケーションは URL とデバイス コードを表示します。

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. ブラウザーを開き、表示される URL を参照します。 指定されたコードを入力し、サインインします。 完了したら、アプリケーションに戻り **、1 を選択します。アクセス トークンを表示** するアクセス トークン オプションを表示します。

> [!TIP]
> Microsoft の仕事用または学校アカウントのアクセス トークンは、トラブルシューティングの目的で解析できます [https://jwt.ms](https://jwt.ms) 。 個人の Microsoft アカウントのアクセス トークンは独自の形式を使用し、解析できません。
