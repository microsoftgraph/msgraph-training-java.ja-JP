---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612069"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="03a71-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="03a71-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="03a71-102">このアプリケーションでは、microsoft graph [SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java)を使用して microsoft graph を呼び出すことにします。</span><span class="sxs-lookup"><span data-stu-id="03a71-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="03a71-103">認証プロバイダを実装する</span><span class="sxs-lookup"><span data-stu-id="03a71-103">Implement an authentication provider</span></span>

<span data-ttu-id="03a71-104">Microsoft Graph SDK for Java を使用するには、 `IAuthenticationProvider`その`GraphServiceClient`オブジェクトをインスタンス化するためのインターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="03a71-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="03a71-105">**Simpleauthprovider**という名前の **/graphtutorial/src/main/java/graphtutorial**ディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="03a71-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="03a71-106">ユーザーの詳細情報を取得する</span><span class="sxs-lookup"><span data-stu-id="03a71-106">Get user details</span></span>

1. <span data-ttu-id="03a71-107">**/Graphtutorial/src/main/java/graphtutorial**ディレクトリに新しいファイルを作成し、次のコードを**追加します**。</span><span class="sxs-lookup"><span data-stu-id="03a71-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="03a71-108">次`import`のステートメントを**app.xaml**の先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="03a71-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="03a71-109">次のコードを**app.xaml** `Scanner input = new Scanner(System.in);`の直前に追加して、ユーザーを取得し、ユーザーの表示名を出力します。</span><span class="sxs-lookup"><span data-stu-id="03a71-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. <span data-ttu-id="03a71-110">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="03a71-110">Run the app.</span></span> <span data-ttu-id="03a71-111">ログインした後、アプリは名前で歓迎されます。</span><span class="sxs-lookup"><span data-stu-id="03a71-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="03a71-112">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="03a71-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="03a71-113">ユーザーの予定表からイベント`Graph`を取得するには、次の関数を**Graph**のクラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="03a71-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="03a71-114">このコードの実行内容を考えましょう。</span><span class="sxs-lookup"><span data-stu-id="03a71-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="03a71-115">呼び出される URL は `/me/events` です。</span><span class="sxs-lookup"><span data-stu-id="03a71-115">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="03a71-116">関数`select`は、各イベントに対して返されるフィールドを、アプリが実際に使用しているものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="03a71-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="03a71-117">は`QueryOption` 、作成された日付と時刻で結果を並べ替え、最新のアイテムを最初に表示するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="03a71-117">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="03a71-118">結果の表示</span><span class="sxs-lookup"><span data-stu-id="03a71-118">Display the results</span></span>

1. <span data-ttu-id="03a71-119">次`import`のステートメントを**app.xaml**に追加します。</span><span class="sxs-lookup"><span data-stu-id="03a71-119">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="03a71-120">次の関数を`App`クラスに追加して、Microsoft Graph の[dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)プロパティをユーザーフレンドリな形式に書式設定します。</span><span class="sxs-lookup"><span data-stu-id="03a71-120">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="03a71-121">次の関数を`App`クラスに追加して、ユーザーのイベントを取得し、コンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="03a71-121">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="03a71-122">`main`関数のコメントの`// List the calendar`直後に以下を追加します。</span><span class="sxs-lookup"><span data-stu-id="03a71-122">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="03a71-123">すべての変更を保存し、アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="03a71-123">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="03a71-124">[**カレンダーイベントのリスト**] オプションを選択して、ユーザーのイベントの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="03a71-124">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```
