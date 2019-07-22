---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: how to trace go apps, tracing go, jaeger go, opentracing go, jaeger packages, debug go app, troubleshoot go, go app help

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 在 Go 应用程序中设置跟踪
{: #go-e2e-tracing}

以下教程侧重于用于跟踪 Go 应用程序的 OpenTracing 和 Jaeger 包。有关使用 Jaeger 的更多信息，请参阅 [Jaeger 文档门户网站](https://www.jaegertracing.io/docs/1.11/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。

在以下步骤中，通过 Jaeger 模块使用两个小型应用程序（一个前端，一个后端）在两个端点之间进行跟踪。您可以从头开始，也可以将此处描述的原则应用于现有 Go 应用程序。

## 步骤 1. 安装和启用 OpenTracing 和 Jaeger 包
{: #install-go-opentracing}

1. 在与 Go 应用程序的 `Gopkg.toml` 文件相同的位置中，输入以下命令以将必需的包添加到依赖项列表：
  ```go
  dep ensure -add "github.com/opentracing/opentracing-go"
  dep ensure -add "github.com/uber/jaeger-client-go"
  dep ensure -add "github.com/uber/jaeger-lib/metrics/prometheus"
  ```
  {: codeblock}

2. 将以下行添加到 Go 服务器代码：
  ```go
  import(
  "github.com/opentracing/opentracing-go"
	"github.com/opentracing/opentracing-go/ext"
	"github.com/uber/jaeger-client-go"
	jaegerprom "github.com/uber/jaeger-lib/metrics/prometheus"
  )
  ```
  {: codeblock}

### 向服务器应用程序添加跟踪
{: #add-tracing-go}

需要一些语句以向服务器应用程序添加跟踪。首先，必须创建跟踪器。

要创建跟踪器，请提供以下项：
 * 传输器
 * 报告器
 * 可选度量值导出器
 
在本教程中，Jaeger 导出 Prometheus 样式的度量值。度量值对象允许 Jaeger 向 Prometheus 报告度量值。

1. 要创建度量值对象，请向 main 函数添加以下语句：
  ```go
  factory := jaegerprom.New()
  metrics := jaeger.NewMetrics(factory, map[string]string{"lib": "jaeger"})
  ```
  {: codeblock}

2. 针对传输器提供以下语句，用于告知 Jaeger 将其跟踪发送到何处。对于本地开发，目标是端口 `5775` 处的 `localhost`。虽然主机名可能更改，但是端口在大多数情况下为 `5775`。
  ```go
  transport, err := jaeger.NewUDPTransport("<hostname>:<port>", 0)
  if err != nil {
	log.Errorln(err.Error())
  }
  ```
  {: codeblock}

3. 报告器告知 Jaeger 如何报告其跟踪数据。使用三种类型的报告器：
  * 日志记录 - 将范围数据打印为日志
  * 远程 - 将范围数据发送到 Jaeger 代理程序
  * 组合 - 组合多个报告器

  因为 Go 入门模板工具包使用 Logrus 来记录数据，缺省情况下无法报告 Jaeger 范围。但是，Jaeger 支持一个日志记录接口，可通过添加具有以下语句的日志记录适配器来使用此接口：
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

4. 要创建日志和报告所跨越的报告器，请添加以下语句：
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

5. 采样器对象确定报告范围的情况或频率。对于开发目的，应用程序可报告它收到的所有范围。但是，对于生产，报告所有范围可能不可行。要报告所有范围，您可以使用 ConstSampler 对象：
  ```go
  sampler := jaeger.NewConstSampler(true)
  ```
  {: codeblock}

6. 既然您已创建报告器、采样器和度量值对象，就可以创建跟踪器。
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

7. 添加中间件功能，此功能从客户机请求抽取范围数据或者创建新的根范围对象：
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

8. 将中间件添加到路由器。**必须**先添加此语句，然后再创建端点：
  ```go
  router.Use(OpenTracing)
  ...
  router.GET("/health", routes.HealthGET)
  ```
  {: codeblock}

  此语句可设置应用程序，先抽取一个范围，然后报告新范围数据。缺省情况下，所有 Go 入门模板工具包完全实现服务器端 OpenTracing。

9. 要将范围数据从一个服务发送到另一个服务，必须再添加几个语句。请参阅以下样本请求：
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)
  client.Do(req)
  ```
  {: codeblock}

10. 要随此请求一起发送范围数据，请在创建请求后添加两个语句：
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)

  span := opentracing.SpanFromContext(c.Request.Context())
  opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

  client.Do(req)
  ```
  {: codeblock}

## 步骤 2. 设置本地 Jaeger 服务器
{: #setup-jaeger-server}

Jaeger 服务器由三个不同的服务组成：
 * 代理程序
 * 收集器
 * 查询
 
Go 服务使用 `UDPTransport` 对象连接到代理程序。然后，代理程序向收集器报告数据，收集器将范围数据存储到数据库。然后，查询服务允许用户在数据库中检索范围。分隔了服务以实现灵活性，因为所有服务必须具有自己的要连接到的代理程序，而收集器和查询服务的数量可根据需求进行缩放。

但是，对于本地开发，Jaeger 提供一个一体化服务，此服务将代理程序、收集器、数据库和查询服务打包在一起。此配置对于本地开发很有用，但请不要将其用于生产。

在将应用程序部署到任何云之前，可在本地测试跟踪。

有关使用 Kubernetes 在容器中部署 Jaeger 的更多信息，请参阅[这些步骤](#jaeger-kube)。

要本地运行一体化服务，请运行以下命令：
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

可在端口 `5775` 上连接代理程序，并可将查询连接到端口 `16686`。

### 将已部署的 Jaeger 服务器设置为 Kubernetes
{: #jaeger-kube}

与本地开发一样，Jaeger 针对 Kubernetes 开发提供一个一体化服务。仅将一体化服务用于开发，而非生产代码。要了解有关针对生产部署到 Kubernetes 的更多信息，请参阅 [Jaeger Kubernetes 模板指南](https://github.com/jaegertracing/jaeger-kubernetes#production-setup){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。

要部署 Jaeger 服务器，请完成以下步骤：
1. 确保通过运行 `ibmcloud cs cluster-config <cluster name>` 设置集群，并遵循指示信息。
2. 运行以下命令：
  ```go
  kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
  ```
  {: codeblock}

  此命令将 Jaeger 代理程序、收集器和查询部署到 Kubernetes 集群。
3. 在将 Go 应用程序部署到 Kubernetes 之前，必须更新 UDP 传输以正确地指向 Jaeger 代理程序。先前步骤中的 `kubectl` 命令创建一个名为 `"jaeger-agent:5775"` 的内部端点。使用此新端点更新传输。
  ```go
  transport, err := jaeger.NewUDPTransport("jaeger-agent:5775", 0)
  ```
  {: codeblock}

4. 部署应用程序后，您可以通过转至 <*public cluster ip*>:<*port*> 来查看跟踪数据。您可以通过运行 `bx cs workers <*cluster name*>` 来查找静态集群 IP 地址。
您可以通过运行 `kubectl get service jaeger-query` 来查找端口。

## 步骤 3. 测试示例场景
{: #test-go-tracing}

在遵循先前步骤时，创建支持跟踪的两个单独的 Go 应用程序很简单。可以使用以下代码向其中一个项目添加路由：
```go
client := http.Client{}
req, _ := http.NewRequest("GET", "http://localhost:<other port>", nil)

span := opentracing.SpanFromContext(c.Request.Context())
opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

client.Do(req)
```
{: codeblock}

此路由将 `GET` 请求从一个应用程序发送到另一个应用程序。

要查看范围，请转至 `http://localhost:16686`。可以按服务、操作和标记搜索跟踪，然后单击**查找跟踪**。

![Jaeger UI](images/JaegerUI.png "Jaeger UI")

单击特定跟踪以查看有关该跟踪的更多信息：![跟踪示例](images/TraceExample.png "跟踪示例")
