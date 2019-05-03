---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: prometheus go, application metrics go, view metrics go app, add metrics go, promhttp go, autoscaling go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go 앱에서 애플리케이션 메트릭 사용
{: #go-appmetrics}

애플리케이션 메트릭은 애플리케이션의 성능을 모니터하는 데 필요합니다. 애플리케이션이 계속하여 효율적으로 실행되도록 하려면 CPU, 메모리, 대기 시간, HTTP 메트릭과 같은 메트릭을 실시간으로 보는 것이 중요합니다. 메트릭에 의존하는 Cloud Foundry의 [Auto-Scaling](/docs/services/Auto-Scaling?topic=services/Auto-Scaling-get-started#get-started)과 같은 클라우드 서비스를 사용하여 현재 워크로드에 맞게 동적으로 인스턴스를 스케일링할 수 있습니다. 애플리케이션 메트릭을 사용하면 인스턴스를 확장하거나 축소하거나, 비용을 낮게 유지하기 위해 더 이상 필요하지 않은 인스턴스를 정리할 시기를 정확하게 알 수 있습니다.

애플리케이션 메트릭은 시계열 데이터로 캡처됩니다. 캡처된 메트릭을 집계하고 시각화하면 다음과 같은 공통 성능 문제점을 식별하는 데 도움이 될 수 있습니다.

* 일부 또는 모든 라우트의 느린 HTTP 응답 시간
* 애플리케이션의 낮은 처리량
* 성능 저하를 야기하는 수요 급증
* 처리량/로드 레벨에 대한 예상보다 높은 CPU 사용량
* 높거나 증가하는 메모리 사용량(잠재적 메모리 누수)

## 기존 Go 애플리케이션에 애플리케이션 메트릭 추가
{: #go-add-appmetrics-existing}

Go 애플리케이션에 성능 모니터링을 추가하기 위해 `promhttp` 패키지에서 제공되는 종합적 메트릭 집계를 사용할 수 있습니다.

`promhttp` 패키지에는 [Prometheus 구성](https://github.com/prometheus/client_golang){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 포함한 많은 확장점이 있습니다.

1. 예를 들어, 다음과 같은 간단한 “Hello World” Go + Gin 애플리케이션을 사용하십시오.
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

2. 다음 명령을 사용하여 패키지를 가져오십시오.
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. 다음과 같이 가져오기에 `promhttp`를 추가하십시오.
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  추가 메트릭 라우트도 라우터에 추가해야 합니다.

  이제 개정된 코드는 다음 예와 유사합니다.
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

## 스타터 킷에서 애플리케이션 메트릭 사용
{: #go-starterkits-appmetrics}

스타터 킷에서 작성된 서버 측 Go 애플리케이션은 `http://<hostname>:<port>/metrics` 아래에 [Prometheus 엔드포인트](https://prometheus.io/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")와 함께 자동으로 제공됩니다. 이 엔드포인트의 코드는 `server.go`에 있습니다.

자세한 정보는 [Prometheus의 GitHub 저장소](https://github.com/prometheus/client_golang/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.
