---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-19"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入門チュートリアル
{: #getting-started}

以下のチュートリアルでは、{{site.data.keyword.cloud_notm}} 提供のツールを使用して Go アプリを作成してデプロイする手順を説明します。 コマンド・ラインで [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) を使用するか、以下のチュートリアルの手順で示すように、Web ベースの [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") を使用することができます。これらの方法のいずれかを使用して、実動用の Go アプリケーションを数分で生成できます。

## ステップ 1. ダッシュボードでのカスタム Go アプリの作成
{: #create-go-app}

1. アプリを作成するために、{{site.data.keyword.cloud_notm}} アカウントにログインします。アカウントがない場合は、[無料アカウントを登録](https://{DomainName}/registration){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") できます。
2. {{site.data.keyword.dev_console}} の「[スターター・キット](https://{DomainName}/developer/appservice/starter-kits){: new_window}![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")」ページから、次のオプションのいずれかを行います。
 * `Go` で作成されたスターター・キットを選択し、**「スターター・キットの詳細」**ページで**「アプリの作成」**をクリックします。
 * 空白のスターター・アプリを選択し、**「アプリの作成」**をクリックします。
3. アプリに名前を付けるか、提供された汎用のアプリ名を使用します。
4. プラットフォームとして**「Go」**が選択されていることを確認し、**「作成」**をクリックします。アプリを作成したら、サービスを追加してからツールチェーンを使用してデプロイするか、引き続き[コマンド・ライン](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)からプロジェクトをビルドしてからデプロイすることができます。

## ステップ 2. {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}} の追加
{: #add-resource-go}

ここで、{{site.data.keyword.watson}} サービスを Go アプリケーションに追加することができます。このチュートリアルでは、クラウド API を使用して音声入力を受け取りそれをテキストに変換する {{site.data.keyword.texttospeechshort}} サービスを Go アプリに追加します。

1. **「アプリの詳細」**ページで、**「サービスの追加」**をクリックします。
2. **「AI」**を選択し、**「次へ」**をクリックします。
3. **「{{site.data.keyword.texttospeechshort}}」**を選択し、**「次へ」**をクリックします。
4. **「作成」**をクリックします。 

このサービスがアプリケーションに追加されると、[Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") の依存関係が `Gopkg.toml` に追加され、`services/` ディレクトリーのサービスで簡単なインスツルメンテーション・コードが利用できるようになります。さらに、それぞれの環境のサービス資格情報にアクセスするための構成情報も組み込まれます。

コードをダウンロードするには、**「アプリの詳細」**ページの**「コードのダウンロード」**をクリックします。ダウンロードされたコードには、[Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン") と、いくつかの基本的な初期設定コードが含まれます。

## ステップ 3. コンソールからアプリをデプロイする
{: #deploy-go}

1. **「アプリの詳細」**ページから、**「継続的デリバリーの構成」**をクリックします。
2. 次の方法のうちの選択する方法に応じた手順に従って、デプロイメント・ターゲットをセットアップします。
  * **[IBM Kubernetes サービス](/docs/apps/deploying?topic=creating-apps-containers-kube)へのデプロイ**。このオプションでは、可用性の高いアプリケーション・コンテナーをデプロイして管理するためのワーカー・ノードと呼ばれるホストのクラスターを作成します。 クラスターを作成したり、既存のクラスターにデプロイしたりすることができます。
  * **Cloud Foundry へのデプロイ**。 このオプションでは、基礎となるインフラストラクチャーの管理を必要とせずに、クラウド・ネイティブ・アプリをデプロイします。 ご使用のアカウントで {{site.data.keyword.cfee_full_notm}} にアクセスできる場合、デプロイヤー・タイプの **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** または **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)** のいずれかを選択できます。後者を使用すれば、お客様の会社専用に Cloud Foundry アプリケーションをホストするための分離された環境を作成して管理することができます。
  * **[仮想サーバー](/docs/apps?topic=creating-apps-vsi-deploy)へのデプロイ**。このオプションでは、仮想サーバー・インスタンスをプロビジョンし、アプリが含まれたイメージをロードし、DevOps ツールチェーンを作成し、最初のデプロイメント・サイクルを自動的に開始します。

3. デプロイメント・ターゲットをセットアップしたら、**「次へ」**をクリックします。
4. 構成オプションを選択して、**「作成」**をクリックします。ツールチェーンが作成され、アプリが自動的にデプロイされます。

## ステップ 4. ローカルでの Go アプリのテストおよびアクセス
{: #run_local-go}

**「アプリの詳細」**ページでは、次のことを行えます。
* **「リポジトリーの表示 (View repo)」**をクリックしてリポジトリーを表示する。
* **「ツールチェーンの表示」**をクリックして、ツールチェーンを表示および変更する。

{{site.data.keyword.dev_cli_long}} を使用すると、`ibmcloud dev` コマンドを使用して Go アプリをローカルで処理できます。これらのツールは、簡単にローカルで繰り返し作業を行い、変更内容をクラウドにプッシュする点で役に立ちます。

1. `ibmcloud dev build` コマンドを使用して、アプリケーションをビルドします。
2. `ibmcloud dev run` コマンドを使用して、アプリケーションをローカルで実行します。 アプリケーションは、ローカル・システム上の Docker コンテナーで実行されます。 `http://localhost:8080` にアクセスして、ブラウザーでアプリケーションをテストできます。
3. ヘルス・チェック・エンドポイントは `http://localhost:8080/health` で利用できます。
4. メトリックには `http://localhost:3000/metrics` でアクセスできます。

{{site.data.keyword.dev_cli_notm}} について詳しくは、[CLI 資料](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)を参照してください。

これで、{{site.data.keyword.texttospeechshort}} サービスを使用するアプリケーションのビルドの準備ができました。

## 代替デプロイメント方式の検討
{: #alt-deploy-go notoc}

[「デプロイメント」タブ](/docs/go?topic=go-go-deploy-apps)でアプリケーションをデプロイする方法を確認してください。

Go アプリケーションを拡張するには、引き続き Go プログラミング・ガイドのトピックを確認してください。
