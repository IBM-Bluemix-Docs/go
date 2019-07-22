---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: prometheus go, application metrics go, view metrics go app, add metrics go, promhttp go, autoscaling go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 将应用程序度量值用于 Go 应用程序
{: #go-appmetrics}

应用程序度量值对于监视应用程序的性能非常重要。实时查看度量值（如 CPU、内存、等待时间和 HTTP）对于确保应用程序能够长期有效地运行至关重要。您可以使用云服务（如 Cloud Foundry 的[自动缩放](/docs/services/Auto-Scaling?topic=Auto-Scaling)），依靠度量值来动态缩放实例，以便与当前工作负载相匹配。通过使用应用程序度量值，您可以精确地获悉何时扩展、缩减或清除不再需要的实例，从而使成本保持在较低水平。

应用程序度量值将作为时间序列数据进行捕获。聚集和可视化捕获的度量值可帮助识别常见的性能问题，例如：

* 某些或所有路径上的 HTTP 响应时间缓慢
* 应用程序中的吞吐量差
* 导致性能下降的需求峰值
* 吞吐量或负载级别的 CPU 使用率高于预期
* 内存使用量高或不断增长（潜在内存泄漏）

## 向现有 Go 应用程序添加应用程序度量值
{: #go-add-appmetrics-existing}

要向 Go 应用程序添加性能监视，可以使用 `promhttp` 包提供的全面度量值聚集。

`promhttp` 包具有多个扩展点，包括 [Prometheus 配置](https://github.com/prometheus/client_golang){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。

1. 例如，使用以下简单的“Hello World”Go + Gin 应用程序：
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

2. 使用以下命令获取包：
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. 将 `promhttp` 添加到导入：
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  还必须向路由器添加额外的度量值路径。

  现在，修订后的代码类似于以下示例：
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

## 在入门模板工具包中使用 Application Metrics
{: #go-starterkits-appmetrics}

从入门模板工具包创建的服务器端 Go 应用程序自动在 `http://<hostname>:<port>/metrics` 下随附 [Prometheus 端点](https://prometheus.io/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。此端点的代码位于 `server.go` 中。

有关更多信息，请参阅[用于 Prometheus 的 GitHub 存储库](https://github.com/prometheus/client_golang/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。
