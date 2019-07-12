---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634760"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3928d-101">プロジェクトを作成するディレクトリで、コマンドラインインターフェイス (CLI) を開きます。</span><span class="sxs-lookup"><span data-stu-id="3928d-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="3928d-102">次のコマンドを実行して、新しい Maven プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3928d-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="3928d-103">上記で指定した値よりも、グループ`DgroupId` ID (パラメーター) と成果`DartifactId`物 ID (パラメーター) に異なる値を入力できます。</span><span class="sxs-lookup"><span data-stu-id="3928d-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="3928d-104">このチュートリアルのサンプルコードは、グループ ID `com.contoso`が使用されていることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="3928d-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="3928d-105">別の値を使用する場合は、サンプルコード`com.contoso`で必ずグループ ID を置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="3928d-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="3928d-106">プロンプトが表示されたら、構成を確認し、プロジェクトが作成されるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="3928d-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="3928d-107">プロジェクトが作成されたら、次のコマンドを実行して、CLI でアプリをパッケージ化して実行し、動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="3928d-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="3928d-108">動作している場合は、アプリ`Hello World!`が出力されます。</span><span class="sxs-lookup"><span data-stu-id="3928d-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="3928d-109">に進む前に、後で使用する追加の依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="3928d-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="3928d-110">ユーザーを認証し、アクセストークンを取得するため[の、Java の Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) 。</span><span class="sxs-lookup"><span data-stu-id="3928d-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="3928d-111">Microsoft graph [SDK For Java を使用](https://github.com/microsoftgraph/msgraph-sdk-java)して、microsoft graph を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="3928d-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="3928d-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)は、msal からのログ出力を抑制します。</span><span class="sxs-lookup"><span data-stu-id="3928d-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="3928d-113">/Graphtutorial/pom.xml を開きます **。**</span><span class="sxs-lookup"><span data-stu-id="3928d-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="3928d-114">`<dependencies>`要素内に次のように追加します。</span><span class="sxs-lookup"><span data-stu-id="3928d-114">Add the following inside the `<dependencies>` element.</span></span>

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.4.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>0.4.0-preview</version>
</dependency>
```

<span data-ttu-id="3928d-115">次回プロジェクトを構築したときに、Maven はそれらの依存関係をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3928d-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="3928d-116">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="3928d-116">Design the app</span></span>

<span data-ttu-id="3928d-117">**./Graphtutorial/src/main/java/com/contoso/App.java**ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3928d-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

```java
package com.contoso;

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

<span data-ttu-id="3928d-118">これは基本的なメニューを実装し、コマンドラインからユーザーの選択を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="3928d-118">This implements a basic menu and reads the user's choice from the command line.</span></span>
