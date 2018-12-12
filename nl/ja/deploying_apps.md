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

# Go アプリのデプロイと統合
{: #deploy_apps}

ツールチェーンまたはコマンド・ライン・インターフェースを使用して Go アプリをデプロイできます。ツールチェーンは、アプリケーション・デプロイメントのビルドと自動化に役立つツール統合のセットです。ツールチェーンとその機能について詳しくは、[継続的デリバリー](/docs/services/ContinuousDelivery/index.html)を参照してください。このトピックは、CLI、Kubernetes、コンテナー、VSI、プライベート・クラウドなど、Go アプリケーションのさまざまなデプロイメント方法に関する有用な情報を見つけるのに役立ちます。

## Kubernetes クラスターへのデプロイ
{: #deploy_kube}

{{site.data.keyword.cloud_notm}} Kubernetes Service を使用してコンテナー化 Go アプリをデプロイする方法を知ることができます。{{site.data.keyword.cloud_notm}} での Kubernetes クラスターのセットアップについて詳しくは、[チュートリアルのステップ](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_cluster)を確認してください。

CLI を使用する関連ブログ:
* [Deploying to Kubernetes with the IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/)。
* [Deploying to IBM Cloud Private with IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/)。

## CLI を使用したコンテナー・サービスへのデプロイ
{: #cf-deploy}

`ibmcloud dev deploy` コマンドを使用して、{{site.data.keyword.cloud_notm}} にデプロイします。 

{{site.data.keyword.cloud_notm}} 内の IBM Container Services にデプロイするには、以下のコマンドを使用します。
```
ibmcloud dev deploy –target container 
```
{: codeblock}

`ibmcloud dev` コマンドについて詳しくは、[アプリの開発とデプロイ](/docs/cli/idt/index.html)を参照してください。

## 仮想サーバーへのデプロイ
{: #vsi-deploy}

{{site.data.keyword.cloud}} App Service アプリを仮想サーバー・インスタンスにデプロイすることで、プラットフォーム開発者とインフラストラクチャー開発者のアクティビティーを連携させ、アプリの制御と柔軟性を高めることができます。

仮想サーバー・インスタンスは、すべてのワークロード・タイプにおいて、他の構成と較べて優れた透過性、予測可能性、および自動化を提供します。これをベアメタル・サーバーと組み合わせると、ユニークなワークロードの組み合わせを作成できます。例えば、ベアメタルと Debian Linux ベースのオペレーティング・システムを実行する GPU 構成で、高性能なデータベース・ロジックまたは機械学習を作成することができます。

詳しくは、[仮想サーバーへのデプロイ](/docs/apps/vsi-deploy.html)を参照してください。



