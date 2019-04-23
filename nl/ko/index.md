---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-19"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 시작하기 튜토리얼
{: #getting-started}

다음 튜토리얼에서는 {{site.data.keyword.cloud_notm}} 제공 도구를 사용하여 Go 앱을 작성하고 배치하는 단계를 안내합니다. 다음 튜토리얼 단계에 표시된 대로 웹 기반 [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 또는 명령행의 [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)를 사용할 수 있습니다. 이러한 방법 중 하나를 사용하여 프로덕션에 사용 가능 Go 애플리케이션을 바로 생성할 수 있습니다.

## 1단계. 대시보드에서 사용자 정의 Go 앱 작성
{: #create-go-app}

1. 앱을 작성하려면 {{site.data.keyword.cloud_notm}} 계정에 로그인하십시오. 계정이 없는 경우 [무료 계정을 등록](https://{DomainName}/registration){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")할 수 있습니다.
2. {{site.data.keyword.dev_console}}의 [스타터 킷](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 페이지에서 다음 옵션 중 하나를 수행하십시오.
 * `Go`에서 작성된 스타터 킷을 선택한 다음 **스타터 킷 세부사항** 페이지에서 **앱 작성**을 클릭하십시오.
 * 공백 스타터 앱을 선택하고 **앱 작성**을 클릭하십시오.
3. 앱의 이름을 제공하거나 제공된 일반 앱 이름을 사용하십시오.
4. **Go**가 플랫폼으로 선택되었는지 확인한 다음 **작성**을 클릭하십시오. 앱이 작성되면 도구 체인을 사용하여 서비스를 추가하고 배치하거나 [명령행](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)에서 프로젝트를 계속 빌드하고 배치할 수 있습니다.

## 2단계. {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}} 추가
{: #add-resource-go}

이제 Go 애플리케이션에 {{site.data.keyword.watson}} 서비스를 추가할 수 있습니다. 이 튜토리얼의 경우 {{site.data.keyword.texttospeechshort}} 서비스를 Go 앱에 추가하십시오. 이렇게 하면 클라우드 API를 통해 음성 입력을 가져와 텍스트로 변환할 수 있습니다.

1. **앱 세부사항** 페이지에서 **서비스 추가**를 클릭하십시오.
2. **AI**를 선택하고 **다음**을 클릭하십시오.
3. **{{site.data.keyword.texttospeechshort}}**를 선택하고 **다음**을 클릭하십시오.
4. **작성**을 클릭하십시오.

이 서비스가 애플리케이션에 추가되면 [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘") 종속 항목이 `Gopkg.toml`에 추가되고 간단한 인스트루먼테이션 코드가 `services/` 디렉토리에 있는 서비스에서 사용 가능합니다. 또한 각 환경의 서비스 인증 정보 액세스를 위한 구성 정보가 포함됩니다.

코드를 다운로드하려면 **앱 세부사항** 페이지에서 **코드 다운로드**를 클릭하십시오. 다운로드된 코드는 몇 가지 기본 초기화 코드 및 포함된 [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")와 함께 제공됩니다.

## 3단계. 콘솔에서 앱 배치
{: #deploy-go}

1. **앱 세부사항** 페이지에서 **지속적 딜리버리 구성**을 클릭하십시오.
2. 선택한 방법에 대한 지시사항에 따라 배치 대상을 설정하십시오.
  * **[IBM Kubernetes Service](/docs/apps/deploying?topic=creating-apps-containers-kube)에 배치**. 이 옵션에서는 작업자 노드라고 하는 호스트의 클러스터를 작성하여 고가용성 애플리케이션 컨테이너를 배치하고 관리합니다. 클러스터를 작성하거나 기존 클러스터에 배치할 수 있습니다.
  * **Cloud Foundry에 배치**. 이 옵션에서는 기본 인프라를 관리하지 않고도 클라우드 고유 앱을 배치합니다. 계정에 {{site.data.keyword.cfee_full_notm}}에 대한 액세스 권한이 있는 경우, **[퍼블릭 클라우드](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** 또는 **[엔터프라이즈 환경](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**의 배치자 유형을 선택할 수 있으며, 이는 사용자 엔터프라이즈에서 독점적으로 Cloud Foundry 애플리케이션을 호스팅하기 위한 격리 환경을 작성하고 관리하는 데 사용할 수 있습니다.
  * **[Virtual Server](/docs/apps?topic=creating-apps-vsi-deploy)에 배치**. 이 옵션에서는 가상 서버 인스턴스를 프로비저닝하고 앱이 포함된 이미지를 로드하며 DevOps 도구 체인을 작성하고 첫 번째 배치 주기를 시작합니다.

3. 배치 대상을 설정한 후 **다음**을 클릭하십시오.
4. 구성 옵션을 선택한 후 **작성**을 클릭하십시오. 도구 체인이 작성되며 앱이 자동으로 배치됩니다.

## 4단계. 로컬로 Go 앱 테스트 및 액세스
{: #run_local-go}

**앱 세부사항**에서 다음을 수행할 수 있습니다.
* **저장소 보기**를 클릭하여 저장소 보기.
* **도구 체인 보기**를 클릭하여 도구 체인 보기 및 수정.

`ibmcloud dev` 명령을 사용하여 {{site.data.keyword.dev_cli_long}}에서 로컬로 Go 앱을 사용한 작업을 수행할 수 있습니다. 이러한 도구를 사용하면 빨리 로컬에서 반복하고 변경사항을 클라우드에 푸시할 수 있습니다.

1. `ibmcloud dev build` 명령을 사용하여 애플리케이션을 빌드하십시오.
2. `ibmcloud dev run` 명령을 사용하여 애플리케이션을 로컬로 실행하십시오. 애플리케이션은 로털 시스템의 Docker 컨테이너에서 실행됩니다. `http://localhost:8080`에 액세스하여 브라우저에서 애플리케이션을 테스트할 수 있습니다.
3. 상태 검사 엔드포인트는 `http://localhost:8080/health`에서 사용 가능합니다.
4. `http://localhost:3000/metrics`에서 메트릭에 액세스할 수 있습니다.

{{site.data.keyword.dev_cli_notm}}에 대해 자세히 보려면 전체 [CLI 문서](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)를 참조하십시오.

이제 {{site.data.keyword.texttospeechshort}} 서비스를 사용하여 애플리케이션을 빌드할 수 있습니다.

## 대체 배치 방법 탐색
{: #alt-deploy-go notoc}

[배치 탭](/docs/go?topic=go-go-deploy-apps)에서 애플리케이션을 배치하는 방법을 알아보십시오.

Go 애플리케이션을 펼치려면 Go 프로그래밍 안내서의 주제를 계속 확인하십시오.
