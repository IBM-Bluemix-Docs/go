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

# 在 Go 应用程序中设置容错
{: #fault-tolerance}

容错允许应用程序在组件发生故障或者变为无响应的情况下继续运行。您可以向现有 Go 应用程序添加容错，或者从生成的 Go 应用程序启用这些功能。本教程重点描述了如何使用 [Hystrix 包](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 向 Go 应用程序添加容错支持。

## 向现有 Go 应用程序添加容错
{: #add-fault-tolerance}

在与 Go 应用程序的 `Gopkg.toml` 文件相同的位置中，输入以下命令以将必需的包添加到依赖项列表：
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

要使用 hystrix，必须添加中间件功能：
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

根据使用的 Go 应用程序的类型，处理故障可能发生更改。对于 Web 应用程序，故障将重定向到 500 页面，同时微服务返回指定该错误的 JSON 对象。

此中间件采用作为已配置命令的字符串。必须在 `main` 函数中完成命令配置，如下所示：
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

配置 Hystrix 命令后，可以使用以下语句将中间件添加到路由器：
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## 向 Prometheus 公开 Hystrix 度量值（可选）
{: #hystrix-optional}

在将 Hystrix 添加到 Prometheus 之前，必须使用应用程序度量值配置应用程序。您可以遵循[将应用程序度量值用于 Go 应用程序](/docs/go?topic=go-go-appmetrics)主题中的步骤来添加应用程序度量值支持。

Hystrix 使用户能够获取度量值数据并公开给度量值收集器。要向 prometheus 公开 Hystrix，还必须添加 metric_collector 包：
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

除了 `metric_collector`，必须将文件 `prometheus_collector.go` 添加到 Go 应用程序。可在[此处](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go)找到此文件。将此文件添加到 `plugins` 包。

需要两个导入：
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

在 `main` 函数中，添加以下语句：
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

运行项目并转至 `/metrics` 路径将显示 Hystrix 提供的度量值。
