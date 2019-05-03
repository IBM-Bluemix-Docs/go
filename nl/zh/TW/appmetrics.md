---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 透過 Go 應用程式使用應用程式度量值
{: #appmetrics}

應用程式度量值對於監視應用程式的效能而言，非常重要。能夠即時檢視 CPU、「記憶體」、「延遲」及 HTTP 度量值之類的度量值，對於確保應用程式可在一段時間內有效率地執行來說至關重要。您可以使用 Cloud Foundry 的[自動調整](/docs/services/Auto-Scaling/index.html)這類雲端服務，根據度量值動態調整實例，以符合現行工作負載。使用應用程式度量值，您可以精確地通知何時擴增、縮減或清除不再需要的實例以保留低成本。

應用程式度量值會擷取為時間序列資料。聚集及視覺化擷取的度量值有助於識別一般效能問題，例如：

* 部分或所有路徑上的 HTTP 回應時間太慢
* 應用程式的傳輸量太低
* 需求飆升導致速度變慢
* 傳輸量/負載層次的 CPU 使用率比預期的還高
* 記憶體用量偏高或持續成長中（可能會有記憶體洩漏）

## 將應用程式度量值新增至現有的 Go 應用程式
{: #add-appmetrics-existing}

若要將效能監視新增至 Go 應用程式，您可以使用 `promhttp` 套件所提供度量值的綜合性聚集。

`promhttp` 套件有許多延伸點，包括 [Prometheus 配置](https://github.com/prometheus/client_golang)。

1. 例如，使用下列簡單的 "Hello World" Go + Gin 應用程式：
    ```go
    // imports above
    func main() {
        router := gin.Default()
        router.GET("/", func(c *gin.Context) {
            c.String(http.StatusOK, "Hello, World!")
        }
        router.Run(":3000")
    }
    ```
    {: codeblock}

2. 使用下列指令取得套件：
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. 將 `promhttp` 新增至匯入項目：
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  還必須將額外的度量值路徑新增至路由器。

  現在，修訂過的程式碼類似下列範例：
  ```go
  // imports above
  func main() {
      router := gin.Default()
      router.GET("/", func(c *gin.Context) {
          c.String(http.StatusOK, "Hello, World!")
      }
      router.GET("/metrics", gin.WrapH(promhttp.Handler()))
      router.Run(":3000")
  }
  ```
  {: codeblock}

## 使用入門範本套件中的應用程式度量值
{: #starterkits-appmetrics}

從「入門範本套件」建立的伺服器端 Go 應用程式會自動隨附於 `http://<hostname>:<port>/metrics` 下的 [Prometheus 端點](https://prometheus.io/)。這個端點的程式碼位於 `server.go` 中。

如需相關資訊，請參閱 [Prometheus 的 GitHub 儲存庫](https://github.com/prometheus/client_golang/)。
