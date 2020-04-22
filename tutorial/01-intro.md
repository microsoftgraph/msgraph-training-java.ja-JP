---
ms.openlocfilehash: f77f0518aa22d015cf5e915886f8f24a487e7bab
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612076"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ff300-101">このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報を取得する Java コンソールアプリを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ff300-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="ff300-102">完成したチュートリアルをダウンロードするだけで済む場合は、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-java)をダウンロードするか、クローンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="ff300-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff300-103">前提条件</span><span class="sxs-lookup"><span data-stu-id="ff300-103">Prerequisites</span></span>

<span data-ttu-id="ff300-104">このチュートリアルを開始する前に、開発用コンピューターに[JAVA SE 開発キット (JDK)](https://java.com/en/download/faq/develop.xml)と[Gradle](https://gradle.org/)がインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff300-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="ff300-105">JDK または Maven を持っていない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="ff300-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

<span data-ttu-id="ff300-106">また、Outlook.com 上のメールボックスを持つ個人の Microsoft アカウント、または Microsoft 職場または学校のアカウントを所有している必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff300-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="ff300-107">Microsoft アカウントを持っていない場合は、無料のアカウントを取得するためのオプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="ff300-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="ff300-108">[新しい個人用 Microsoft アカウントにサインアップ](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)することができます。</span><span class="sxs-lookup"><span data-stu-id="ff300-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="ff300-109">[Office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="ff300-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="ff300-110">このチュートリアルは、OpenJDK バージョン14.0.0.36 および Gradle 6.3 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="ff300-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="ff300-111">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。</span><span class="sxs-lookup"><span data-stu-id="ff300-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="ff300-112">フィードバック</span><span class="sxs-lookup"><span data-stu-id="ff300-112">Feedback</span></span>

<span data-ttu-id="ff300-113">このチュートリアルに関するフィードバックは、 [GitHub リポジトリ](https://github.com/microsoftgraph/msgraph-training-java)に記入してください。</span><span class="sxs-lookup"><span data-stu-id="ff300-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
