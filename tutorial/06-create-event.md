---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661139"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c77d7-101">このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="c77d7-102">**./graphtu graphl/src/main/java/graphtu graph graphl/Graph.java** を開き **、Graph** クラスに次の関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="c77d7-103">**./graphtu graphl/src/main/java/graphtu graph graphl/App.java** を開き、次の関数を App クラスに **追加** します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="c77d7-104">この関数は、ユーザーに件名、出席者、開始、終了、本文を求めるメッセージを表示し、それらの値を使用して呼び出します `Graph.createEvent` 。</span><span class="sxs-lookup"><span data-stu-id="c77d7-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="c77d7-105">関数のコメントの直後に `// Create a new event` 次を追加 `Main` します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="c77d7-106">すべての変更を保存し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="c77d7-107">[イベントの **追加] オプションを選択** します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="c77d7-108">プロンプトに応答して、ユーザーの予定表に新しいイベントを作成します。</span><span class="sxs-lookup"><span data-stu-id="c77d7-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
