---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661082"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b9ba1-101">このセクションでは、基本的なコンソール アプリJava作成します。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="b9ba1-102">プロジェクトを作成するディレクトリでコマンド ライン インターフェイス (CLI) を開きます。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="b9ba1-103">次のコマンドを実行して、新しい Gradle プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="b9ba1-104">プロジェクトが作成された後、次のコマンドを実行して CLI でアプリを実行して、プロジェクトが動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="b9ba1-105">動作する場合、アプリは出力する必要があります `Hello World.` 。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="b9ba1-106">依存関係をインストールする</span><span class="sxs-lookup"><span data-stu-id="b9ba1-106">Install dependencies</span></span>

<span data-ttu-id="b9ba1-107">次に進む前に、後で使用する依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="b9ba1-108">[ユーザーを認証し、アクセス トークンJava](https://github.com/AzureAD/microsoft-authentication-library-for-java) 取得するための Microsoft 認証ライブラリ (MSAL)。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="b9ba1-109">[Microsoft Graph SDK for](https://github.com/microsoftgraph/msgraph-sdk-java) Java Microsoft Graph の呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="b9ba1-110">[MSAL からのログ記録を抑制する SLF4J NOP](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) バインド。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="b9ba1-111">**./build.gradle を開きます**。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-111">Open **./build.gradle**.</span></span> <span data-ttu-id="b9ba1-112">セクションを `dependencies` 更新して、これらの依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="b9ba1-113">**./build.gradle の末尾に次を追加します**。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="b9ba1-114">次にプロジェクトをビルドすると、Gradle はそれらの依存関係をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="b9ba1-115">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="b9ba1-115">Design the app</span></span>

1. <span data-ttu-id="b9ba1-116">**./src/main/java/graphtutorial/App.java** ファイルを開き、その内容を次のように置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="b9ba1-117">これにより、基本的なメニューが実装され、コマンド ラインからユーザーの選択が読み取ります。</span><span class="sxs-lookup"><span data-stu-id="b9ba1-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
