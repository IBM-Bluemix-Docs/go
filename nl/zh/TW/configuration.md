---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 配置 Go 環境
{: #configure-go-env}

提供需遵循的標準化準則供您用來開發 Go 應用程式，以協助維護一致的可攜性。需考量的事項包括認證管理、儲存資料、保存資料，以及將內容發佈至雲端。透過遵循「雲端原生」作法，Go 應用程式可以從某個環境輕鬆移至另一個環境。例如，可以從測試環境移至正式作業環境，而不需要變更程式碼，或運用其他未經測試的程式碼路徑。

無論您是需要將雲端支援新增至現有的 Go 應用程式，還是使用「入門範本套件」來建立 Go 應用程式，目標都是為了提供可用於任何開發平台的可攜性。

## 將雲端支援新增至現有的 Go 應用程式
{: #go-add-cloud-support}

[`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 模組可從各種雲端提供者（例如 CloudFoundry 及 Kubernetes）聚集環境變數，因此該應用程式與環境無關。

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

使用下列指令，以擷取應用程式中的值。

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

現在，您可以藉由摘要從不同「雲端」運算提供者建立的差異，以在任何運行環境中實作應用程式。

### 過濾標記和標籤的值
{: #go-filter-creds}

您可以根據服務標記和服務標籤來過濾模組所產生的認證，如下列範例所示：
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## 使用來自入門範本套件應用程式的 Go 配置管理程式
{: #go-config-manager}

使用[入門範本套件](https://cloud.ibm.com/developer/appservice/starter-kits/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 建立的 Go 應用程式會自動隨附在許多雲端部署環境（例如 Cloud Foundry、Kubernetes、VSI 及 Functions）中執行所需的認證及配置。

### 瞭解服務認證
{: #go-credentials-config}

您的服務應用程式配置資訊儲存在 `/server/config` 目錄的 `localdev-config.json` 檔案中。此檔案位於 `.gitignore` 目錄中，可避免將機密性資訊儲存至 Git。在本端執行之任何已配置服務的連線資訊（例如使用者名稱、密碼及主機名稱）都儲存在這個檔案中。

應用程式會使用配置管理程式，從環境及這個檔案中讀取連線及配置資訊。它使用位於 `server/config` 目錄的自訂建置 `mappings.json`，以瞭解可以在哪裡找到每個服務的認證。

如果應用程式是在本端執行，則可以使用從 `mappings.json` 檔案讀取的未連結認證，來連接至 {{site.data.keyword.cloud_notm}} 服務。但是，如果您需要建立未連結的認證，則可以從 {{site.data.keyword.cloud_notm}} Web 主控台（範例）或使用 [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") `cf create-service-key` 指令來執行此作業。

當您將應用程式推送至 {{site.data.keyword.cloud_notm}} 時，不會再使用這些值。相反地，應用程式會使用環境變數自動連接至連結服務。 

* **Cloud Foundry**：服務認證取自 `VCAP_SERVICES` 環境變數。

* **Kubernetes**：服務認證取自每個服務的個別環境變數。

* **{{site.data.keyword.cloud_notm}} 容器服務**：服務認證是取自虛擬伺服器實例或 {{site.data.keyword.openwhisk}} (Openwhisk)。

## 後續步驟
{: #go-next-steps-config notoc}

`ibm-cloud-env-goldang` 支援使用下列四個搜尋型樣類型來搜尋值：`user-providedd`、`cloudfoundry`、`env` 及 `file`。如果您要參閱其他受支援搜尋型樣及搜尋型樣範例，請查看[用法](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 小節。
