---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 配置 Go 環境
{: #configure-go-env}

提供需遵循的標準化準則供您用來開發 Go 應用程式，以協助維護一致的可攜性。需考量的事項包括認證管理、儲存資料、保存資料，以及將內容發佈至雲端。透過遵循雲端原生實務，Go 應用程式可輕鬆地從一個環境移至另一個環境。例如，可以從測試環境移至正式作業環境，而不需要變更程式碼，或運用其他未經測試的程式碼路徑。

無論您是需要向現有 Go 應用程式新增雲端支援，還是使用入門範本套件建立 Go 應用程式，目標都是提供可攜性以用於任何開發平台。

## 向現有 Go 應用程式新增雲端支援
{: #go-add-cloud-support}

[` ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 模組聚集了各種雲端提供者（例如，Cloud Foundry 和 Kubernetes）的環境變數，因此該應用程式與環境無關。

### 安裝 `ibm-cloud-env-golang` 模組
{: #go-install-env-module}

1. 使用下列指令，安裝 `ibm-cloud-env-golang` 模組：
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. 透過參照 `mappings.json` 檔案，在您的程式碼中起始設定模組：
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  由於 Go 不支援預設參數，因此未提供預設的 `mappings.json` 檔案。如果未在 `IBMCloudEnv.Initialize()` 中指定對映檔案路徑，則會記載錯誤。
  {: tip}

  範例 `mappings.json` 檔案：
  ```javascript
  {
      "service1-credentials": {
        "searchPatterns": [
         "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name", 
              "env:my-service1-credentials", 
              "file:/localdev/my-service1-credentials.json" 
          ]
      },
      "service2-username": {
         "searchPatterns":[
            "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### 擷取服務認證
{: #go-get-creds}

使用下列指令擷取應用程式中的值。

1. 擷取變數 `service1credentials`：
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. 擷取變數 `service2username`：
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

現在，透過對不同雲端運算提供者引入的差異進行摘要，應用程式可以在任何運行環境中實作。

### 過濾標記和標籤的值
{: #go-filter-creds}

您可以根據服務標記和服務標籤來過濾模組所產生的認證，如下列範例所示：
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## 透過入門範本套件應用程式使用 Go 配置管理程式
{: #go-config-manager}

使用[入門範本套件](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 建立的 Go 應用程式會自動隨附在許多雲端部署目標中執行所需的認證和配置，例如 [Kubernetes](/docs/containers?topic=containers-getting-started)、[Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)、[{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about)、[Virtual Server (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) 或 [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started)。

  VSI 部署適用於部分入門範本套件。若要使用此特性，請前往 [{{site.data.keyword.cloud_notm}} 儀表板](https://{DomainName})，然後按一下**應用程式**磚中的**建立應用程式**。
  {: note}

### 瞭解服務認證
{: #go-credentials-config}

服務的應用程式配置資訊儲存在 `/server/config` 目錄下的 `localdev-config.json` 檔案中。此檔案位於 `.gitignore` 目錄中，可避免將機密性資訊儲存至 Git。在本端執行之任何已配置服務的連線資訊（例如使用者名稱、密碼及主機名稱）都儲存在這個檔案中。

應用程式使用配置管理程式，從環境及這個檔案中讀取連線和配置資訊。它使用位於 `server/config` 目錄的自訂建置 `mappings.json`，以瞭解可以在哪裡找到每個服務的認證。

如果應用程式在本端執行，則可以使用從 `mappings.json` 檔案中讀取的未連結認證連接至 {{site.data.keyword.cloud_notm}} 服務。但是，如果您需要建立未連結的認證，則可以從 {{site.data.keyword.cloud_notm}} Web 主控台（範例）或使用 [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") `cf create-service-key` 指令來執行此作業。

將應用程式推送到 {{site.data.keyword.cloud_notm}} 時，不會再使用這些值。應用程式將改為使用環境變數來自動連接至連結服務。 

* **Cloud Foundry**：服務認證取自 `VCAP_SERVICES` 環境變數。

* **Kubernetes**：服務認證取自每個服務的個別環境變數。

* **{{site.data.keyword.cloud_notm}} Container Service**：從虛擬伺服器實例或 {{site.data.keyword.openwhisk}} 獲取服務認證。

## 後續步驟
{: #go-next-steps-config notoc}

`ibm-cloud-env-goldang` 支援使用下列四個搜尋型樣類型來搜尋值：`user-providedd`、`cloudfoundry`、`env` 及 `file`。如果您要參閱其他受支援搜尋型樣及搜尋型樣範例，請查看[用法](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 小節。
