---

copyright:
  years: 2018, 2019
lastupdated: "2019-10-29"

keywords: prometheus go, application metrics go, view metrics go app, add metrics go, promhttp go, autoscaling go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using Application Metrics with Go apps
{: #go-appmetrics}

Application metrics are important for monitoring the performance of your application. Having a live view of metrics like CPU, Memory, Latency, and HTTP are essential to ensure that your application is running effectively over time. You can use a cloud service like Cloud Foundry's [autoscaling](/docs/services/Auto-Scaling?topic=Auto-Scaling) that relies on metrics to dynamically scale instances to match current workload. By using application metrics, you are informed precisely when to scale up, down, or clean up instances that are no longer needed to keep costs low.

Application metrics are captured as time series data. Aggregating and visualizing captured metrics can help to identify common performance problems such as:

* Slow HTTP response times on some or all routes
* Poor throughput in the application
* Spikes in demand that cause slowdown
* Higher than expected CPU usage for the level of throughput or load
* High or growing memory usage (potential memory leak)

## Adding Application Metrics to your existing Go application
{: #go-add-appmetrics-existing}

To add performance monitoring to your Go application, you can use the comprehensive aggregation of metrics that are provided by the `promhttp` package.

The `promhttp` package has many extension points, including [Prometheus configuration](https://github.com/prometheus/client_golang){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").

1. For example, use the following simple "Hello World" Go + Gin application:
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

2. Get the package with the following command:
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. Add `promhttp` to the imports:
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  An extra metrics route must also be added to the router.

  The revised code now looks like the following example:
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

