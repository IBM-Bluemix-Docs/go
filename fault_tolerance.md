---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: fault tolerance go, hystrix go, add fault tolerance, prometheus go, debug go apps

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Setting up fault tolerance in Go apps
{: #fault-tolerance}

Fault tolerance allows an application to continue running if a component fails, or becomes unresponsive. You can add fault tolerance to an existing Go application, or enable these features from a generated Go Application. This tutorial focuses on using the [Hystrix package](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") to add fault tolerance support to a Go application.

## Adding fault tolerance to an existing Go app
{: #add-fault-tolerance}

In the same location as your Go application’s `Gopkg.toml` file, enter the following commands to add the required packages into your dependency list:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

To use hystrix, a middleware function must be added:
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

Handling failures can change depending on what type of Go application is being used. For web apps, a failure redirects to a 500 page, while microservices return a JSON object that specifies the error.

This middleware takes a string that is a configured command. Configuring a command must be done in the `main` function as follows:
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

After a Hystrix command is configured, the middleware can be added to the router with the following statement:
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Exposing Hystrix metrics to Prometheus (Optional)
{: #hystrix-optional}

Before you add Hystrix to Prometheus, your app must be configured with application metrics. You can follow the steps in the [Using Application Metrics with Go apps](/go?topic=go-go-appmetrics) topic to add appmetrics support.

Hystrix provides users the ability to take metrics data and expose them to a metric collector. To expose Hystrix to prometheus, the metric_collector package must also be added:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

In addition to `metric_collector`, an additional file, `prometheus_collector.go`, must be added to your Go application. This file can be found [here](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go). Add this file to the `plugins` package.

Two additional imports are required:
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

In the `main` function, add the following statements:
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

Running the project and going to the `/metrics` route shows the metrics that are provided by Hystrix.