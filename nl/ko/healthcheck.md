---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: healthcheck go, add healthcheck, healthcheck endpoint, readiness go, liveness go, endpoint go, probes go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go 앱에서 상태 검사 사용
{: #go-healthcheck}

상태 검사는 서버 측 애플리케이션이 올바르게 작동하는지 여부를 판별할 수 있는 단순 메커니즘을 제공합니다. [Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 및 [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")와 같은 클라우드 환경은 서비스의 인스턴스가 트래픽을 수신할 준비가 되어 있는지 판별하기 위해 상태 엔드포인트를 주기적으로 폴링하도록 구성될 수 있습니다.

## 상태 검사 개요
{: #go-healtcheck-overview}

상태 검사는 서버 측 애플리케이션이 올바르게 작동하는지 여부를 판별할 수 있는 단순 메커니즘을 제공합니다. 이 상태 검사는 일반적으로 HTTP를 통해 액세스되며 표준 리턴 코드를 사용하여 작동 또는 작동 중단 상태를 표시합니다. 상태 검사의 리턴값은 변수이지만, `{"status": "UP"}`과 같은 최소한의 JSON 응답이 일반적입니다.

Kubernetes는 프로세스 상태에 대한 미묘한 개념을 가지고 있습니다. 이는 다음과 같은 두 가지 프로브를 정의합니다.

- _**준비 상태(readiness)**_ 프로브는 프로세스가 요청을 처리할 수 있는지(라우팅 가능) 여부를 표시하는 데 사용됩니다.

  Kubernetes는 실패한 준비 상태 프로브가 있는 컨테이너로 작업을 라우팅하지 않습니다. 준비 상태 프로브는 서비스의 초기화가 완료되지 않는 경우 또는 서비스가 사용 중인거나, 과부화되거나, 요청을 처리할 수 없는 경우 실패할 수 있습니다.

- _**활성 상태(liveness)**_ 프로브는 프로세스의 다시 시작 여부를 표시하는 데 사용됩니다.

  Kubernetes는 실패한 활성 상태 프로브가 있는 컨테이너를 중지하고 다시 시작합니다. 예를 들어, 메모리가 고갈되었거나 내부 프로세스가 실패한 후 컨테이너가 올바르게 중지하지 않은 경우, 활성 상태 프로브를 사용하여 복구 불가능 상태의 프로세스를 정리하십시오.

비교를 위한 참고사항으로 Cloud Foundry는 하나의 상태 엔드포인트를 사용합니다. 이 검사에 실패하는 경우 프로세스가 다시 시작되지만, 성공하는 경우에는 요청이 라우팅됩니다. 이 환경에서 엔드포인트는 프로세스가 활성 상태일 때 최소한으로 성공합니다. 초기화 지연은 서비스가 다시 시작 주기를 실행하지 않기 위해 초기화를 완료할 때까지 상태 검사를 지연시키도록 구성됩니다.

다음 표는 준비 상태, 활성 상태 및 단일 상태 엔드포인트가 제공해야 하는 응답에 대한 지침을 제공합니다.

| 상태     | 준비 상태(readiness)        | 활성 상태(liveness)        | 단일 상태(health)         |
|----------|-----------------------------|----------------------------|---------------------------|
|          | 정상 상태가 아니면 로드되지 않음 | 정상 상태가 아니면 다시 시작 | 정상 상태가 아니면 다시 시작 |
| 시작 중  | 503 - 사용 불가능           | 200 - 정상                 | 테스트하지 않으려면 지연을 사용함 |
| 작동     | 200 - 정상                  | 200 - 정상                 | 200 - 정상                |
| 중지 중  | 503 - 사용 불가능           | 200 - 정상                 | 503 - 사용 불가능         |
| 작동 중단 | 503 - 사용 불가능          | 503 - 사용 불가능          | 503 - 사용 불가능         |
| 오류 발생 | 500 - 서버 오류            | 500 - 서버 오류            | 500 - 서버 오류           |
{: caption="표 1. HTTP 상태 코드" caption-side="bottom"}

## 기존 Go 앱에 상태 검사 추가
{: #go-add-healthcheck-existing}

다음 예에 표시된 대로 새 라우트를 도입하여 기존 `Gin-Gonic` 애플리케이션에 최소 상태 또는 활성 상태 검사를 추가할 수 있습니다.
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

`/health` 엔드포인트에 액세스하여 브라우저에서 앱의 상태를 확인하십시오.

[`http-healthcheck`](https://github.com/robzienert/http-healthcheck){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")와 같이 광범위한 라이브러리를 사용할 수 있으며, 이를 통해 지원 서비스에 대해 실행되는 캐싱 검사 지원으로 확장 가능한 상태 검사의 정의가 허용됩니다. 이 경우 사용자는 예제의 단순한 활성 상태 테스트를 상태 검사 패키지에서 빌드된 더 강력하고 자세한 준비 상태 검사와 구분하기를 원합니다.

## Go 스타터 킷 앱의 상태 검사 엔드포인트에 액세스
{: #go-healthcheck-starterkit}

기본적으로 스타터 킷을 사용하여 Go 앱을 생성할 때
기본(권한 없음) 상태 검사 엔드포인트가 `/health`에서 앱 상태(작동/작동 중단)를 검사하는 데 사용 가능합니다.

상태 검사 엔드포인트 코드는 다음 `/routers/health.go` 파일에서 제공됩니다.
```go
package routers

import (
  "github.com/gin-gonic/gin"
)

func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
}
```
{: codeblock}

## 준비 상태 및 활성 상태 프로브에 대한 권장사항
{: #go-recommend-healthcheck}

준비 상태 프로브는 다운스트림 서비스가 사용 불가능한 경우 허용 가능한 폴백이 없을 때 결과에서 다운스트림 서비스에 대한 연결의 실행 가능성을 포함해야 합니다. 인프라가 사용자를 위해 확인함에 따라 다운스트림 서비스에서 직접 제공되는 상태 검사를 호출할 필요가 없습니다. 대신 애플리케이션이 가지는 다운스트림 서비스에 대한 기존 참조의 상태 확인을 고려하십시오. 예를 들어, 참조는 WebSphere MQ에 대한 JMS 연결 또는 초기화된 Kafka 이용자 또는 생성자일 수 있습니다. 다운스트림 서비스에 대한 내부 참조의 실행 가능성을 확인하는 경우 애플리케이션에 있는 영향 상태 검사를 최소화하도록 결과를 캐시하십시오.

반대로 활성 상태 프로브는 실패 시 프로세스가 즉각적으로 종료되므로 확인되는 항목에 대해 신중합니다. 상태 코드 `200`과 함께 `{"status": "UP"}`을 항상 리턴하는 단순 HTTP 엔드포인트는 합리적 선택사항입니다.

## Kubernetes에서 준비 상태 및 활성 상태 프로브 구성
{: #go-config-probes-healthcheck}

Kubernetes 배치와 함께 활성 상태 및 준비 상태 프로브를 선언하십시오. 두 프로브 모두 동일한 구성 매개변수를 사용합니다.

* kubelet은 초기 프로브 전에 `initialDelaySeconds` 동안 기다립니다.

* kubelet은 `periodSeconds`초마다 서비스를 프로브합니다. 기본값은 1입니다.

* 프로브는 `timeoutSeconds`초 후에 제한시간이 초과됩니다. 기본값 및 최소값은 1입니다.

* 프로브는 실패 후 `successThreshold`번 성공하면 성공 상태가 됩니다. 기본값 및 최소값은 1입니다. 활성 상태 프로브의 값은 1이어야 합니다.

* 팟(Pod)이 시작되고 프로브가 실패하는 경우 Kubernetes는 다시 시작을 `failureThreshold`번 시도한 후 포기합니다. 최소값은 1이고 기본값은 3입니다.
    - 활성 상태 프로브의 경우 "포기"는 팟(Pod)의 다시 시작을 의미합니다.
    - 준비 상태 프로브의 경우 "포기"는 팟(Pod) `Unready` 표시를 의미합니다.

주기를 다시 시작하지 않으려면 서비스를 초기화하는 데 소요되는 시간보다 길게 `livenessProbe.initialDelaySeconds`를 설정하십시오. 그런 다음 준비되는 즉시 `readinessProbe.initialDelaySeconds`에 대한 더 짧은 값을 사용하여 요청을 서비스로 라우팅할 수 있습니다.

다음 구성 예제를 참조하십시오.
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```
{: codeblock}

자세한 정보는 [활성 상태 및 준비 상태 프로브 구성](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 방법을 참조하십시오.
