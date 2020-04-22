---
ms.openlocfilehash: 725648d1b8518c17938c17c160c5799258dc1d92
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612352"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="a01da-101">完了したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="a01da-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a01da-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="a01da-102">Prerequisites</span></span>

<span data-ttu-id="a01da-103">このフォルダーで完了したプロジェクトを実行するには、次のものが必要です。</span><span class="sxs-lookup"><span data-stu-id="a01da-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="a01da-104">開発用コンピューターにインストールされている[JAVA SE 開発キット (JDK)](https://java.com/en/download/faq/develop.xml)と[Gradle](https://gradle.org/) 。</span><span class="sxs-lookup"><span data-stu-id="a01da-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="a01da-105">JDK または Gradle を使用していない場合は、「ダウンロードオプション」の前のリンクにアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="a01da-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="a01da-106">(**注:** このチュートリアルは、openjdk バージョン14.0.0.36 および Gradle 6.3 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="a01da-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="a01da-107">このガイドの手順は、他のバージョンでは動作しますが、テストされていません。)</span><span class="sxs-lookup"><span data-stu-id="a01da-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="a01da-108">Microsoft 職場または学校のアカウント。</span><span class="sxs-lookup"><span data-stu-id="a01da-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="a01da-109">Microsoft アカウントを持っていない場合は、 [office 365 開発者プログラムにサインアップ](https://developer.microsoft.com/office/dev-program)して、無料の office 365 サブスクリプションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="a01da-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="a01da-110">Web アプリケーションを Azure Active Directory 管理センターに登録する</span><span class="sxs-lookup"><span data-stu-id="a01da-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="a01da-111">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。</span><span class="sxs-lookup"><span data-stu-id="a01da-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="a01da-112">**個人用アカウント** (別名: Microsoft アカウント)、または**職場/学校アカウント**を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="a01da-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="a01da-113">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a01da-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="a01da-114">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="a01da-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="a01da-115">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a01da-115">Select **New registration**.</span></span> <span data-ttu-id="a01da-116">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="a01da-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="a01da-117">`Java Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="a01da-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="a01da-118">**サポートされているアカウントの種類**を**任意の組織ディレクトリのアカウント**に設定します。</span><span class="sxs-lookup"><span data-stu-id="a01da-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="a01da-119">**[リダイレクト URI]** を空のままにします。</span><span class="sxs-lookup"><span data-stu-id="a01da-119">Leave **Redirect URI** empty.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="a01da-121">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a01da-121">Choose **Register**.</span></span> <span data-ttu-id="a01da-122">[ **Java Graph のチュートリアル**] ページで、**アプリケーション (クライアント) ID**の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="a01da-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="a01da-124">[**リダイレクト URI を追加する**] リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="a01da-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="a01da-125">[**リダイレクト uri** ] ページで、[**パブリッククライアント (モバイル、デスクトップ)] セクションの推奨されるリダイレクト uri**を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a01da-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="a01da-126">`https://login.microsoftonline.com/common/oauth2/nativeclient` URI を選択します。</span><span class="sxs-lookup"><span data-stu-id="a01da-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![リダイレクト Uri ページのスクリーンショット](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="a01da-128">[**既定のクライアントの種類**] セクションを探し、[アプリケーションを**パブリッククライアントとして扱う**] を [**はい**] に変更して、[**保存**] を選択します。</span><span class="sxs-lookup"><span data-stu-id="a01da-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![[既定のクライアントの種類] セクションのスクリーンショット](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="a01da-130">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="a01da-130">Configure the sample</span></span>

1. <span data-ttu-id="a01da-131">ファイルの`oAuth.properties.example`名前を`oAuth.properties`に変更します。</span><span class="sxs-lookup"><span data-stu-id="a01da-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="a01da-132">`oAuth.properties`ファイルを編集し、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="a01da-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="a01da-133">を`YOUR_APP_ID_HERE`アプリ登録ポータルで取得した**アプリケーション Id**に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a01da-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="a01da-134">サンプルの構築と実行</span><span class="sxs-lookup"><span data-stu-id="a01da-134">Build and run the sample</span></span>

<span data-ttu-id="a01da-135">コマンドラインインターフェイス (CLI) で、プロジェクトディレクトリに移動し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a01da-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
