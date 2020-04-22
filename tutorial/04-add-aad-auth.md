---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612062"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1c52e-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="1c52e-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="1c52e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="1c52e-103">この手順では、 [Java 用 Microsoft 認証ライブラリ (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java)をアプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="1c52e-104">**/Src/main/resources**ディレクトリに、 **graphtutorial**という名前の新しいディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="1c52e-105">**/Src/main/resources/graphtutorial**ディレクトリに**oAuth. プロパティ**という名前の新しいファイルを作成し、そのファイルに次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="1c52e-106">を`YOUR_APP_ID_HERE` Azure ポータルで作成したアプリケーション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1c52e-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1c52e-107">Git などのソース管理を使用している場合は、アプリ ID を誤ってリークしないように、ソース管理から**oAuth**ファイルを除外することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1c52e-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="1c52e-108">**App.xaml**を開き、次`import`のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="1c52e-109">次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、 **oAuth プロパティ**ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="1c52e-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="1c52e-110">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="1c52e-110">Implement sign-in</span></span>

1. <span data-ttu-id="1c52e-111">**/Graphtutorial/src/main/java/graphtutorial**という名前のディレクトリに新しいファイルを作成し、次のコードを**追加します**。</span><span class="sxs-lookup"><span data-stu-id="1c52e-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="1c52e-112">**App.xaml**で、次のコードを行の`Scanner input = new Scanner(System.in);`直前に追加して、アクセストークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="1c52e-113">コメントの`// Display access token`後に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="1c52e-114">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-114">Run the app.</span></span> <span data-ttu-id="1c52e-115">アプリケーションには、URL とデバイスコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c52e-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="1c52e-116">ブラウザーを開き、表示されている URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="1c52e-117">提供されたコードを入力し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="1c52e-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="1c52e-118">完了したら、アプリケーションに戻り、1を選択し**ます。** アクセストークンを表示するためのアクセストークンオプションを表示します。</span><span class="sxs-lookup"><span data-stu-id="1c52e-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
