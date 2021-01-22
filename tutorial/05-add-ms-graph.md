---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919530"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="af287-101">この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="af287-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="af287-102">このアプリケーションでは [、Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Javaを使用して Microsoft Graph を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="af287-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="af287-103">認証プロバイダーを実装する</span><span class="sxs-lookup"><span data-stu-id="af287-103">Implement an authentication provider</span></span>

<span data-ttu-id="af287-104">Microsoft Graph SDK for Javaオブジェクトをインスタンス化するには、インターフェイス `IAuthenticationProvider` の実装が必要 `GraphServiceClient` です。</span><span class="sxs-lookup"><span data-stu-id="af287-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="af287-105">**SimpleAuthProvider.java** **という名前の ./graphtu thel/src/main/java/graphtutul** ディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="af287-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="af287-106">ユーザーの詳細情報を取得する</span><span class="sxs-lookup"><span data-stu-id="af287-106">Get user details</span></span>

1. <span data-ttu-id="af287-107">**Graph.java** という名前の **./graphtu graphl/src/main/java/graphtutu graphl** ディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="af287-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

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
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="af287-108">`import`App.java の上部に次の **ステートメントを追加します**。</span><span class="sxs-lookup"><span data-stu-id="af287-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="af287-109">次のコードを **App.java 行** の直前に追加して、ユーザーを取得し、ユーザーの表示 `Scanner input = new Scanner(System.in);` 名を出力します。</span><span class="sxs-lookup"><span data-stu-id="af287-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="af287-110">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="af287-110">Run the app.</span></span> <span data-ttu-id="af287-111">アプリにログインすると、名前でようこそ。</span><span class="sxs-lookup"><span data-stu-id="af287-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="af287-112">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="af287-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="af287-113">Graph.java のクラスに次 `Graph` の関数を **追加** して、ユーザーの予定表からイベントを取得します。</span><span class="sxs-lookup"><span data-stu-id="af287-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="af287-114">このコードの実行内容を考えましょう。</span><span class="sxs-lookup"><span data-stu-id="af287-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="af287-115">呼び出される URL は `/me/calendarview` です。</span><span class="sxs-lookup"><span data-stu-id="af287-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="af287-116">`QueryOption` オブジェクトを使用して、カレンダー ビューの開始と終了を `startDateTime` `endDateTime` 設定するパラメーターとパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="af287-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="af287-117">オブジェクト `QueryOption` を使用してパラメーターを追加 `$orderby` し、結果を開始時刻で並べ替える。</span><span class="sxs-lookup"><span data-stu-id="af287-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="af287-118">オブジェクトを使用してヘッダーを追加し、開始時刻と終了時刻をユーザーのタイム ゾーン `HeaderOption` `Prefer: outlook.timezone` に合わせて調整します。</span><span class="sxs-lookup"><span data-stu-id="af287-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="af287-119">この `select` 関数は、各イベントで返されるフィールドを、アプリが実際に使用するフィールドに制限します。</span><span class="sxs-lookup"><span data-stu-id="af287-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="af287-120">この `top` 関数は、応答内のイベント数を最大 25 に制限します。</span><span class="sxs-lookup"><span data-stu-id="af287-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="af287-121">この関数は、現在の週に 25 を超えるイベントがある場合に結果の追加ページ `getNextPage` を要求するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="af287-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="af287-122">**GraphToIana.java** **という名前の ./graphtu graphl/src/main/java/graphtutul ディレクトリ** に新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="af287-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="af287-123">このクラスは、Windows タイム ゾーン名を IANA 識別子に変換し、Windows タイム ゾーン名に基づいて **ZoneId** を生成する簡単な参照を実装します。</span><span class="sxs-lookup"><span data-stu-id="af287-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="af287-124">結果の表示</span><span class="sxs-lookup"><span data-stu-id="af287-124">Display the results</span></span>

1. <span data-ttu-id="af287-125">`import`App.java で次の **ステートメントを追加します**。</span><span class="sxs-lookup"><span data-stu-id="af287-125">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="af287-126">次の関数をクラスに追加して、Microsoft Graph の dateTimeTimeZone プロパティをユーザー に使い分け可能な形式 `App` に書式設定します。 [](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)</span><span class="sxs-lookup"><span data-stu-id="af287-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="af287-127">次の関数をクラスに追加して、ユーザーのイベントを取得し、コンソール `App` に出力します。</span><span class="sxs-lookup"><span data-stu-id="af287-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="af287-128">関数のコメントの直後に `// List the calendar` 次を追加 `main` します。</span><span class="sxs-lookup"><span data-stu-id="af287-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="af287-129">すべての変更を保存し、アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="af287-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="af287-130">[予定表 **イベントの一覧表示]** オプションを選択して、ユーザーのイベントの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="af287-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```
