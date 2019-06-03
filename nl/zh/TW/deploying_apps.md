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

# 部署及整合 Go 應用程式
{: #go-deploy-apps}

您可以利用工具鏈或使用指令行介面來部署 Go 應用程式。工具鏈是一組工具整合，可協助建置及自動化應用程式部署。若要進一步瞭解工具鏈及其運作方式，請參閱 [Continuous Delivery](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started)。本主題可協助您找到 Go 應用程式中不同部署方法（例如，CLI、Kubernetes、容器、VSI 及專用雲端）的有用資訊。

## 部署至 Kubernetes 叢集
{: #deploy_kube-go}

您可以學習如何使用「{{site.data.keyword.cloud_notm}} Kubernetes 服務」來部署容器化的 Go 應用程式。如需在 {{site.data.keyword.cloud_notm}} 中設定 Kubernetes 叢集的相關資訊，請參閱[指導教學步驟](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)。

使用 CLI 的相關部落格：
* [Deploying to Kubernetes with the {{site.data.keyword.dev_cli_long}} CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。
* [Deploying to IBM Cloud Private with {{site.data.keyword.dev_cli_short}} CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。

## 使用 CLI 部署至容器服務
{: #go-deploy-container}

使用 `ibmcloud dev deploy` 指令可部署至 {{site.data.keyword.cloud_notm}}。 

若要部署至 {{site.data.keyword.cloud_notm}} 中的 IBM Container Services，請使用下列指令：
```
ibmcloud dev deploy –target container 
```
{: codeblock}

如需 `ibmcloud dev` 指令的相關資訊，請參閱[開發及部署應用程式](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)。

## 部署至 Cloud Foundry
{: #go-deploy-cf}

此選項可部署雲端原生應用程式，您不需要管理基礎架構。

如果您計劃將應用程式部署至 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about#about)，則必須[準備 {{site.data.keyword.cloud_notm}} 帳戶](/docs/cloud-foundry?topic=cloud-foundry-prepare#prepare)。

如果您的帳戶有權存取 {{site.data.keyword.cfee_full_notm}}，則可以選取**[公用雲端](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)**或**[企業環境](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)**的部署者類型，您能夠使用此類型來建立及管理隔離環境，專供您的企業用來管理 Cloud Foundry 應用程式。

## 部署至虛擬伺服器
{: #go-vsi-deploy}

將「{{site.data.keyword.cloud}} 應用程式服務」應用程式部署至虛擬伺服器實例，讓您的平台與基礎架構開發人員活動能夠一起運作並提高應用程式控制和彈性。

與其他配置相比時，虛擬伺服器實例為所有工作負載類型提供較佳的透通性、可預測性及自動化。將它與裸機伺服器結合，即可建立唯一的工作負載組合。例如，您可以使用執行 Debian Linux 作業系統的裸機伺服器和 GPU 配置，建立高效能的資料庫邏輯或機器學習。

如需相關資訊，請參閱[部署至虛擬伺服器](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy)。

