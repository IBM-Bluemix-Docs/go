---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入門チュートリアル
{: #getting-started}

以下のチュートリアルでは、{{site.data.keyword.cloud_notm}} 提供のツールを使用して Go アプリを作成してデプロイする手順を説明します。 以下のチュートリアルの手順で示したように、コマンド・ラインで [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) を使用でき、また Web ベースの [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) を使用することもできます。これらの方法のいずれかを使用して、実動用の Go アプリケーションを数分で生成できます。

## ステップ 1. ダッシュボードでの Go アプリの作成
{: #create-go-app}

1. アプリ・サービスの[スターター・キット](https://cloud.ibm.com/developer/appservice/starter-kits)のページから、`Go` で作成されたスターター・キットを選択します。 **「アプリの作成」** をクリックし、言語として `Go` を選択することによって、ブランクのスターター・アプリを作成することもできます。

    アプリを作成するには、{{site.data.keyword.cloud_notm}} アカウントにログインしている必要があります。アカウントがない場合は、[無料アカウントを登録](https://cloud.ibm.com/registration)できます。
    {: tip}

3. **「アプリの作成」**をクリックします。
4. アプリに**名前**を付けるか、提供された汎用アプリ名を使用します。
5. **固有のホスト名**を入力します。 このホスト名は、アプリケーションにアクセスするために使用されます (例: `server.mybluemix.net`)。
6. **「作成」**をクリックします。 アプリを作成したら、ツールチェーンを使用してデプロイでき、また引き続き[コマンド・ライン](/docs/cli/index.html#ibmcloud-cli)からプロジェクトをビルドしてからデプロイすることもできます。

## ステップ 2. ダッシュボードを使用したデプロイ
{: #deploy-go}

1. 「アプリの詳細」ページで、**「クラウドにデプロイ」**をクリックします。
2. 以下の選択する方法の手順に従って、デプロイメント方式をセットアップします。
  * **[Kubernetes へのデプロイ](/docs/apps/deploying/containers.html#containers)**。このオプションでは、可用性の高いアプリケーション・コンテナーをデプロイして管理するためのワーカー・ノードと呼ばれるホストのクラスターを作成します。クラスターを作成したり、既存のクラスターにデプロイしたりすることができます。
  * **Cloud Foundry へのデプロイ**。このオプションでは、基礎となるインフラストラクチャーの管理を必要とせずに、クラウド・ネイティブ・アプリをデプロイします。ご使用のアカウントで {{site.data.keyword.cfee_full_notm}} にアクセスできる場合、デプロイヤー・タイプの **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** または **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)** のいずれかを選択できます。後者を使用すれば、お客様の会社専用に Cloud Foundry アプリケーションをホストするための分離された環境を作成して管理することができます。
  * **[仮想サーバー](/docs/apps/vsi-deploy.html#vsi-deploy)へのデプロイ**。このオプションでは、仮想サーバー・インスタンスをプロビジョンし、アプリが含まれたイメージをロードし、DevOps ツールチェーンを作成し、最初のデプロイメント・サイクルを自動的に開始します。

3. オプションを選択し、**「作成」**をクリックします。 ツールチェーンが自動的に作成され、アプリが自動的にデプロイされます。

## ステップ 3. リソースの追加 (オプション)
{: #add-resource-go}

ダッシュボード内から Go アプリに、セキュリティーやストレージなどのサービスを素早く追加できます。

1. [IBM Cloud アプリ・サービス](https://cloud.ibm.com/developer/appservice/dashboard)の Go アプリに戻ります。
2. **「リソースの追加」**をクリックし、追加するサービスのカテゴリーを選択してから**「次へ」**をクリックし、サービスを選択します。 選択されたプランに基づき、{{site.data.keyword.cloud_notm}} アプリ・サービスによって自動的にサービスが作成されます。 使用する予定のサービスを以前に作成した場合は、**「既存」** カテゴリーを選択してください。
3. サービスが作成されたら、**「コードのダウンロード」** をクリックして、サービスに接続する SDK と共にプロジェクトを再生成します。

## ステップ 4. ローカルでの Go アプリのテストおよびアクセス
{: #run_local-go}

[{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) を使用すると、`ibmcloud dev` コマンドを使用して Go アプリをローカルで処理できます。

1. `ibmcloud dev build` コマンドを使用して、アプリケーションをビルドします。
2. `ibmcloud dev run` コマンドを使用して、アプリケーションをローカルで実行します。 アプリケーションは、ローカル・システム上の Docker コンテナーで実行されます。 `http://localhost:8080` にアクセスして、ブラウザーでアプリケーションをテストできます。
3. ヘルス・チェック・エンドポイントは `http://localhost:8080/health` で利用できます。
4. メトリックには `http://localhost:3000/metrics` でアクセスできます。

{{site.data.keyword.dev_cli_long}} について詳しくは、[CLI 資料](/docs/cli/index.html#ibmcloud-cli)を参照してください。

## 代替デプロイメント方式の検討
{: #deploy_cloud-go notoc}

「デプロイメント」タブでアプリケーションをデプロイする方法については、[ここ](/docs/go/deploying_apps.html))を参照してください。 

Go アプリケーションを拡張するには、引き続き Go プログラミング・ガイドのトピックを確認してください。
