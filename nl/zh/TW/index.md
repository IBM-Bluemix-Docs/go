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

# 入門指導教學
{: #getting-started}

下列指導教學會逐步引導您使用 {{site.data.keyword.cloud_notm}} 提供的工具來建立及部署 Go 應用程式。您可以在指令行或 Web 型 [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) 中使用 [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli)，如下列指導教學步驟所示。透過使用這些方法的任何一種，只需幾分鐘就可以產生可用於正式作業的 Go 應用程式。

## 步驟 1. 在儀表板中建立 Go 應用程式
{: #create-go-app}

1. 從「應用程式服務」的[入門範本套件](https://cloud.ibm.com/developer/appservice/starter-kits)頁面，選取以 `Go` 撰寫的「入門範本套件」。您也可以按一下**建立應用程式**並選取 `Go` 作為語言，藉以建立空白的入門範本應用程式。

    您必須登入 {{site.data.keyword.cloud_notm}} 帳戶，才能建立應用程式。如果您沒有帳戶，則可以[登錄免費帳戶](https://cloud.ibm.com/registration)。
  {: tip}

3. 按一下**建立應用程式**。
4. 為應用程式提供**名稱**，或使用提供的一般應用程式名稱。
5. 輸入**唯一主機名稱**。此主機名稱可用來存取您的應用程式，例如：`server.mybluemix.net`。
6. 按一下**建立**。建立應用程式之後，您可以使用工具鏈來部署，也可以繼續建置，並從[指令行](/docs/cli/index.html#ibmcloud-cli)部署專案。

## 步驟 2. 使用儀表板來部署
{: #deploy-go}

1. 從「應用程式詳細資料」頁面中，按一下**部署至雲端**。
2. 根據所選擇方法的指示，設定部署方法：
  * **部署至 [Kubernetes](/docs/apps/deploying/containers.html#containers)**。此選項可建立主機（稱為工作者節點）的叢集，以部署及管理高可用性應用程式容器。您可以建立叢集或部署至現有的叢集。
  * **部署至 Cloud Foundry**。此選項可部署雲端原生應用程式，您不需要管理基礎架構。如果您的帳戶有權存取 {{site.data.keyword.cfee_full_notm}}，則可以選取**[公用雲端](/docs/cloud-foundry-public/about-cf.html#about-cf)**或**[企業環境](/docs/cloud-foundry-public/cfee.html#cfee)**的部署人員類型，您能夠使用此類型來建立及管理隔離環境，專供您的企業用來管理 Cloud Foundry 應用程式。
  * **部署至[虛擬伺服器](/docs/apps/vsi-deploy.html#vsi-deploy)**。此選項可佈建虛擬伺服器實例、載入包含應用程式的映像檔、建立 DevOps 工具鏈，以及為您起始第一個部署週期。

3. 選取您的選項，然後按一下**建立**。即會為您建立工具鏈，並自動部署您的應用程式。

## 步驟 3. 新增資源（選用）
{: #add-resource-go}

您可以在儀表板內將安全或儲存空間等服務快速新增至 Go 應用程式。

1. 回到您在 [IBM Cloud 應用程式服務](https://cloud.ibm.com/developer/appservice/dashboard)中的 Go 應用程式。
2. 按一下**新增資源**，並選取您要新增的服務種類，再按**下一步**，然後選擇您的服務。「{{site.data.keyword.cloud_notm}} 應用程式服務」會根據選取的方案建立服務。如果您先前已建立計劃要使用的服務，請選擇**現有**種類。
3. 建立服務之後，按一下**下載程式碼**，以使用連接至服務的 SDK 來重新產生專案。

## 步驟 4. 在本端測試及存取 Go 應用程式
{: #run_local-go}

您可以使用 [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli)，透過 `ibmcloud dev` 指令在本端處理 Go 應用程式。

1. 使用 `ibmcloud dev build` 指令來建置應用程式。
2. 使用 `ibmcloud dev run` 指令，在本端執行應用程式。您的應用程式是在本端系統的 Docker 容器中執行。您可以在瀏覽器中存取 `http://localhost:8080` 來測試應用程式。
3. 性能檢查端點位於 `http://localhost:8080/health`。
4. 您可以在 `http://localhost:3000/metrics` 上存取度量值。

若要進一步瞭解 {{site.data.keyword.dev_cli_long}}，請參閱完整的 [CLI 文件](/docs/cli/index.html#ibmcloud-cli)。

## 探索替代部署方法
{: #deploy_cloud-go notoc}

請在「部署」標籤（[這裡](/docs/go/deploying_apps.html)）下瞭解如何部署您的應用程式。 

若要擴充 Go 應用程式，請繼續查看 Go 程式設計手冊中的主題。
