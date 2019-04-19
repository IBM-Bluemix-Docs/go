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

# 入門指導教學
{: #getting-started}

下列指導教學會逐步引導您使用 {{site.data.keyword.cloud_notm}} 提供的工具來建立及部署 Go 應用程式。您可以在指令行或 Web 型 [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 中使用 [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)，如下列指導教學步驟所示。透過使用這些方法的任何一種，只需幾分鐘就可以產生可用於正式作業的 Go 應用程式。

## 步驟 1. 在儀表板中建立自訂 Go 應用程式
{: #create-go-app}

1. 登入 {{site.data.keyword.cloud_notm}} 帳戶來建立應用程式。如果您沒有帳戶，則可以[註冊以取得免費帳戶](https://{DomainName}/registration){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。
2. 從 {{site.data.keyword.dev_console}} 頁面中的[入門範本套件](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 中，執行下列其中一項選項：
 * 選取以 `Go` 撰寫的入門範本套件，然後在**入門範本套件詳細資料**頁面上按一下**建立應用程式**。
 * 選取空白的入門範本應用程式，然後按一下**建立應用程式**。
3. 為您的應用程式提供名稱，或使用提供的一般應用程式名稱。
4. 確定 **Go** 已選為平台，然後按一下**建立**。建立應用程式之後，您可以新增服務，然後使用工具鏈部署它，或可以從[指令行](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)繼續建置並部署專案。

## 步驟 2. 新增 {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

您現在可以將 {{site.data.keyword.watson}} 服務新增至 Go 應用程式。對於本指導教學，將 {{site.data.keyword.texttospeechshort}} 服務新增至 Go 應用程式，這會取得口頭輸入，並使用雲端 API 將其轉換為文字。

1. 從**應用程式詳細資料**頁面中，按一下**新增服務**。
2. 選取 **AI**，然後按**下一步**。
3. 選取 **{{site.data.keyword.texttospeechshort}}**然後按**下一步**。
4. 按一下**建立**。

在將此服務新增至您的應用程式之後，您將會看到 [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 相依關係新增至 `Gopkg.toml`，並對 `services/` 目錄中的服務提供簡單的設備測試代碼。此外，還會包括在各自環境存取服務認證的配置資訊。

若要下載程式碼，請在**應用程式詳細資料**頁面上按一下**下載程式碼**。下載的程式碼隨附 [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")，以及一些基本起始設定碼。

## 步驟 3. 從主控台部署您的應用程式
{: #deploy-go}

1. 從**應用程式詳細資料**頁面中，按一下**配置 Continuous Delivery**。
2. 根據所選擇方法的指示，設定部署目標：
  * **部署至 [IBM Kubernetes Service](/docs/apps/deploying?topic=creating-apps-containers-kube)**。此選項可建立主機（稱為工作者節點）的叢集，以部署及管理高可用性應用程式容器。您可以建立叢集或部署至現有的叢集。
  * **部署至 Cloud Foundry**。此選項可部署雲端原生應用程式，您不需要管理基礎架構。如果您的帳戶有權存取 {{site.data.keyword.cfee_full_notm}}，則可以選取**[公用雲端](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)**或**[企業環境](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**的部署者類型，您能夠使用此類型來建立及管理隔離環境，專供您的企業用來管理 Cloud Foundry 應用程式。
  * **部署至[虛擬伺服器](/docs/apps?topic=creating-apps-vsi-deploy)**。此選項可佈建虛擬伺服器實例、載入包含應用程式的映像檔、建立 DevOps 工具鏈，以及為您起始第一個部署週期。

3. 在設定您的部署目標之後，請按**下一步**。
4. 選取您的配置選項，然後按一下**建立**。即會為您建立工具鏈，並自動部署您的應用程式。

## 步驟 4. 在本端測試及存取 Go 應用程式
{: #run_local-go}

在**應用程式詳細資料**頁面上，您可以：
* 按一下**檢視儲存庫**來檢視您的儲存庫。
* 按一下**檢視工具鏈**來檢視及修改您的工具鏈。

您可以使用 {{site.data.keyword.dev_cli_long}}，透過 `ibmcloud dev` 指令在本端處理 Go 應用程式。這些工具可協助您在本端反覆運算，並將您的變更推送至雲端。

1. 使用 `ibmcloud dev build` 指令來建置應用程式。
2. 使用 `ibmcloud dev run` 指令，在本端執行應用程式。您的應用程式是在本端系統的 Docker 容器中執行。您可以在瀏覽器中存取 `http://localhost:8080` 來測試應用程式。
3. 性能檢查端點位於 `http://localhost:8080/health`。
4. 您可以在 `http://localhost:3000/metrics` 上存取度量值。

若要進一步瞭解 {{site.data.keyword.dev_cli_notm}}，請參閱完整的 [CLI 文件](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)。

您現在準備好可以使用 {{site.data.keyword.texttospeechshort}} 服務建置您的應用程式！

## 探索替代部署方法
{: #alt-deploy-go notoc}

瞭解如何在[部署標籤](/docs/go?topic=go-go-deploy-apps)下部署應用程式。

若要擴充 Go 應用程式，請繼續查看 Go 程式設計手冊中的主題。
