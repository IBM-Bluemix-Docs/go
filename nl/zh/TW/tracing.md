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

# 在 Go 應用程式中設定追蹤
{: #e2e-tracing}

下列指導教學著重於用來追蹤 Go 應用程式的 Opentracing 及 Jaeger 套件。如需使用 Jaeger 的相關資訊，請參閱 [Jaeger 文件入口網站](https://www.jaegertracing.io/docs/)。

在下列步驟中，將使用兩個小型應用程式（一個用於前端，一個用於後端），利用 Jaeger 模組在兩個端點之間進行追蹤。您可以從頭開始，或將這裡說明的原則套用至現有的 Go 應用程式。

## 步驟 1. 安裝並啟用 Opentracing 及 Jaeger 套件
{: #install-packages}

1. 在與 Go 應用程式之 `Gopkg.toml` 檔案相同的位置中，輸入下列指令，將必要套件新增至相依關係清單：
  ```go
  dep ensure -add "github.com/opentracing/opentracing-go"
  dep ensure -add "github.com/uber/jaeger-client-go"
  dep ensure -add "github.com/uber/jaeger-lib/metrics/prometheus"
  ```
  {: codeblock}

2. 將下列幾行新增至 Go 伺服器程式碼：
  ```go
  import(
  "github.com/opentracing/opentracing-go"
	"github.com/opentracing/opentracing-go/ext"
	"github.com/uber/jaeger-client-go"
	jaegerprom "github.com/uber/jaeger-lib/metrics/prometheus"
  )
  ```
  {: codeblock}

### 將追蹤新增至伺服器應用程式
{: #tracing-go}

需要有一些陳述式，才能將追蹤新增至伺服器應用程式。首先，您必須建立一個追蹤器。

若要建立追蹤器，請提供下列項目：
 * 傳輸器
 * 報告程式
 * 選用的度量值匯出器
 
在本指導教學中，Jaeger 會匯出 Prometheus 樣式的度量值。metrics 物件可讓 Jaeger 將度量值報告給 Prometheus。

1. 若要建立 metrics 物件，請將下列陳述式新增至 main 函數：
  ```go
  factory := jaegerprom.New()
  metrics := jaeger.NewMetrics(factory, map[string]string{"lib": "jaeger"})
  ```
  {: codeblock}

2. 為傳輸器提供下列陳述式，以告知 Jaeger 要傳送其追蹤資料的位置。若為本端開發，目的地是埠 `5775` 上的 `localhost`。雖然主機名稱可能會變更，但在大部分情況下埠為 `5775`。
  ```go
  transport, err := jaeger.NewUDPTransport("<hostname>:<port>", 0)
  if err != nil {
				log.Errorln(err.Error())
  }
  ```
  {: codeblock}

3. 報告程式會告知 Jaeger 報告其追蹤資料的方式。使用了三種類型的報告程式：
  * 記載 - 將跨距資料列印為日誌
  * 遠端 - 將跨距資料傳送至 Jaeger 代理程式
  * 複合 - 結合多個報告程式

  因為 Go 入門範本套件使用 Logus 來記載資料，所以依預設無法報告 Jaeger 的跨距。不過，Jaeger 支援一個記載介面，其可透過下列陳述式新增記載配接器來使用：
  ```go
  type LogrusAdapter struct{}

  func (l LogrusAdapter) Error(msg string) {
	log.Errorf(msg)
  }

  func (l LogrusAdapter) Infof(msg string, args ...interface{}) {
	log.Infof(msg, args)
  }
  ```
  {: codeblock}

4. 若要建立同時記載及報告跨距的報告程式，請新增下列陳述式：
  ```go
  logAdapt := LogrusAdapter{}
  reporter := jaeger.NewCompositeReporter(
	jaeger.NewLoggingReporter(logAdapt),
	jaeger.NewRemoteReporter(transport,
		jaeger.ReporterOptions.Metrics(metrics),
		jaeger.ReporterOptions.Logger(logAdapt),
	),
  )
  defer reporter.Close()
  ```
  {: codeblock}

5. sampler 物件會決定報告跨距的狀況或頻率。為了進行開發，應用程式應該報告它收到的所有跨距。不過，若為正式作業，報告所有跨距可能不可行。若要報告所有跨距，您可以使用 ConstSpler 物件：
  ```go
  sampler := jaeger.NewConstSampler(true)
  ```
  {: codeblock}

6. 現在，您已建立 reporter、sampler 及 metrics 物件，接著可以建立追蹤器。
  ```go
  tracer, closer := jaeger.NewTracer("<application name>",
	sampler,
	reporter,
	jaeger.TracerOptions.Metrics(metrics),
  )
  defer closer.Close()

  opentracing.SetGlobalTracer(tracer)
  ```
  {: codeblock}

7. 新增從用戶端要求中擷取跨距資料，或建立新跨距物件的中介軟體函數：
  ```go
  func OpenTracing() gin.HandlerFunc {
	return func(c *gin.Context) {
		wireCtx, _ := opentracing.GlobalTracer().Extract(
			opentracing.HTTPHeaders,
			opentracing.HTTPHeadersCarrier(c.Request.Header))

		serverSpan := opentracing.StartSpan(c.Request.URL.Path,
			ext.RPCServerOption(wireCtx))
		defer serverSpan.Finish()
		c.Request = c.Request.WithContext(opentracing.ContextWithSpan(c.Request.Context(), serverSpan))
		c.Next()
	}
  }
  ```
  {: codeblock}

8. 將中介軟體新增至路由器。您**必須**在建立端點之前新增此陳述式：
  ```go
  router.Use(OpenTracing)
  ...
  router.GET("/health", routes.HealthGET)
  ```
  {: codeblock}

  此陳述式設定的應用程式可擷取跨距，然後報告新的跨距資料。依預設，所有 Go 入門範本套件都會完整實作伺服器端的 OpenTracing。

9. 若要將跨距資料從某個服務傳送到另一個服務，您必須新增一些其他的陳述式。請參閱下列範例要求：
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)
  client.Do(req)
  ```
  {: codeblock}

10. 若要隨這個要求一起傳送跨距資料，請在建立要求之後，新增兩個陳述式：
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)

  span := opentracing.SpanFromContext(c.Request.Context())
  opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

  client.Do(req)
  ```
  {: codeblock}

## 步驟 2. 設定本端 Jaeger 伺服器
{: #setup-jaeger-server}

Jaeger 伺服器由三種不同的服務組成：
 * 代理程式
 * 收集器
 * 查詢
 
Go 服務使用 `UDPTransport` 物件來連接至代理程式。代理程式接著將資料報告給收集器，這會將跨距資料儲存至資料庫。然後，查詢服務容許使用者擷取資料庫中的跨距。因為所有服務都必須有自己要連接的代理程式，而收集器及查詢服務數目可以根據需求來調整，所以為了彈性會將這些服務分開。

不過，為了進行本端開發，Jaeger 提供了全功能的服務，其隨附於包裝在一起的代理程式、收集器、資料庫及查詢服務。此配置對於本端開發很有用，但請勿在正式作業中使用它。

將應用程式部署至任何雲端之前，您可以在本端測試追蹤。

如需使用 Kubenetes 在容器中部署 Jaeger 的相關資訊，請參閱[這些步驟](#jaeger-kube)。

若要在本端執行全功能服務，請執行下列指令：
```bash
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 9411:9411 \
  jaegertracing/all-in-one:latest
 ```
{: codeblock}

代理程式可以在埠 `5775` 上進行連接，而查詢可以連接至埠 `16686`。

### 設定部署至 Kubernetes 的 Jaeger 伺服器
{: #jaeger-kube}

就像本端開發一樣，Jaeger 提供適用於 Kubernetes 開發的全功能服務。全功能服務僅限用於開發，而非正式作業程式碼。若要進一步瞭解如何針對正式作業部署至 Kubernetes，請參閱 [Jaeger Kubernetes 範本手冊](https://github.com/jaegertracing/jaeger-kubernetes#production-setup)。

若要部署 Jaeger 伺服器，請完成下列步驟：
1. 確定您已透過執行 `ibmcloud cs cluster-config <cluster name>` 來設定叢集，並遵循指示。
2. 請執行下列指令：
  ```go
  kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
  ```
  {: codeblock}

  此指令會將 Jaeger 代理程式、收集器及查詢部署至 Kubernetes 叢集。
3. 將 Go 應用程式部署至 Kubernetes 之前，您必須先更新「UDP 傳輸」，以正確地指向 Jaeger 代理程式。前一個步驟中的 `kubectl` 指令會建立名為 `"jaeger-agent:5775"` 的內部端點。請使用這個新的端點來更新傳輸。
  ```go
  transport, err := jaeger.NewUDPTransport("jaeger-agent:5775", 0)
  ```
  {: codeblock}

4. 部署應用程式之後，您可以移至 <*public cluster ip*>:<*port*> 來檢視追蹤資料。透過執行 `bx cs workers <*cluster name*>` 可尋找公用叢集 IP 位址。
透過執行 `kubectl get service jaeger-query` 則可尋找埠。

## 步驟 3. 測試範例情境
{: #example-scenario}

當您遵循先前的步驟時，即可簡單建立兩個支援追蹤的個別 Go 應用程式。您可以使用下列程式碼，將路徑新增至其中一個專案：
```go
client := http.Client{}
req, _ := http.NewRequest("GET", "http://localhost:<other port>", nil)

span := opentracing.SpanFromContext(c.Request.Context())
opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

client.Do(req)
```
{: codeblock}

這個路徑會將 `GET` 要求從某個應用程式傳送至另一個應用程式。

若要檢視跨距，請移至 `http://localhost:16686`。您可以依服務、作業和標籤來搜尋追蹤資料，然後按一下**尋找追蹤資料**。

![Jaeger 使用者介面](images/JaegerUI.png)

按一下特定追蹤以檢視其相關資訊：
![追蹤範例](images/TraceExample.png)
