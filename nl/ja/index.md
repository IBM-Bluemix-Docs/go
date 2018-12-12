---

copyright:
  years: 2018
lastupdated: "2018-10-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入門チュートリアル

以下のチュートリアルでは、{{site.data.keyword.cloud_notm}} 提供のツールを使用して Go アプリを作成してデプロイする手順を説明します。チュートリアルの以下の手順で示すように、コマンド・ラインまたは Web ベースの [IBM Cloud アプリ・サービス](https://console.bluemix.net/developer/appservice/dashboard)で [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) を使用できます。これらの方法のいずれかを使用して、実動用の Go アプリケーションを数分で生成できます。

## ステップ 1. ダッシュボードでの Go アプリの作成
{: #create_project}

1. アプリ・サービスの[スターター・キット](https://console.bluemix.net/developer/appservice/starter-kits)のページから、`Go` で作成されたスターター・キットを選択します。**「アプリの作成」** をクリックし、言語として `Go` を選択することによって、ブランクのスターター・アプリを作成することもできます。

    プロジェクトを作成するには、{{site.data.keyword.cloud_notm}} アカウントにログインしている必要があります。 アカウントがない場合は、[無料アカウントを登録](https://console.bluemix.net/registration)できます。
    {: tip}

3. **「アプリの作成」**をクリックします。
4. アプリに**名前**を付けるか、提供された汎用アプリ名を使用します。
5. **固有のホスト名**を入力します。 このホスト名は、アプリケーションにアクセスするために使用されます (例: `server.mybluemix.net`)。
6. **「作成」**をクリックします。 プロジェクトが作成されたら、ツールチェーンを使用してデプロイするか、引き続き[コマンド・ライン](/docs/cli/idt/index.html)からプロジェクトをビルドしてからデプロイすることができます。

## ステップ 2. ダッシュボードを使用したデプロイ
{: #deploy_dashboard}

1. 「アプリの詳細」ページで、**「クラウドにデプロイ」**をクリックします。
2. 次に、以下のいずれかのデプロイメント方式を選択します。
    * **Kubernetes にデプロイ** - ワーカー・ノードのセットを作成する必要があります。例えば、VM を使用して、可用性の高いアプリケーション・コンテナーをデプロイおよび管理できます。 クラスターを作成したり、既存のクラスターにデプロイしたりすることができます。
    * **Cloud Foundry にデプロイ** - 基礎となるインフラストラクチャーを管理する必要はありません。
    * **仮想サーバーにデプロイ (ベータ)** - 仮想サーバーにアクセスするには、[従量課金 (PAYG) アカウント](https://console.bluemix.net/dashboard/ibm-iaas-g1)が必要です。
3. オプションを選択し、**「作成」**をクリックします。ツールチェーンが自動的に作成され、アプリが自動的にデプロイされます。

## ステップ 3. サービスの追加 (オプション)
{: #add_service}

ダッシュボード内から Go アプリに、セキュリティーやストレージなどのサービスを素早く追加できます。

1. [IBM Cloud アプリ・サービス](https://console.bluemix.net/developer/appservice/dashboard)の Go アプリに戻ります。
2. **「リソースの追加」**をクリックし、追加するサービスのカテゴリーを選択してから**「次へ」**をクリックし、サービスを選択します。選択されたプランに基づき、{{site.data.keyword.cloud_notm}} アプリ・サービスによって自動的にサービスが作成されます。使用する予定のサービスを以前に作成した場合は、**「既存」** カテゴリーを選択してください。
3. サービスが作成されたら、**「コードのダウンロード」** をクリックして、サービスに接続する SDK と共にプロジェクトを再生成します。

## ステップ 4. ローカルでの Go アプリのテストおよびアクセス
{: #run_local}

[{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) を使用すると、`ibmcloud dev` コマンドを使用して Go アプリをローカルで処理できます。

1. `ibmcloud dev build` コマンドを使用して、アプリケーションをビルドします。
2. `ibmcloud dev run` コマンドを使用して、アプリケーションをローカルで実行します。 アプリケーションは、ローカル・システム上の Docker コンテナーで実行されます。 `http://localhost:8080` にアクセスして、ブラウザーでアプリケーションをテストできます。
3. ヘルス・チェック・エンドポイントは `http://localhost:8080/health` で利用できます。
4. メトリックには `http://localhost:3000/metrics` でアクセスできます。

{{site.data.keyword.dev_cli_long}} について詳しくは、[CLI 資料](/docs/cli/idt/index.html)を参照してください。

## 代替デプロイメント方式の検討
{: #deploy_cloud notoc}

「デプロイメント」タブ ([こちら](/docs/go/deploying_apps.html)) で、アプリケーションを Cloud Foundry または Kubernetes にデプロイする方法について説明します。 

Go アプリケーションを拡張するには、引き続き Go プログラミング・ガイドのトピックを確認してください。
