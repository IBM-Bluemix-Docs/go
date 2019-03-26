---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 시작하기 튜토리얼
{: #getting-started}

다음 튜토리얼에서는 {{site.data.keyword.cloud_notm}} 제공 도구를 사용하여 Go 앱을 작성하고 배치하는 단계를 안내합니다. 다음 튜토리얼 단계에 표시된 대로 웹 기반 [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) 또는 명령행의 [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli)를 사용할 수 있습니다. 이러한 방법 중 하나를 사용하여 프로덕션에 사용 가능 Go 애플리케이션을 바로 생성할 수 있습니다.

## 1단계. 대시보드에서 Go 앱 작성
{: #create-go-app}

1. App Service의 [스타터 킷](https://cloud.ibm.com/developer/appservice/starter-kits) 페이지에서 `Go`에 작성된 스타터 킷을 선택하십시오. **앱 작성**을 클릭하고 `Go`를 언어로 선택하여 빈 스타터 앱을 작성할 수도 있습니다.

    앱을 작성하려면 {{site.data.keyword.cloud_notm}} 계정에 로그인해야 합니다. 계정이 없는 경우 [무료 계정을 등록](https://cloud.ibm.com/registration)할 수 있습니다.
    {: tip}

3. **앱 작성**을 클릭하십시오.
4. 앱의 **이름을 지정**하거나 제공된 일반 앱 이름을 사용하십시오.
5. **고유 호스트 이름**을 입력하십시오. 호스트 이름은 애플리케이션에 액세스하는 데 사용됩니다(예: `server.mybluemix.net`).
6. **작성**을 클릭하십시오. 앱이 작성되면 도구 체인을 사용하여 배치하거나 [명령행](/docs/cli/index.html#ibmcloud-cli)에서 프로젝트를 계속 빌드하고 배치할 수 있습니다.

## 2단계. 대시보드로 배치
{: #deploy-go}

1. 앱 세부사항 페이지에서 **클라우드에 배치**를 클릭하십시오.
2. 선택한 방법에 대한 지시사항에 따라 배치 방법을 설정하십시오.
  * **[Kubernetes](/docs/apps/deploying/containers.html#containers)에 배치**. 이 옵션에서는 작업자 노드라고 하는 호스트의 클러스터를 작성하여 고가용성 애플리케이션 컨테이너를 배치하고 관리합니다. 클러스터를 작성하거나 기존 클러스터에 배치할 수 있습니다.
  * **Cloud Foundry에 배치**. 이 옵션에서는 기본 인프라를 관리하지 않고도 클라우드 고유 앱을 배치합니다. 계정에 {{site.data.keyword.cfee_full_notm}}에 대한 액세스 권한이 있는 경우, **[퍼블릭 클라우드](/docs/cloud-foundry-public/about-cf.html#about-cf)** 또는 **[엔터프라이즈 환경](/docs/cloud-foundry-public/cfee.html#cfee)**의 배치자 유형을 선택할 수 있으며, 이는 사용자 엔터프라이즈에서 독점적으로 Cloud Foundry 애플리케이션을 호스팅하기 위한 격리 환경을 작성하고 관리하는 데 사용할 수 있습니다.
  * **[Virtual Server](/docs/apps/vsi-deploy.html#vsi-deploy)에 배치**. 이 옵션에서는 가상 서버 인스턴스를 프로비저닝하고 앱이 포함된 이미지를 로드하며 DevOps 도구 체인을 작성하고 첫 번째 배치 주기를 시작합니다.

3. 옵션을 선택한 후 ** 작성 **을 클릭하십시오. 도구 체인이 작성되며 앱을 자동으로 배치합니다.

## 3단계. 리소스 추가(선택사항)
{: #add-resource-go}

보안 또는 스토리지와 같은 서비스를 대시보드 내 Go 앱에 신속하게 추가할 수 있습니다.

1. [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard)의 Go 앱으로 돌아가십시오.
2. **리소스 추가**를 클릭하고 추가할 서비스의 카테고리를 선택하고 **다음**을 클릭한 다음 서비스를 선택하십시오. {{site.data.keyword.cloud_notm}} App Service는 선택한 플랜을 기반으로 서비스를 작성합니다. 이전에 사용할 서비스를 작성한 경우 **기존** 카테고리를 선택하십시오.
3. 서비스가 작성되면 **코드 다운로드**를 클릭하여 서비스에 연결된 SDK로 프로젝트를 재생성하십시오.

## 4단계. 로컬로 Go 앱 테스트 및 액세스
{: #run_local-go}

`ibmcloud dev` 명령을 사용하여 [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli)에서 로컬로 Go 앱을 사용한 작업을 수행할 수 있습니다.

1. `ibmcloud dev build` 명령을 사용하여 애플리케이션을 빌드하십시오.
2. `ibmcloud dev run` 명령을 사용하여 애플리케이션을 로컬로 실행하십시오. 애플리케이션은 로털 시스템의 Docker 컨테이너에서 실행됩니다. `http://localhost:8080`에 액세스하여 브라우저에서 애플리케이션을 테스트할 수 있습니다.
3. 상태 검사 엔드포인트는 `http://localhost:8080/health`에서 사용 가능합니다.
4. `http://localhost:3000/metrics`에서 메트릭에 액세스할 수 있습니다.

{{site.data.keyword.dev_cli_long}}에 대해 자세히 보려면 전체 [CLI 문서](/docs/cli/index.html#ibmcloud-cli)를 참조하십시오.

## 대체 배치 방법 탐색
{: #deploy_cloud-go notoc}

[여기](/docs/go/deploying_apps.html)서 배치 탭 아래에 애플리케이션을 배치하는 방법을 알아보십시오. 

Go 애플리케이션을 펼치려면 Go 프로그래밍 안내서의 주제를 계속 확인하십시오.
