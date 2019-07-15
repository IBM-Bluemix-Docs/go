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

# Go 앱에서 추적 설정
{: #go-e2e-tracing}

다음 튜토리얼에서는 Go 애플리케이션 추적을 위한 OpenTracing 및 Jaeger 패키지에 초점을 맞춥니다. Jaeger 사용에 대한 자세한 정보는 [Jaeger 문서 포털](https://www.jaegertracing.io/docs/1.11/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 참조하십시오.

다음 단계에서 두 개의 소형 애플리케이션(프론트 엔드와 백엔드)은 Jaeger 모듈을 사용하여 두 엔드포인트 사이를 추적하는 데 사용됩니다. 처음부터 시작하거나 기존 Go 애플리케이션에 여기에 설명되어 있는 원칙을 적용할 수 있습니다.

## 1단계. OpenTracing 및 Jaeger 패키지 설치 및 사용
{: #install-go-opentracing}

1. Go 애플리케이션의 `Gopkg.toml` 파일과 동일한 위치에 다음 명령을 입력하여 종속성 목록에 필수 패키지를 추가하십시오.
  ```go
  dep ensure -add "github.com/opentracing/opentracing-go"
  dep ensure -add "github.com/uber/jaeger-client-go"
  dep ensure -add "github.com/uber/jaeger-lib/metrics/prometheus"
  ```
  {: codeblock}

2. Go 서버 코드에 다음 행을 추가하십시오.
  ```go
  import(
  "github.com/opentracing/opentracing-go"
	"github.com/opentracing/opentracing-go/ext"
	"github.com/uber/jaeger-client-go"
	jaegerprom "github.com/uber/jaeger-lib/metrics/prometheus"
  )
  ```
  {: codeblock}

### 서버 애플리케이션에 추적 추가
{: #add-tracing-go}

서버 애플리케이션에 추적을 추가하려면 몇 가지 명령문이 필요합니다. 먼저 추적 프로그램을 작성해야 합니다.

추적 프로그램을 작성하려면 다음 항목을 제공하십시오.
 * 트랜스포터
 * 리포터
 * 선택적 메트릭 익스포터
 
이 튜토리얼에서 Jaeger는 Prometheus 스타일의 메트릭을 내보냅니다. 메트릭 오브젝트를 사용하면 Jaeger가 Prometheus에 메트릭을 보고할 수 있습니다.

1. 메트릭 오브젝트를 작성하려면 main 함수에 다음 명령문을 추가하십시오.
  ```go
  factory := jaegerprom.New()
  metrics := jaeger.NewMetrics(factory, map[string]string{"lib": "jaeger"})
  ```
  {: codeblock}

2. 트랜스포터에 다음 명령문을 제공하십시오. 이 명령문은 추적을 보낼 위치를 Jaeger에 알립니다. 로컬 개발의 경우 대상은 `5775` 포트의 `localhost`입니다. 호스트 이름이 변경될 수 있지만 대부분의 경우 포트는 `5775`입니다.
  ```go
  transport, err := jaeger.NewUDPTransport("<hostname>:<port>", 0)
  if err != nil {
	log.Errorln(err.Error())
  }
  ```
  {: codeblock}

3. 리포터는 추적 데이터를 보고하는 방법을 Jaeger에 알립니다. 다음 세 가지 유형의 리포터가 사용됩니다.
  * 로깅 - 범위 데이터를 로그로 인쇄
  * 원격 - Jaeger 에이전트로 범위 데이터 전송
  * 컴포지트 - 둘 이상의 리포터 결합

  Go 스타터 킷이 Logrus를 사용하여 데이터를 로그하므로 Jaeger 범위 보고는 기본적으로 가능하지 않습니다. 하지만 Jaeger는 다음 명령문으로 로깅 어댑터를 추가하여 사용할 수 있는 로깅 인터페이스를 지원합니다.
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

4. 로그와 보고서를 모두 포괄하는 리포터를 작성하려면 다음 명령문을 추가하십시오.
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

5. 샘플러 오브젝트는 발생한 상황 또는 범위가 보고되는 빈도를 판별합니다. 개발 목적으로 애플리케이션은 수신되는 모든 범위를 보고합니다. 하지만 프로덕션의 경우 모든 범위를 보고하는 것은 실현 불가능할 수 있습니다. 모든 범위를 보고하기 위해 ConstSampler 오브젝트를 사용할 수 있습니다.
  ```go
  sampler := jaeger.NewConstSampler(true)
  ```
  {: codeblock}

6. 리포터, 샘플러 및 메트릭 오브젝트를 작성했으므로 추적 프로그램을 작성할 수 있습니다.
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

7. 클라이언트 요청에서 범위 데이터를 추출하거나 새 루트 범위 오브젝트를 작성하는 미들웨어 함수를 추가하십시오.
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

8. 라우터에 미들웨어를 추가하십시오. 엔드포인트를 작성하기 전에 이 명령문을 추가**해야 합니다**.
  ```go
  router.Use(OpenTracing)
  ...
  router.GET("/health", routes.HealthGET)
  ```
  {: codeblock}

  이 명령문은 범위를 추출한 다음 새 범위 데이터를 보고할 수 있는 애플리케이션을 설정합니다. 기본적으로 모든 Go 스타터 킷은 서버 측 OpenTracing을 완전히 구현합니다.

9. 하나의 서비스에서 다른 서비스로 범위 데이터를 전송하려면 몇 가지 명령문을 추가해야 합니다. 다음 샘플 요청을 확인하십시오.
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)
  client.Do(req)
  ```
  {: codeblock}

10. 이 요청과 함께 범위 데이터를 전송하려면 요청을 추가한 후 두 개의 명령문을 추가하십시오.
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)

  span := opentracing.SpanFromContext(c.Request.Context())
  opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

  client.Do(req)
  ```
  {: codeblock}

## 2단계. 로컬 Jaeger 서버 설정
{: #setup-jaeger-server}

Jaeger 서버는 세 가지 개별 서비스로 구성됩니다.
 * 에이전트
 * 콜렉터
 * 조회
 
`UDPTransport` 오브젝트를 사용하여 Go 서비스를 에이전트에 연결합니다. 그런 다음 에이전트는 데이터베이스에 범위 데이터를 저장하는 콜렉터에 데이터를 보고합니다. 조회 서비스를 사용하면 사용자가 데이터베이스에서 범위를 검색할 수 있습니다. 필요에 따라 콜렉터 및 조회 서비스의 수를 스케일링할 수 있는 반면, 모든 서비스에서 고유 에이전트에 연결해야 하므로 유연성을 위해 서비스가 구분됩니다.

하지만 로컬 개발을 위해 Jaeger는 에이전트, 콜렉터, 데이터베이스 및 조회 서비스가 패키징되어 함께 제공되는 올인원 서비스를 제공합니다. 이 구성은 로컬 개발에 유용하지만 프로덕션에서는 이를 사용하지 않습니다.

클라우드에 앱을 배치하기 전에 추적을 로컬로 테스트할 수 있습니다.

Kubernetes를 사용하여 컨테이너에 Jaeger를 배치하는 데 대한 자세한 정보는 [이 단계](#jaeger-kube)를 참조하십시오.

올인원 서비스를 로컬로 실행하려면 다음 명령을 실행하십시오.
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

`5775` 포트에 에이전트를 연결할 수 있는 반면, 조회는 `16686` 포트에 연결할 수 있습니다.

### 배치된 Jaeger 서버를 Kubernetes로 설정
{: #jaeger-kube}

Jaeger는 로컬 개발의 경우와 같이 Kubernetes 개발을 위해 올인원 서비스를 제공합니다. 프로덕션 코드가 아닌 개발을 위해서만 올인원 서비스를 사용하십시오. 프로덕션을 위해 Kubernetes에 배치하는 데 대해 자세히 알아보려면 [Jaeger Kubernetes 템플리트 안내서](https://github.com/jaegertracing/jaeger-kubernetes#production-setup){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.

Jaeger 서버를 배치하려면 다음 단계를 완료하십시오.
1. `ibmcloud cs cluster-config<cluster name>`를 실행하여 클러스터가 설정되었는지 확인하고 지시사항을 따르십시오.
2. 다음 명령을 실행하십시오.
  ```go
  kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
  ```
  {: codeblock}

  이 명령은 Jaeger 에이전트, 콜렉터 및 조회를 Kubernetes 클러스터에 배치합니다.
3. Kubernetes에 Go 애플리케이션을 배치하기 전에, Jaeger 에이전트를 정확하게 가리키도록 UDP 전송을 업데이트해야 합니다. 이전 단계의 `kubectl` 명령은 내부 엔드포인트 `"jaeger-agent:5775"`를 작성합니다. 이 새 엔드포인트로 전송을 업데이트하십시오.
  ```go
  transport, err := jaeger.NewUDPTransport("jaeger-agent:5775", 0)
  ```
  {: codeblock}

4. 애플리케이션이 배치된 후 <*공용 클러스터 IP*>:<*포트*>로 이동하여 추적 데이터를 볼 수 있습니다. `bx cs workers<*cluster name*>`를 실행하여 공용 클러스터 IP 주소를 찾을 수 있습니다.
`kubectl get service jaeger-query`를 실행하여 포트를 찾을 수 있습니다.

## 3단계. 시나리오 예 테스트
{: #test-go-tracing}

이전 단계를 수행하는 경우 추적을 지원하는 두 개의 개별 Go 애플리케이션을 작성하는 것이 간단합니다. 다음 코드를 사용하여 프로젝트 중 하나에 라우트를 추가할 수 있습니다.
```go
client := http.Client{}
req, _ := http.NewRequest("GET", "http://localhost:<other port>", nil)

span := opentracing.SpanFromContext(c.Request.Context())
opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

client.Do(req)
```
{: codeblock}

이 라우트는 하나의 애플리케이션에서 다른 애플리케이션으로 `GET` 요청을 보냅니다.

범위를 보려면 `http://localhost:16686`으로 이동하십시오. 서비스, 오퍼레이션 및 태그별로 추적을 검색한 다음 **추적 찾기**를 클릭하십시오.

![Jaeger UI](images/JaegerUI.png "Jaeger UI")

특정 추적을 클릭하여 이에 대한 자세한 정보를 보십시오.
![추적 예](images/TraceExample.png "추적 예")
