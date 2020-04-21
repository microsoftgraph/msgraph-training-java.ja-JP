---
ms.openlocfilehash: 381e4166f07e1dbc51c072645f17002e43f6cc16
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612020"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、基本的な Java コンソールアプリを作成します。

1. プロジェクトを作成するディレクトリで、コマンドラインインターフェイス (CLI) を開きます。 次のコマンドを実行して、新しい Gradle プロジェクトを作成します。

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. プロジェクトが作成されたら、次のコマンドを実行して、CLI でアプリを実行し、動作を確認します。

    ```Shell
    ./gradlew --console plain run
    ```

    動作している場合は、アプリ`Hello World.`が出力されます。

## <a name="install-dependencies"></a>依存関係のインストール

に進む前に、後で使用する追加の依存関係を追加します。

- ユーザーを認証し、アクセストークンを取得するため[の、Java の Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) 。
- Microsoft graph [SDK For Java を使用](https://github.com/microsoftgraph/msgraph-sdk-java)して、microsoft graph を呼び出すことができます。
- [SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)は、msal からのログ出力を抑制します。

1. /Build.gradle を開きます **。** セクションを`dependencies`更新して、それらの依存関係を追加します。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. 以下を **/build.gradle**の末尾に追加します。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

次回プロジェクトをビルドするときに、Gradle はそれらの依存関係をダウンロードします。

## <a name="design-the-app"></a>アプリを設計する

1. **./Src/main/java/graphtutorial/App.java**ファイルを開き、その内容を次のように置き換えます。

    ```java
    package graphtutorial;

    import java.util.InputMismatchException;
    import java.util.Scanner;

    /**
     * Graph Tutorial
     *
     */
    public class App {
        public static void main(String[] args) {
            System.out.println("Java Graph Tutorial");
            System.out.println();

            Scanner input = new Scanner(System.in);

            int choice = -1;

            while (choice != 0) {
                System.out.println("Please choose one of the following options:");
                System.out.println("0. Exit");
                System.out.println("1. Display access token");
                System.out.println("2. List calendar events");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                    input.nextLine();
                }

                // Process user choice
                switch(choice) {
                    case 0:
                        // Exit the program
                        System.out.println("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    これは基本的なメニューを実装し、コマンドラインからユーザーの選択を読み取ります。
