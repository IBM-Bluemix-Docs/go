---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-30"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry, go deploy virtual

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go アプリのデプロイと統合
{: #go-deploy-apps}

Go アプリは、ツールチェーンまたはコマンド・ライン・インターフェースを使用してデプロイできます。 ツールチェーンは、アプリケーション・デプロイメントのビルドと自動化に役立つツール統合のセットです。 ツールチェーンとその機能について詳しくは、[継続的デリバリー](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started)を参照してください。 このトピックは、CLI、Kubernetes、コンテナー、VSI、プライベート・クラウドなど、Go アプリケーションのさまざまなデプロイメント方法に関する有用な情報を見つけるのに役立ちます。

## Kubernetes クラスターへのデプロイ
{: #deploy_kube-go}

{{site.data.keyword.cloud_notm}} Kubernetes Service を使用してコンテナー化 Go アプリをデプロイする方法を知ることができます。 {{site.data.keyword.cloud_notm}} での Kubernetes クラスターのセットアップについて詳しくは、[チュートリアルのステップ](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)を確認してください。

CLI を使用する関連ブログ:
* [{{site.data.keyword.dev_cli_long}} CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} を使用した Kubernetes へのデプロイ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")。
* [{{site.data.keyword.dev_cli_short}} CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/){: new_window} を使用した IBM Cloud Private へのデプロイ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")。

## CLI を使用したコンテナー・サービスへのデプロイ
{: #go-deploy-container}

`ibmcloud dev deploy` コマンドを使用して、{{site.data.keyword.cloud_notm}} にデプロイします。 

{{site.data.keyword.cloud_notm}} 内の IBM Container Services にデプロイするには、以下のコマンドを使用します。
```
ibmcloud dev deploy –target container 
```
{: codeblock}

`ibmcloud dev` コマンドについて詳しくは、[アプリの開発とデプロイ](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)を参照してください。

## Cloud Foundry へのデプロイ
{: #go-deploy-cf}

このオプションでは、基礎となるインフラストラクチャーの管理を必要とせずに、クラウド・ネイティブ・アプリをデプロイします。

アプリを [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about#about) にデプロイする計画の場合は、[{{site.data.keyword.cloud_notm}} アカウントを準備する](/docs/cloud-foundry?topic=cloud-foundry-prepare#prepare)必要があります。

ご使用のアカウントで {{site.data.keyword.cfee_full_notm}} にアクセスできる場合、デプロイヤー・タイプの **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)** または **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)** のいずれかを選択できます。後者を使用すれば、お客様の会社専用に Cloud Foundry アプリケーションをホストするための分離された環境を作成して管理することができます。

## 仮想サーバーへのデプロイ
{: #go-vsi-deploy}

{{site.data.keyword.cloud}} App Service アプリを仮想サーバー・インスタンスにデプロイすることで、プラットフォーム開発者とインフラストラクチャー開発者のアクティビティーを連携させ、アプリの制御と柔軟性を高めることができます。

仮想サーバー・インスタンスは、すべてのワークロード・タイプにおいて、他の構成と較べて優れた透過性、予測可能性、および自動化を提供します。 これをベアメタル・サーバーと組み合わせると、ユニークなワークロードの組み合わせを作成できます。 例えば、ベアメタルと Debian Linux ベースのオペレーティング・システムを実行する GPU 構成で、高性能なデータベース・ロジックまたは機械学習を作成することができます。

詳しくは、[仮想サーバーへのデプロイ](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy)を参照してください。

