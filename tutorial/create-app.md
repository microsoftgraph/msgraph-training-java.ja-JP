---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634760"
---
<!-- markdownlint-disable MD002 MD041 -->

プロジェクトを作成するディレクトリで、コマンドラインインターフェイス (CLI) を開きます。 次のコマンドを実行して、新しい Maven プロジェクトを作成します。

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> 上記で指定した値よりも、グループ`DgroupId` ID (パラメーター) と成果`DartifactId`物 ID (パラメーター) に異なる値を入力できます。 このチュートリアルのサンプルコードは、グループ ID `com.contoso`が使用されていることを前提としています。 別の値を使用する場合は、サンプルコード`com.contoso`で必ずグループ ID を置き換えてください。

プロンプトが表示されたら、構成を確認し、プロジェクトが作成されるまで待機します。 プロジェクトが作成されたら、次のコマンドを実行して、CLI でアプリをパッケージ化して実行し、動作を確認します。

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

動作している場合は、アプリ`Hello World!`が出力されます。 に進む前に、後で使用する追加の依存関係を追加します。

- ユーザーを認証し、アクセストークンを取得するため[の、Java の Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) 。
- Microsoft graph [SDK For Java を使用](https://github.com/microsoftgraph/msgraph-sdk-java)して、microsoft graph を呼び出すことができます。
- [SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)は、msal からのログ出力を抑制します。

/Graphtutorial/pom.xml を開きます **。** `<dependencies>`要素内に次のように追加します。

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

次回プロジェクトを構築したときに、Maven はそれらの依存関係をダウンロードします。

## <a name="design-the-app"></a>アプリを設計する

**./Graphtutorial/src/main/java/com/contoso/App.java**ファイルを開き、その内容を次のように置き換えます。

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

これは基本的なメニューを実装し、コマンドラインからユーザーの選択を読み取ります。
