---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612069"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込みます。 このアプリケーションでは、microsoft graph [SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java)を使用して microsoft graph を呼び出すことにします。

## <a name="implement-an-authentication-provider"></a>認証プロバイダを実装する

Microsoft Graph SDK for Java を使用するには、 `IAuthenticationProvider`その`GraphServiceClient`オブジェクトをインスタンス化するためのインターフェイスを実装する必要があります。

1. **Simpleauthprovider**という名前の **/graphtutorial/src/main/java/graphtutorial**ディレクトリに新しいファイルを作成し、次のコードを追加します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>ユーザーの詳細情報を取得する

1. **/Graphtutorial/src/main/java/graphtutorial**ディレクトリに新しいファイルを作成し、次のコードを**追加します**。

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

1. 次`import`のステートメントを**app.xaml**の先頭に追加します。

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. 次のコードを**app.xaml** `Scanner input = new Scanner(System.in);`の直前に追加して、ユーザーを取得し、ユーザーの表示名を出力します。

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. アプリを実行します。 ログインした後、アプリは名前で歓迎されます。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

1. ユーザーの予定表からイベント`Graph`を取得するには、次の関数を**Graph**のクラスに追加します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

このコードの実行内容を考えましょう。

- 呼び出される URL は `/me/events` です。
- 関数`select`は、各イベントに対して返されるフィールドを、アプリが実際に使用しているものだけに制限します。
- は`QueryOption` 、作成された日付と時刻で結果を並べ替え、最新のアイテムを最初に表示するために使用されます。

## <a name="display-the-results"></a>結果の表示

1. 次`import`のステートメントを**app.xaml**に追加します。

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. 次の関数を`App`クラスに追加して、Microsoft Graph の[dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)プロパティをユーザーフレンドリな形式に書式設定します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. 次の関数を`App`クラスに追加して、ユーザーのイベントを取得し、コンソールに出力します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. `main`関数のコメントの`// List the calendar`直後に以下を追加します。

    ```java
    listCalendarEvents(accessToken);
    ```

1. すべての変更を保存し、アプリをビルドして実行します。 [**カレンダーイベントのリスト**] オプションを選択して、ユーザーのイベントの一覧を表示します。

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
