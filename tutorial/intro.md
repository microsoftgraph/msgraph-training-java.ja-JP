---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634781"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="158bc-101">このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する Java コンソールアプリを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="158bc-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="158bc-102">完成したチュートリアルをダウンロードするだけで済む場合は、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-java)をダウンロードするか、クローンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="158bc-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="158bc-103">前提条件</span><span class="sxs-lookup"><span data-stu-id="158bc-103">Prerequisites</span></span>

<span data-ttu-id="158bc-104">このチュートリアルを開始する前に、開発用のコンピューターに[JAVA SE 開発キット (JDK)](https://java.com/en/download/faq/develop.xml)と[Maven](https://maven.apache.org/)がインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="158bc-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="158bc-105">JDK または Maven を持っていない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="158bc-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="158bc-106">このチュートリアルは、OpenJDK バージョン12.0.1 および Maven 3.6.1 を使用して記述されています。</span><span class="sxs-lookup"><span data-stu-id="158bc-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="158bc-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。</span><span class="sxs-lookup"><span data-stu-id="158bc-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="158bc-108">フィードバック</span><span class="sxs-lookup"><span data-stu-id="158bc-108">Feedback</span></span>

<span data-ttu-id="158bc-109">このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-java)に記入してください。</span><span class="sxs-lookup"><span data-stu-id="158bc-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
