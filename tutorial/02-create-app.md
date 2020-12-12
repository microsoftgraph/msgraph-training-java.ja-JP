---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661082"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、基本的なコンソール アプリJava作成します。

1. プロジェクトを作成するディレクトリでコマンド ライン インターフェイス (CLI) を開きます。 次のコマンドを実行して、新しい Gradle プロジェクトを作成します。

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. プロジェクトが作成された後、次のコマンドを実行して CLI でアプリを実行して、プロジェクトが動作を確認します。

    ```Shell
    ./gradlew --console plain run
    ```

    動作する場合、アプリは出力する必要があります `Hello World.` 。

## <a name="install-dependencies"></a>依存関係をインストールする

次に進む前に、後で使用する依存関係を追加します。

- [ユーザーを認証し、アクセス トークンJava](https://github.com/AzureAD/microsoft-authentication-library-for-java) 取得するための Microsoft 認証ライブラリ (MSAL)。
- [Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Java Microsoft Graph の呼び出しを行います。
- [MSAL からのログ記録を抑制する SLF4J NOP](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) バインド。

1. **./build.gradle を開きます**。 セクションを `dependencies` 更新して、これらの依存関係を追加します。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. **./build.gradle の末尾に次を追加します**。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

次にプロジェクトをビルドすると、Gradle はそれらの依存関係をダウンロードします。

## <a name="design-the-app"></a>アプリを設計する

1. **./src/main/java/graphtutorial/App.java** ファイルを開き、その内容を次のように置き換えてください。

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
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

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
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    これにより、基本的なメニューが実装され、コマンド ラインからユーザーの選択が読み取ります。
