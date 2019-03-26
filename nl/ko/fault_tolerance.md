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

# Go 앱에서 결함 허용 설정
{: #fault-tolerance}

결함 허용을 사용하면 컴포넌트가 실패하거나 응답하지 않는 경우에도 애플리케이션이 계속 실행됩니다. 기존 Go 애플리케이션에 결함 허용을 추가하거나 생성된 Go 애플리케이션에서 이러한 기능을 사용할 수 있습니다. 이 튜토리얼에서는 [Hystrix 패키지](https://godoc.org/github.com/afex/hystrix-go/hystrix)를 사용하여 Go 애플리케이션에 결함 허용 지원을 추가하는 데 초점을 맞춥니다.

## 기존 Go 앱에 결함 허용 추가
{: #add-fault-tolerance}

Go 애플리케이션의 `Gopkg.toml` 파일과 동일한 위치에 다음 명령을 입력하여 종속성 목록에 필수 패키지를 추가하십시오.
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

Hystrix를 사용하려면 미들웨어 함수를 추가해야 합니다.
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

사용 중인 Go 애플리케이션의 유형에 따라 핸들링 오류가 변경될 수 있습니다. 웹 앱의 경우 마이크로서비스가 오류를 지정하는 JSON 오브젝트를 리턴할 때 오류가 페이지 500으로 경로 재지정됩니다.

이 미들웨어는 구성된 명령인 문자열을 사용합니다. 다음과 같이 `main` 함수에서 명령 구성을 수행해야 합니다.
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Hystrix 명령을 구성한 후 다음 명령문을 사용하여 미들웨어를 라우터에 추가할 수 있습니다.
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Prometheus에 Hystrix 메트릭 노출(선택사항)
{: #hystrix-optional}

Prometheus에 Hystrix를 추가하기 전에 애플리케이션 메트릭을 사용하여 앱을 구성해야 합니다. [Go 앱에서 애플리케이션 메트릭 사용](/docs/go/appmetrics.html) 주제의 단계를 따라 앱 메트릭 지원을 추가할 수 있습니다.

Hystrix는 메트릭 데이터를 가져와 메트릭 콜렉터에 노출하는 기능을 사용자에게 제공합니다. Prometheus에 Hystrix를 노출하려면 metric_collector 패키지를 추가해야 합니다.
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

`metric_collector` 외에 추가 파일 `prometheus_collector.go`를 Go 애플리케이션에 추가해야 합니다. 이 파일은 [여기](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go)에 있습니다. 이 파일은 `plugins` 패키지에 추가해야 합니다.

두 가지 추가 가져오기가 필요합니다.
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

`main` 함수에서 다음 명령문을 추가하십시오.
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

프로젝트를 실행하고 `/metrics` 라우트로 이동하면 Hystrix에서 제공하는 메트릭이 표시됩니다.
