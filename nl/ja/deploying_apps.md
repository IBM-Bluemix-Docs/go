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

# Go アプリのデプロイと統合
{: #deploy_apps}

Go アプリは、ツールチェーンまたはコマンド・ライン・インターフェースを使用してデプロイできます。ツールチェーンは、アプリケーション・デプロイメントのビルドと自動化に役立つツール統合のセットです。 ツールチェーンとその機能について詳しくは、[継続的デリバリー](/docs/services/ContinuousDelivery/index.html#cd_getting_started)を参照してください。 このトピックは、CLI、Kubernetes、コンテナー、VSI、プライベート・クラウドなど、Go アプリケーションのさまざまなデプロイメント方法に関する有用な情報を見つけるのに役立ちます。

## Kubernetes クラスターへのデプロイ
{: #deploy_kube-go}

{{site.data.keyword.cloud_notm}} Kubernetes Service を使用してコンテナー化 Go アプリをデプロイする方法を知ることができます。 {{site.data.keyword.cloud_notm}} での Kubernetes クラスターのセットアップについて詳しくは、[チュートリアルのステップ](/docs/containers/cs_cluster.html#cs_cluster)を確認してください。

CLI を使用する関連ブログ:
* [Deploying to Kubernetes with the IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/)。
* [Deploying to IBM Cloud Private with IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/)。

## CLI を使用したコンテナー・サービスへのデプロイ
{: #go-deploy-container}

`ibmcloud dev deploy` コマンドを使用して、{{site.data.keyword.cloud_notm}} にデプロイします。 

{{site.data.keyword.cloud_notm}} 内の IBM Container Services にデプロイするには、以下のコマンドを使用します。
```
ibmcloud dev deploy –target container 
```
{: codeblock}

`ibmcloud dev` コマンドについて詳しくは、[アプリの開発とデプロイ](/docs/cli/index.html)を参照してください。

## Cloud Foundry へのデプロイ
{: #go-deploy-cf}

このオプションでは、基礎となるインフラストラクチャーの管理を必要とせずに、クラウド・ネイティブ・アプリをデプロイします。

アプリを [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html) にデプロイする計画の場合は、[{{site.data.keyword.cloud_notm}} アカウントを準備する](/docs/cloud-foundry/prepare-account.html)必要があります。

ご使用のアカウントで {{site.data.keyword.cfee_full_notm}} にアクセスできる場合、デプロイヤー・タイプの **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** または **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)** のいずれかを選択できます。後者を使用すれば、お客様の会社専用に Cloud Foundry アプリケーションをホストするための分離された環境を作成して管理することができます。

## 仮想サーバーへのデプロイ
{: #go-vsi-deploy}

{{site.data.keyword.cloud}} App Service アプリを仮想サーバー・インスタンスにデプロイすることで、プラットフォーム開発者とインフラストラクチャー開発者のアクティビティーを連携させ、アプリの制御と柔軟性を高めることができます。

仮想サーバー・インスタンスは、すべてのワークロード・タイプにおいて、他の構成と較べて優れた透過性、予測可能性、および自動化を提供します。 これをベアメタル・サーバーと組み合わせると、ユニークなワークロードの組み合わせを作成できます。 例えば、ベアメタルと Debian Linux ベースのオペレーティング・システムを実行する GPU 構成で、高性能なデータベース・ロジックまたは機械学習を作成することができます。

詳しくは、[仮想サーバーへのデプロイ](/docs/apps/vsi-deploy.html#vsi-deploy)を参照してください。

