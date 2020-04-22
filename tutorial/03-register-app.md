---
ms.openlocfilehash: c21adae303c65e52ec2402c56e174066f879f40b
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612087"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e606e-101">この手順では、Azure Active Directory 管理センターを使用して、新しい Azure AD アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e606e-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="e606e-102">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動して、**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="e606e-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="e606e-103">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e606e-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="e606e-104">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="e606e-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="e606e-105">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e606e-105">Select **New registration**.</span></span> <span data-ttu-id="e606e-106">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="e606e-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="e606e-107">`Java Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="e606e-107">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="e606e-108">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="e606e-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="e606e-109">[**リダイレクト URI**] で、ドロップダウンを [**パブリッククライアント (モバイル & デスクトップ)**] に変更し`https://login.microsoftonline.com/common/oauth2/nativeclient`、値をに設定します。</span><span class="sxs-lookup"><span data-stu-id="e606e-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](./images/aad-register-an-app.png)

1. <span data-ttu-id="e606e-111">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e606e-111">Choose **Register**.</span></span> <span data-ttu-id="e606e-112">[ **Java Graph のチュートリアル**] ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="e606e-112">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](./images/aad-application-id.png)

1. <span data-ttu-id="e606e-114">**[管理]** の下の **[認証]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e606e-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="e606e-115">[**詳細設定**] セクションを探し、 **[アプリケーションをパブリッククライアントとして扱う**] を [**はい**] に変更して、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="e606e-115">Locate the **Advanced settings** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![[既定のクライアントの種類] セクションのスクリーンショット](./images/aad-default-client-type.png)
