---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661068"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d325a-101">この演習では、前の演習のアプリケーションを拡張して、Azure AD での認証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d325a-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="d325a-102">これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="d325a-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="d325a-103">この手順では、Microsoft [Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) アプリケーションに統合します。</span><span class="sxs-lookup"><span data-stu-id="d325a-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="d325a-104">./src/main/resources ディレクトリに **graphtu読み** 込 **みという名前の新しいディレクトリを作成** します。</span><span class="sxs-lookup"><span data-stu-id="d325a-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="d325a-105">**oAuth.properties\*\*\*\*という名前の ./src/main/resources/graphtutorial** ディレクトリに新しいファイルを作成し、そのファイルに次のテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="d325a-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="d325a-106">`YOUR_APP_ID_HERE`Azure portal で作成したアプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="d325a-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="d325a-107">値には、 `app.scopes` アプリケーションが必要とするアクセス許可スコープが含まれる。</span><span class="sxs-lookup"><span data-stu-id="d325a-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="d325a-108">**User.Read を** 使うと、アプリはユーザーのプロファイルにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d325a-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="d325a-109">**MailboxSettings.Read** を使用すると、アプリはユーザーのメールボックスの設定 (ユーザーの構成されたタイム ゾーンを含む) にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d325a-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="d325a-110">**Calendars.ReadWrite** を使うと、アプリはユーザーの予定表を一覧表示し、予定表に新しいイベントを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d325a-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d325a-111">git などのソース管理を使用している場合は **、oAuth.properties** ファイルをソース管理から除外して、アプリ ID が誤って漏洩しないようにする良い時期です。</span><span class="sxs-lookup"><span data-stu-id="d325a-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="d325a-112">**App.java を開** き、次のステートメント `import` を追加します。</span><span class="sxs-lookup"><span data-stu-id="d325a-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="d325a-113">oAuth.properties ファイルを読み込む行の直前に次の `Scanner input = new Scanner(System.in);` **コードを追加** します。</span><span class="sxs-lookup"><span data-stu-id="d325a-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="d325a-114">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="d325a-114">Implement sign-in</span></span>

1. <span data-ttu-id="d325a-115">**Authentication.java** という名前の **./graphtu graphl/src/main/java/graphtutu graphl ディレクトリ** に新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d325a-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="d325a-116">**App.java で、** 行の直前に次のコードを追加して、アクセス `Scanner input = new Scanner(System.in);` トークンを取得します。</span><span class="sxs-lookup"><span data-stu-id="d325a-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="d325a-117">コメントの後に次の行を追加 `// Display access token` します。</span><span class="sxs-lookup"><span data-stu-id="d325a-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="d325a-118">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="d325a-118">Run the app.</span></span> <span data-ttu-id="d325a-119">アプリケーションは URL とデバイス コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="d325a-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="d325a-120">ブラウザーを開き、表示される URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="d325a-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="d325a-121">指定されたコードを入力し、サインインします。</span><span class="sxs-lookup"><span data-stu-id="d325a-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="d325a-122">完了したら、アプリケーションに戻り **、1 を選択します。アクセス トークンを表示** するアクセス トークン オプションを表示します。</span><span class="sxs-lookup"><span data-stu-id="d325a-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="d325a-123">Microsoft の仕事用または学校アカウントのアクセス トークンは、トラブルシューティングの目的で解析できます [https://jwt.ms](https://jwt.ms) 。</span><span class="sxs-lookup"><span data-stu-id="d325a-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="d325a-124">個人の Microsoft アカウントのアクセス トークンは独自の形式を使用し、解析できません。</span><span class="sxs-lookup"><span data-stu-id="d325a-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
