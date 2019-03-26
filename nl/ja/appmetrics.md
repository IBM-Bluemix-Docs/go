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

# Go アプリでのアプリケーション・メトリックの使用
{: #appmetrics}

アプリケーション・メトリックは、アプリケーションのパフォーマンスをモニターするのに重要です。 CPU、メモリー、待ち時間、HTTP などのメトリックをライブで表示できることは、時間の経過とともにアプリケーションが効果的に実行されていることを確認するために不可欠です。 メトリックに依存するクラウド・サービス (例えば、Cloud Foundry の[自動スケーリング](/docs/services/Auto-Scaling/index.html)など) を使用して、現行作業負荷に合わせて動的にインスタンスをスケーリングできます。 アプリケーション・メトリックを使用することによって、インスタンスのスケーリングや不要インスタンスのクリーンアップを行うタイミングを正確に把握して、コストを低く抑えることができます。

アプリケーション・メトリックは、時系列データとして収集されます。 収集されたメトリックを集約して視覚化することは、以下のような一般的なパフォーマンス上の問題を特定するのに役立ちます。

* 一部またはすべての経路で HTTP 応答時間が遅くなる
* アプリケーションのスループットが低下する
* 需要の急上昇によりスローダウンが発生する
* スループット/負荷のレベルで、予期される CPU 使用量を上回る
* メモリー使用量が多いか、増加している (メモリー・リークの可能性)

## 既存の Go アプリケーションへのアプリケーション・メトリックの追加
{: #add-appmetrics-existing}

Go アプリケーションにパフォーマンス・モニターを追加するために、`promhttp` パッケージが提供するメトリックの包括的な集約を使用できます。

`promhttp` パッケージには、[Prometheus 構成](https://github.com/prometheus/client_golang)など、多くの拡張ポイントがあります。

1. 例として、以下の単純な Go + Gin アプリケーション「Hello World」を使用します。
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

2. 以下のコマンドを使用して、パッケージを取得します。
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. `promhttp` をインポートに追加します。
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  追加のメトリック経路もルーターに追加する必要があります。

  変更後のコードは、以下の例のようになります。
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

## スターター・キットでのアプリケーション・メトリックの使用
{: #starterkits-appmetrics}

スターター・キットで作成されたサーバー・サイドの Go アプリケーションには、`http://<hostname>:<port>/metrics` の下に [Prometheus エンドポイント](https://prometheus.io/)が自動的に付属します。 このエンドポイントのコードは `server.go` にあります。

詳しくは、[GitHub Repository for Prometheus](https://github.com/prometheus/client_golang/) を参照してください。
