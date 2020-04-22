---
ms.openlocfilehash: 381e4166f07e1dbc51c072645f17002e43f6cc16
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612020"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b5085-101">このセクションでは、基本的な Java コンソールアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b5085-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="b5085-102">プロジェクトを作成するディレクトリで、コマンドラインインターフェイス (CLI) を開きます。</span><span class="sxs-lookup"><span data-stu-id="b5085-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="b5085-103">次のコマンドを実行して、新しい Gradle プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b5085-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="b5085-104">プロジェクトが作成されたら、次のコマンドを実行して、CLI でアプリを実行し、動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="b5085-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="b5085-105">動作している場合は、アプリ`Hello World.`が出力されます。</span><span class="sxs-lookup"><span data-stu-id="b5085-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="b5085-106">依存関係のインストール</span><span class="sxs-lookup"><span data-stu-id="b5085-106">Install dependencies</span></span>

<span data-ttu-id="b5085-107">に進む前に、後で使用する追加の依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="b5085-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="b5085-108">ユーザーを認証し、アクセストークンを取得するため[の、Java の Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) 。</span><span class="sxs-lookup"><span data-stu-id="b5085-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="b5085-109">Microsoft graph [SDK For Java を使用](https://github.com/microsoftgraph/msgraph-sdk-java)して、microsoft graph を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b5085-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="b5085-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)は、msal からのログ出力を抑制します。</span><span class="sxs-lookup"><span data-stu-id="b5085-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="b5085-111">/Build.gradle を開きます **。**</span><span class="sxs-lookup"><span data-stu-id="b5085-111">Open **./build.gradle**.</span></span> <span data-ttu-id="b5085-112">セクションを`dependencies`更新して、それらの依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="b5085-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="b5085-113">以下を **/build.gradle**の末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="b5085-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="b5085-114">次回プロジェクトをビルドするときに、Gradle はそれらの依存関係をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b5085-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="b5085-115">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="b5085-115">Design the app</span></span>

1. <span data-ttu-id="b5085-116">**./Src/main/java/graphtutorial/App.java**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b5085-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="b5085-117">これは基本的なメニューを実装し、コマンドラインからユーザーの選択を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="b5085-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
