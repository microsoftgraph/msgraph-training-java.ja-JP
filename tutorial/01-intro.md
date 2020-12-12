---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661075"
---
<!-- markdownlint-disable MD002 MD041 -->

このチュートリアルでは、Microsoft Graph API を使用してJava情報を取得するカスタム コンソール アプリを構築する方法について説明します。

> [!TIP]
> 完成したチュートリアルをダウンロードする場合は [、GitHub](https://github.com/microsoftgraph/msgraph-training-java)リポジトリをダウンロードまたは複製できます。

## <a name="prerequisites"></a>前提条件

このチュートリアルを開始する前に、Java SE 開発キット [(JDK)](https://java.com/en/download/faq/develop.xml) と [Gradle](https://gradle.org/) が開発用コンピューターにインストールされている必要があります。 JDK または Gradle がない場合は、ダウンロード オプションに関する上記のリンクを参照してください。

また、メールボックスを持つ個人用の Microsoft アカウントが Outlook.com Microsoft の仕事用アカウントまたは学校アカウントである必要があります。 Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。

- 新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。

> [!NOTE]
> このチュートリアルは、OpenJDK バージョン 14.0.0.36 および Gradle 6.7.1 で記述されています。 このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません。

## <a name="feedback"></a>フィードバック

このチュートリアルに関するフィードバックは [、GitHub リポジトリで提供してください](https://github.com/microsoftgraph/msgraph-training-java)。
