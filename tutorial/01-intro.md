---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661075"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="dd1a8-101">このチュートリアルでは、Microsoft Graph API を使用してJava情報を取得するカスタム コンソール アプリを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="dd1a8-102">完成したチュートリアルをダウンロードする場合は [、GitHub](https://github.com/microsoftgraph/msgraph-training-java)リポジトリをダウンロードまたは複製できます。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd1a8-103">前提条件</span><span class="sxs-lookup"><span data-stu-id="dd1a8-103">Prerequisites</span></span>

<span data-ttu-id="dd1a8-104">このチュートリアルを開始する前に、Java SE 開発キット [(JDK)](https://java.com/en/download/faq/develop.xml) と [Gradle](https://gradle.org/) が開発用コンピューターにインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="dd1a8-105">JDK または Gradle がない場合は、ダウンロード オプションに関する上記のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span>

<span data-ttu-id="dd1a8-106">また、メールボックスを持つ個人用の Microsoft アカウントが Outlook.com Microsoft の仕事用アカウントまたは学校アカウントである必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="dd1a8-107">Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="dd1a8-108">新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="dd1a8-109">Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1a8-110">このチュートリアルは、OpenJDK バージョン 14.0.0.36 および Gradle 6.7.1 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="dd1a8-111">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="dd1a8-112">フィードバック</span><span class="sxs-lookup"><span data-stu-id="dd1a8-112">Feedback</span></span>

<span data-ttu-id="dd1a8-113">このチュートリアルに関するフィードバックは [、GitHub リポジトリで提供してください](https://github.com/microsoftgraph/msgraph-training-java)。</span><span class="sxs-lookup"><span data-stu-id="dd1a8-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
