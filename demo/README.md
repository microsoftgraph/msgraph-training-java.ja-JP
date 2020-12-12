---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661054"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="e17e4-101">完成したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="e17e4-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e17e4-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="e17e4-102">Prerequisites</span></span>

<span data-ttu-id="e17e4-103">このフォルダーで完成したプロジェクトを実行するには、以下が必要です。</span><span class="sxs-lookup"><span data-stu-id="e17e4-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="e17e4-104">[Javaコンピューターにインストールされている SE 開発キット (JDK)](https://java.com/en/download/faq/develop.xml) と [Gradle](https://gradle.org/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e17e4-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="e17e4-105">JDK または Gradle がない場合は、ダウンロード オプションに関する上記のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e17e4-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="e17e4-106">(**注:** このチュートリアルは、OpenJDK バージョン 14.0.0.36 および Gradle 6.7.1 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="e17e4-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="e17e4-107">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。</span><span class="sxs-lookup"><span data-stu-id="e17e4-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="e17e4-108">Microsoft の仕事用または学校用のアカウント。</span><span class="sxs-lookup"><span data-stu-id="e17e4-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="e17e4-109">Microsoft アカウントをお持ちでない場合は [、Office 365 開発者](https://developer.microsoft.com/office/dev-program) プログラムにサインアップして、無料の Office 365 サブスクリプションを取得できます。</span><span class="sxs-lookup"><span data-stu-id="e17e4-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="e17e4-110">Azure Active Directory 管理センターに Web アプリケーションを登録する</span><span class="sxs-lookup"><span data-stu-id="e17e4-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="e17e4-111">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="e17e4-112">**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="e17e4-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="e17e4-113">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="e17e4-114">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="e17e4-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="e17e4-115">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-115">Select **New registration**.</span></span> <span data-ttu-id="e17e4-116">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="e17e4-117">`Java Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="e17e4-118">サポート **されているアカウントの種類を任意** の **組織のディレクトリのアカウントに設定します**。</span><span class="sxs-lookup"><span data-stu-id="e17e4-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="e17e4-119">**[リダイレクト URI]** を空のままにします。</span><span class="sxs-lookup"><span data-stu-id="e17e4-119">Leave **Redirect URI** empty.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="e17e4-121">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-121">Choose **Register**.</span></span> <span data-ttu-id="e17e4-122">[ **Java Graph チュートリアル** ] ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="e17e4-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="e17e4-124">[リダイレクト **URI の追加] リンクを選択** します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="e17e4-125">[リダイレクト **URI]** ページで、パブリック クライアント (モバイル、デスクトップ) の推奨リダイレクト **URI セクションを探** します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="e17e4-126">URI を選択 `https://login.microsoftonline.com/common/oauth2/nativeclient` します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![[リダイレクト URI] ページのスクリーンショット](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="e17e4-128">[既定の **クライアントの種類] セクションを見** つけて、[アプリケーションをパブリッククライアントとして扱う **]** トグルを [はい] に変更し、[保存] を選択 **します**。</span><span class="sxs-lookup"><span data-stu-id="e17e4-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![[既定のクライアントの種類] セクションのスクリーンショット](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="e17e4-130">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="e17e4-130">Configure the sample</span></span>

1. <span data-ttu-id="e17e4-131">ファイルの名前 `oAuth.properties.example` を変更します `oAuth.properties` 。</span><span class="sxs-lookup"><span data-stu-id="e17e4-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="e17e4-132">ファイルを `oAuth.properties` 編集し、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="e17e4-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="e17e4-133">アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="e17e4-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="e17e4-134">サンプルの構築と実行</span><span class="sxs-lookup"><span data-stu-id="e17e4-134">Build and run the sample</span></span>

<span data-ttu-id="e17e4-135">コマンド ライン インターフェイス (CLI) で、プロジェクト ディレクトリに移動し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="e17e4-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
