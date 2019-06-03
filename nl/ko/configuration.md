---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-19"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go 환경 구성
{: #configure-go-env}

표준화된 가이드라인은 일관된 이식성을 유지하는 데 도움이 되는 Go 애플리케이션을 개발하는 데 사용할 수 있습니다. 고려할 사항에는 인증 정보 관리, 데이터 저장 및 클라우드에 컨텐츠 공개가 포함됩니다. 클라우드 네이티브 사례에 따라 Go 애플리케이션은 하나의 환경에서 다른 환경으로 쉽게 이동할 수 있습니다. 예를 들어, 코드를 변경하거나 테스트되지 않은 코드 경로를 사용하지 않고 테스트에서 프로덕션 환경으로 이동하는 것입니다.

기존 Go 애플리케이션에 클라우드 지원을 추가하는지 아니면 스타터 킷으로 Go 앱을 작성하는지에 관계없이, 목적은 개발 플랫폼에 사용할 이식성을 제공하는 것입니다.

## 기존 Go 애플리케이션에 클라우드 지원 추가
{: #go-add-cloud-support}

[`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 모듈이 CloudFoundry 및 Kubernetes와 같은 다양한 클라우드 제공자로부터 환경 변수를 집계하므로 애플리케이션은 환경에 의존하지 않습니다.

### `ibm-cloud-env-golang` 모듈 설치
{: #go-install-env-module}

1. 다음 명령을 사용하여 `ibm-cloud-env-golang` 모듈을 설치하십시오.
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. `mappings.json` 파일을 참조하여 코드에서 모듈을 초기화하십시오.
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Go가 기본 매개변수를 지원하지 않으므로 기본 `mappings.json` 파일이 제공되지 않습니다. 맵핑 파일 경로가 `IBMCloudEnv.Initialize()`에 지정되지 않은 경우 오류가 로그됩니다. 
  {: tip}

  `mappings.json` 파일 예:
  ```javascript
  {
      "service1-credentials": {
          "searchPatterns": [
              "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name", 
              "env:my-service1-credentials", 
              "file:/localdev/my-service1-credentials.json" 
          ]
      },
      "service2-username": {
         "searchPatterns":[
              "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### 서비스 인증 정보 검색
{: #go-get-creds}

다음 명령을 사용하여 애플리케이션에서 값을 검색하십시오.

1. `service1credentials` 변수를 검색하십시오.
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. `service2username` 변수를 검색하십시오.
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

이제 여러 클라우드 컴퓨팅 제공자에서 소개된 차이점을 요약하여 런타임 환경에서 애플리케이션을 구현할 수 있습니다.

### 태그 및 레이블에 대한 값 필터링
{: #go-filter-creds}

다음 예에 표시된 대로 서비스 태그와 서비스 레이블을 기반으로 모듈에서 생성된 인증 정보를 필터링할 수 있습니다.
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## 스타터 킷 앱에서 Go 구성 관리자 사용
{: #go-config-manager}

[스타터 킷](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window}![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")으로 작성된 Go 앱은 많은 클라우드 배치 대상(Cloud Foundry, Kubernetes, VSI 및 Functions)에서 실행하는 데 필요한 인증 정보 및 구성과 함께 자동으로 제공됩니다.

### 서비스 인증 정보 이해
{: #go-credentials-config}

서비스에 대한 애플리케이션 구성 정보는 `/server/config` 디렉토리의 `localdev-config.json` 파일에 저장됩니다. Git에 민감한 정보가 저장되지 않도록 파일이 `.gitignore` 디렉토리에 있습니다. 로컬로 실행되는 구성된 서비스에 대한 연결 정보(예: 사용자 이름, 비밀번호 및 호스트 이름)가 이 파일에 저장됩니다.

애플리케이션이 구성 관리자를 사용하여 환경 및 이 파일의 연결 및 구성 정보를 읽습니다. `server/config` 디렉토리에 있는 사용자 정의 빌드 `mappings.json`을 사용하여 각 서비스에 대해 인증 정보를 찾을 수 있는 위치를 전달합니다.

애플리케이션이 로컬로 실행되는 경우 `mappings.json` 파일에서 읽어온 바인드되지 않은 인증 정보를 사용하여 {{site.data.keyword.cloud_notm}} 서비스에 연결될 수 있습니다. 
하지만 바인드되지 않은 인증 정보를 작성해야 하는 경우 {{site.data.keyword.cloud_notm}} 웹 콘솔(예제)에서 또는 [CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") `cf create-service-key` 명령을 사용하여 이를 수행할 수 있습니다.

애플리케이션을 {{site.data.keyword.cloud_notm}}에 푸시하면 이러한 값이 더 이상 사용되지 않습니다. 대신 애플리케이션이 환경 변수를 사용하여 서비스를 바인드하도록 자동으로 연결됩니다. 

* **Cloud Foundry**: 서비스 인증 정보는 `VCAP_SERVICES` 환경 변수에서 가져옵니다.

* **Kubernetes**: 서비스 인증 정보는 서비스마다 개별 환경 변수에서 가져옵니다.

* **{{site.data.keyword.cloud_notm}} 컨테이너 서비스**: 서비스 인증 정보는 가상 서버 인스턴스 또는 {{site.data.keyword.openwhisk}}(Openwhisk)에서 가져옵니다.

## 다음 단계
{: #go-next-steps-config notoc}

`ibm-cloud-env-golang`은 네 개의 검색 패턴 유형인 `user-provided`, `cloudfoundry`, `env` 및 `file`을 사용하여 값 검색을 지원합니다. 지원되는 다른 검색 패턴 및 검색 패턴 예를 확인하려면 [사용법](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 섹션을 참조하십시오.
