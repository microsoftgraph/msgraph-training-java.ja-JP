---
ms.openlocfilehash: 7118c8300b17adb66893324dd2f2269d9fb8d00b
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634704"
---
# <a name="completed-module-add-azure-ad-authentication"></a><span data-ttu-id="c09a4-101">完了したモジュール: Azure AD 認証を追加する</span><span class="sxs-lookup"><span data-stu-id="c09a4-101">Completed module: Add Azure AD authentication</span></span>

<span data-ttu-id="c09a4-102">このディレクトリにあるプロジェクトのバージョンは、「 [AZURE AD 認証を追加](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3)する」のチュートリアルを完了することを反映しています。</span><span class="sxs-lookup"><span data-stu-id="c09a4-102">The version of the project in this directory reflects completing the tutorial up through [Add Azure AD authentication](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3).</span></span> <span data-ttu-id="c09a4-103">このバージョンのプロジェクトを使用する場合は、「[予定表データを取得](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4)する」からチュートリアルの残りの部分を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c09a4-103">If you use this version of the project, you need to complete the rest of the tutorial starting at [Get calendar data](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4).</span></span>

> <span data-ttu-id="c09a4-104">**注:**「[アプリをポータルに登録](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2)する」で示されているように、アプリ登録ポータルに既にアプリケーションを登録していることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="c09a4-104">**Note:** It is assumed that you have already registered an application in the app registration portal as specified in [Register the app in the portal](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2).</span></span> <span data-ttu-id="c09a4-105">このバージョンのサンプルは、次のように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c09a4-105">You need to configure this version of the sample as follows:</span></span>
>
> 1. <span data-ttu-id="c09a4-106">ファイルの`oAuth.properties.example`名前を`oAuth.properties`に変更します。</span><span class="sxs-lookup"><span data-stu-id="c09a4-106">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
> 1. <span data-ttu-id="c09a4-107">`oAuth.properties`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="c09a4-107">Edit the `oAuth.properties` file and make the following changes.</span></span>
>     1. <span data-ttu-id="c09a4-108">を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c09a4-108">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
