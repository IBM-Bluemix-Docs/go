---

copyright:
  years: 2018
lastupdated: "2018-10-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go アプリでのフォールト・トレランスのセットアップ
{: #fault-tolerance}

フォールト・トレランスを使用すると、コンポーネントで障害が発生した場合や応答しなくなった場合に、アプリケーションを実行し続けることができます。既存の Go アプリケーションにフォールト・トレランスを追加したり、生成された Go アプリケーションからこれらの機能を有効にしたりすることもできます。このチュートリアルでは、[Hystrix パッケージ](https://godoc.org/github.com/afex/hystrix-go/hystrix)を使用して Go アプリケーションにフォールト・トレランス・サポートを追加する方法を中心に説明します。

## 既存の Go アプリへのフォールト・トレランスの追加
{: #add-fault-tolerance}

Go アプリケーションの `Gopkg.toml` ファイルと同じ場所で、以下のコマンドを入力して、必要なパッケージを依存関係リストに追加します。
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

hystrix を使用するには、ミドルウェア機能を追加する必要があります。
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

障害の処理は、使用されている Go アプリケーションのタイプによって異なります。Web アプリの場合、障害が発生すると 500 ページにリダイレクトされます。マイクロサービスの場合は、エラーを指定する JSON オブジェクトが返されます。

このミドルウェアは、構成済みコマンドであるストリングを使用します。コマンドの構成は、`main` 関数で以下のように実行する必要があります。
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Hystrix コマンドの構成後、以下のステートメントを使用して、ミドルウェアをルーターに追加できます。
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Hystrix メトリックの Prometheus への公開 (オプション)

Hystrix を Prometheus に追加する前に、アプリケーション・メトリックを使用してアプリを構成する必要があります。『[Go アプリでのアプリケーション・メトリックの使用](/docs/go/appmetrics.html)』トピックの手順に従って、アプリケーション・メトリック・サポートを追加できます。

Hystrix は、メトリック・データを取得してメトリック・コレクターに公開する機能を提供します。Hystrix を prometheus に公開するには、以下のようにして metric_collector パッケージも追加する必要があります。
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

`metric_collector` の他に、追加ファイル `prometheus_collector.go` を Go アプリケーションに追加する必要があります。このファイルは[こちら](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go)にあります。 このファイルは `plugins` パッケージに追加する必要があります。

2 つの追加インポートが必要です。
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

`main` 関数に、以下のステートメントを追加します。
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

プロジェクトを実行し、`/metrics` 経路に移動すると、Hystrix が提供するメトリックが表示されます。
