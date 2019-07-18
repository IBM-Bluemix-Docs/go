---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: fault tolerance go, hystrix go, add fault tolerance, prometheus go, debug go apps

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 在 Go 應用程式中設定容錯
{: #fault-tolerance}

容錯可讓應用程式在元件失敗或變得無回應時繼續執行。您可以向現有 Go 應用程式新增容錯，或者從產生的 Go 應用程式啟用這些特性。本指導教學重點描述了如何使用 [Hystrix 套件](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 向 Go 應用程式新增容錯支援。

## 將容錯新增至現有的 Go 應用程式
{: #add-fault-tolerance}

在與 Go 應用程式的 `Gopkg.toml` 檔案相同的位置中，輸入下列指令以將必要的套件新增到相依關係清單：
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

若要使用 Hystrix，則必須新增中介軟體功能：
```go
func HystrixHandler(command string) gin.HandlerFunc {
  return func(c *gin.Context) {
    hystrix.Do(command, func() error {
      c.Next()
      return nil
    }, func(err error) error {
      //Handle failures
      return err
    })
  }
}
``` 
{: codeblock}

處理故障可能會根據使用的 Go 應用程式類型而發生變更。若為 Web 應用程式，失敗會重新導向至 500 頁面，而微服務則會傳回指定錯誤的 JSON 物件。

此中介軟體會接受一個本身為已配置指令的字串。配置指令必須在 `main` 函數中完成，如下所示：
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

配置 Hystrix 指令之後，您可以使用下列陳述式將中介軟體新增至路由器：
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## 將 Hystrix 度量值公開到 Prometheus（選用）
{: #hystrix-optional}

在將 Hystrix 新增到 Prometheus 之前，必須使用應用程式度量值配置應用程式。您可以遵循[將應用程式度量值用於 Go 應用程式](/docs/go?topic=go-go-appmetrics)主題中的步驟來新增應用程式度量值支援。

Hystrix 讓使用者能夠取得度量值資料，並將資料公開到度量值收集程式。若要向 prometheus 公開 Hystrix，還必須新增 metric_collector 套件：
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

除了 `metric_collector`，必須將檔案 `prometheus_collector.go` 新增到 Go 應用程式。此檔案可在[這裡](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go)找到。將此檔案新增到 `plugins` 套件。

需要匯入兩項：
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

在 `main` 函數中，新增下列陳述式：
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

執行專案並移至 `/metrics` 路徑時，會顯示 Hystrix 所提供的度量值。
