---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661054"
---
# <a name="how-to-run-the-completed-project"></a>完成したプロジェクトを実行する方法

## <a name="prerequisites"></a>前提条件

このフォルダーで完成したプロジェクトを実行するには、以下が必要です。

- [Javaコンピューターにインストールされている SE 開発キット (JDK)](https://java.com/en/download/faq/develop.xml) と [Gradle](https://gradle.org/) をインストールします。 JDK または Gradle がない場合は、ダウンロード オプションに関する上記のリンクを参照してください。 (**注:** このチュートリアルは、OpenJDK バージョン 14.0.0.36 および Gradle 6.7.1 で記述されています。 このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。
- Microsoft の仕事用または学校用のアカウント。

Microsoft アカウントをお持ちでない場合は [、Office 365 開発者](https://developer.microsoft.com/office/dev-program) プログラムにサインアップして、無料の Office 365 サブスクリプションを取得できます。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Azure Active Directory 管理センターに Web アプリケーションを登録する

1. ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。 **個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。

1. 左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。

    ![アプリの登録のスクリーンショット ](/tutorial/images/aad-portal-app-registrations.png)

1. **[新規登録]** を選択します。 **[アプリケーションを登録]** ページで、次のように値を設定します。

    - `Java Graph Tutorial` に **[名前]** を設定します。
    - サポート **されているアカウントの種類を任意** の **組織のディレクトリのアカウントに設定します**。
    - **[リダイレクト URI]** を空のままにします。

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/aad-register-an-app.png)

1. **[登録]** を選択します。 [ **Java Graph チュートリアル** ] ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/aad-application-id.png)

1. [リダイレクト **URI の追加] リンクを選択** します。 [リダイレクト **URI]** ページで、パブリック クライアント (モバイル、デスクトップ) の推奨リダイレクト **URI セクションを探** します。 URI を選択 `https://login.microsoftonline.com/common/oauth2/nativeclient` します。

    ![[リダイレクト URI] ページのスクリーンショット](/tutorial/images/aad-redirect-uris.png)

1. [既定の **クライアントの種類] セクションを見** つけて、[アプリケーションをパブリッククライアントとして扱う **]** トグルを [はい] に変更し、[保存] を選択 **します**。

    ![[既定のクライアントの種類] セクションのスクリーンショット](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>サンプルを構成する

1. ファイルの名前 `oAuth.properties.example` を変更します `oAuth.properties` 。
1. ファイルを `oAuth.properties` 編集し、次の変更を行います。
    1. アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。

## <a name="build-and-run-the-sample"></a>サンプルの構築と実行

コマンド ライン インターフェイス (CLI) で、プロジェクト ディレクトリに移動し、次のコマンドを実行します。

```Shell
./gradlew --console plain run
```
