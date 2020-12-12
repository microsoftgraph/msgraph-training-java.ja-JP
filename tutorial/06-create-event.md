---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661139"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。

1. **./graphtu graphl/src/main/java/graphtu graph graphl/Graph.java** を開き **、Graph** クラスに次の関数を追加します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. **./graphtu graphl/src/main/java/graphtu graph graphl/App.java** を開き、次の関数を App クラスに **追加** します。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    この関数は、ユーザーに件名、出席者、開始、終了、本文を求めるメッセージを表示し、それらの値を使用して呼び出します `Graph.createEvent` 。

1. 関数のコメントの直後に `// Create a new event` 次を追加 `Main` します。

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. すべての変更を保存し、アプリを実行します。 [イベントの **追加] オプションを選択** します。 プロンプトに応答して、ユーザーの予定表に新しいイベントを作成します。
