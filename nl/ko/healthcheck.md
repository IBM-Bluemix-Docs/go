---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

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

상태 검사에서는 서버 측 애플리케이션이 올바르게 작동하는지 여부를 판별하는 간단한 메커니즘을 제공합니다. [Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 및 [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")와 같은 클라우드 환경은 서비스의 인스턴스가 트래픽을 허용할 준비가 되었는지 여부를 판별하기 위해 주기적으로 상태 엔드포인트를 폴링하도록 구성될 수 있습니다.

## 상태 검사 개요
{: #go-healtcheck-overview}

상태 검사에서는 서버 측 애플리케이션이 올바르게 작동하는지 여부를 판별하는 간단한 메커니즘을 제공합니다. 이 상태 검사는 일반적으로 HTTP를 통해 액세스되며 표준 리턴 코드를 사용하여 작동 또는 작동 중지 상태를 표시합니다. 상태 검사의 리턴 값은 변수이지만, `{"status": "UP"}`과 같은 최소한의 JSON 응답은 전형적입니다.

Kubernetes는 미묘한 개념의 프로세스 상태를 가집니다. 이는 두 개의 프로브를 정의합니다.

- _**준비**_ 프로브는 프로세스가 요청을 처리할 수 있는지(라우팅 가능) 여부를 표시하는 데 사용됩니다.

  Kubernetes는 실패한 준비 프로브를 포함한 컨테이너로 작업을 라우팅하지 않습니다. 서비스가 초기화를 완료하지 않았거나 사용 중이거나 과부하되었거나 요청을 처리할 수 없는 경우 준비 프로브가 실패할 수 있습니다.

- _**활성**_ 프로브는 프로세스를 다시 시작할지 여부를 표시하는 데 사용됩니다.

  Kubernetes는 실패한 활성 프로브를 포함한 컨테이너를 중지하고 다시 시작합니다. 예를 들어, 메모리가 고갈되었거나 내부 프로세스가 실패한 후 컨테이너가 올바르게 중지하지 않은 경우, 활성 프로브를 사용하여 복구 불가능 상태의 프로세스를 정리하십시오.

비교를 위해 참고로 Cloud Foundry는 하나의 상태 엔드포인트를 사용합니다. 이 검사가 실패하면 프로세스가 다시 시작되지만, 성공하면 요청이 이 엔드포인트로 라우팅됩니다. 이 환경에서 엔드포인트는 프로세스가 활성일 때 최소한도로 성공합니다. 서비스가 다시 시작 사이클을 피하도록 초기화를 완료할 때까지 상태 검사를 지연시키도록 초기 지연이 구성됩니다.

다음 표에서는 준비, 활성 및 단일 상태 엔드포인트가 제공할 응답에 대한 안내를 제공합니다.

|시/도    | 준비                   | 활성                   | 상태                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | 확인이 없으면 로드 없음       | 확인이 없으면 다시 시작      | 확인이 없으면 다시 시작     |
| 시작 | 503 - 사용 불가능           | 200 - 확인                   |지연을 사용하여 테스트 방지   |
| 작동       | 200 - 확인                    | 200 - 확인                   | 200 - 확인                  |
| 중지 | 503 - 사용 불가능           | 200 - 확인                   | 503 - 사용 불가능         |
|작동 중지     | 503 - 사용 불가능           | 503 - 사용 불가능          | 503 - 사용 불가능         |
|오류 발생  | 500 - 서버 오류          | 500 - 서버 오류         | 500 - 서버 오류        |

## 기존 Go 앱에 상태 검사 추가
{: #go-add-healthcheck-existing}

다음 예에 표시된 대로 새 라우트를 도입하여 기존 `Gin-Gonic` 애플리케이션에 최소 상태 또는 활성 검사를 추가할 수 있습니다.
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

`/health` 엔드포인트에 액세스하여 브라우저로 앱의 상태를 검사하십시오.

[`http-healthcheck`](https://github.com/robzienert/http-healthcheck){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")와 같이 광범위한 라이브러리를 사용할 수 있으며, 이를 통해 지원 서비스에 대해 실행되는 캐싱 검사 지원으로 확장 가능한 상태 검사의 정의가 허용됩니다. 이 경우 상태 검사 패키지에서 빌드된 강력하고 자세한 준비 검사로부터 예제의 단순 활성 테스트를 분리하려고 합니다.

## Go 스타터 킷 앱의 상태 검사 엔드포인트에 액세스
{: #go-healthcheck-starterkit}

기본적으로 스타터 킷을 사용하여 Go 앱을 생성할 때
기본(권한 없음) 상태 검사 엔드포인트는 `/health`에서 앱 상태(작동/작동 중지)를 검사하는 데 사용 가능합니다.

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

## 준비 및 활성 프로브에 대한 권장사항
{: #go-recommend-healthcheck}

준비 프로브에는 다운스트림 서비스가 사용 불가능한 경우 허용 가능한 대비책이 없을 때 결과에 서비스를 다운스트림하기 위한 연결 가능성이 포함되어야 합니다. 인프라가 대신 검사하므로, 다운스트림 서비스에서 직접 제공되는 상태 검사를 호출하는 것을 의미하지는 않습니다. 대신 서비스를 다운스트림하기 위해 애플리케이션이 가진 기존 참조 상태를 확인할 것을 고려하십시오. 이는 WebSphere MQ에 대한 JMS 연결이거나 초기화된 Kafka 이용자 또는 생성자일 수 있습니다. 내부 참조의 실행 가능성을 확인하여 서비스를 다운스트림하는 경우, 결과를 캐싱하여 상태 검사가 애플리케이션에 미치는 영향을 최소화하십시오.

이와 대조적으로, 실패로 인해 프로세스가 즉시 종료되므로 활성 프로브는 검사된 사항에 대해 의도적입니다. 상태 코드가 `200`인 `{"status": "UP"}`을 항상 리턴하는 단순 HTTP 엔드포인트는 합리적 선택사항입니다.

## Kubernetes에서 준비 및 활성 프로브 구성
{: #go-config-probes-healthcheck}

Kubernetes 배치와 함께 활성 및 준비 프로브를 선언하십시오. 두 프로브 모두 동일한 구성 매개변수를 사용합니다.

* kubelet은 첫 번째 프로브 전까지 `initialDelaySeconds` 동안 대기합니다.

* kubelet은 `periodSeconds`초마다 서비스를 프로브합니다. 기본값은 1입니다.

* 프로브는 `timeoutSeconds`초 후에 제한시간이 초과됩니다. 기본값 및 최소값은 1입니다.

* 실패 후 `successThreshold`회 성공하면 프로브가 성공합니다. 기본값 및 최소값은 1입니다. 활성 프로브에 대한 값은 1이어야 합니다.

* 팟(Pod)이 시작되고 프로브가 실패하면, Kubernetes는 `failureThreshold`회 시도하여 팟(Pod)을 다시 시작한 다음 포기합니다. 최소값은 1이고 기본값은 3입니다.
    - 활성 프로브의 경우 "포기" 는 팟을 다시 시작하는 것을 의미합니다.
    - 준비 프로브의 경우 "포기"는 팟(Pod)을 `Unready`로 표시하는 것을 의미합니다.

다시 시작 사이클을 피하려면 서비스 초기화에 걸리는 시간보다 더 길게 `livenessProbe.initialDelaySeconds`를 설정하십시오. 그런 다음 `readinessProbe.initialDelaySeconds`에 더 짧은 값을 사용하여 준비가 되는 즉시 서비스로 요청을 라우팅할 수 있습니다.

구성 예는 다음과 유사합니다.
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

자세한 정보는 [활성 및 준비 프로브를 구성](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")하는 방법을 참조하십시오.
