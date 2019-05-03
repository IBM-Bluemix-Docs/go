---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-12"

keywords: how to log in go, go logging, debug go apps, go troubleshooting, logrus go, go stdout

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go에 로그인
{: #logging-golang}

로그 메시지는 로그 항목이 작성되었을 때의 마이크로서비스 상태 및 활동에 대한 컨텍스트 정보가 포함된 문자열입니다. 로그는 서비스가 실패한 이유와 과정을 진단하는 데 필요하며 애플리케이션 상태 모니터링에서 [메트릭](/docs/go?topic=go-appmetrics#appmetrics)에 대한 지원 역할을 수행합니다.

클라우드 환경에서 프로세스의 일시적인 네이처를 고려할 때, 로그를 수집하여 다른 위치에(대개 분석을 위해 중앙 위치에) 보내야 합니다. 클라우드 환경에 로그인하는 가장 일관된 방식은 표준 출력 및 오류 스트림에 로그 항목을 전송하는 것이며, 인프라가 나머지를 처리하게 합니다.

## Go 앱에 Logrus 지원 추가
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")는 Go를 위한 인기 있는 로깅 프레임워크이며, 다음을 포함한 많은 기본 이점을 제공합니다. 
 * `stdout` 또는 `stderr`에 로깅
 * 다양한 추가 옵션
 * 구성 가능한 로그 메시지 레이아웃 및 패턴
 * 여러 로그 카테고리에 로그 레벨 사용

1. Logrus를 사용하려면 애플리케이션의 루트 디렉토리에서 다음 명령을 실행하십시오. 이 명령은 패키지를 설치하고 `Gopkg.toml` 파일에 추가합니다.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. 애플리케이션에서 이를 사용하려면 다음 코드 행을 추가하십시오.
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Stdout 출력**:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

어펜더, 로그 레벨 및 구성 세부사항으로 로그 메시지를 사용자 정의하는 방법에 대한 자세한 정보는 공식 [Logrus 문서](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.

## 다음 단계
{: #go-logging-next notoc}

각 배치 대상에서 로그 보기에 대해 자세히 보십시오.
* [Kubernetes 로그](https://kubernetes.io/docs/concepts/cluster-administration/logging/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
* [Cloud Foundry 로그](/docs/cli/reference/bluemix_cli?topic=cloud-cli-ibmcloud_cli#ibmcloud_app_logs)
* [Cloud Foundry 엔터프라이즈 환경 - 감사 및 로깅](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [{{site.data.keyword.openwhisk}} 로그 및 모니터링](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#openwhisk_logs)

로그 집계 프로그램에 대해 자세히 보십시오.
* [{{site.data.keyword.cloud_notm}} 로그 분석](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK 스택](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")
