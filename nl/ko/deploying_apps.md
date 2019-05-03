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

# Go 앱 배치 및 통합
{: #deploy_apps}

도구 체인을 사용하거나 명령행 인터페이스를 사용하여 Go 앱을 배치할 수 있습니다. 도구 체인은 애플리케이션 배치를 빌드하고 자동화하는 데 도움이 되는 도구 통합 세트입니다. 도구 체인 및 작동 방식에 대해 자세히 보려면 [Continuous Delivery](/docs/services/ContinuousDelivery/index.html#cd_getting_started)를 참조하십시오. 이 주제는 CLI, Kubernetes, 컨테이너, VSI 및 프라이빗 클라우드와 같은 Go 애플리케이션의 여러 배치 방법에 대한 유용한 정보를 찾는 데 도움이 됩니다.

## Kubernetes 클러스터에 배치
{: #deploy_kube-go}

{{site.data.keyword.cloud_notm}} Kubernetes 서비스를 사용하여 컨테이너화된 Go 앱을 배치하는 방법을 알아볼 수 있습니다. {{site.data.keyword.cloud_notm}}에서 Kubernetes 클러스터를 설정하는 방법에 대한 자세한 정보는 [튜토리얼 단계](/docs/containers/cs_cluster.html#cs_cluster)를 확인하십시오.

CLI를 사용하는 관련 블로그:
* [IBM Cloud Developer Tools CLI를 사용하여 Kubernetes에 배치](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [IBM Cloud Developer Tools CLI를 사용하여 IBM Cloud Private에 배치](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## CLI를 사용하여 컨테이너 서비스에 배치
{: #go-deploy-container}

`ibmcloud dev deploy` 명령을 사용하여 {{site.data.keyword.cloud_notm}}에 배치하십시오. 

{{site.data.keyword.cloud_notm}}의 IBM Container Services에 배치하려면 다음 명령을 사용하십시오.
```
ibmcloud dev deploy –target container 
```
{: codeblock}

`ibmcloud dev` 명령에 대한 자세한 정보는 [앱 개발 및 배치](/docs/cli/index.html)를 참조하십시오.

## Cloud Foundry에 배치
{: #go-deploy-cf}

이 옵션에서는 기본 인프라를 관리하지 않고도 클라우드 고유 앱을 배치합니다.

[{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html)에 앱을 배치하려면 [{{site.data.keyword.cloud_notm}} 계정을 준비](/docs/cloud-foundry/prepare-account.html)해야 합니다.

계정에 {{site.data.keyword.cfee_full_notm}}에 대한 액세스 권한이 있는 경우, **[퍼블릭 클라우드](/docs/cloud-foundry-public/about-cf.html#about-cf)** 또는 **[엔터프라이즈 환경](/docs/cloud-foundry-public/cfee.html#cfee)**의 배치자 유형을 선택할 수 있으며, 이는 사용자 엔터프라이즈에서 독점적으로 Cloud Foundry 애플리케이션을 호스팅하기 위한 격리 환경을 작성하고 관리하는 데 사용할 수 있습니다.

## 가상 서버에 배치
{: #go-vsi-deploy}

{{site.data.keyword.cloud}} App Service 앱을 가상 서버 인스턴스에 배치하여 플랫폼 및 인프라 개발자 활동이 함께 작동하게 하고 앱 제어 및 유연성을 향상시키십시오.

가상 서버 인스턴스는 다른 구성과 비교할 때 모든 워크로드 유형에 대해 향상된 투명성, 예측 가능성 및 자동화를 제공합니다. 이를 베어 메탈 서버와 결합하여 고유 워크로드 조합을 작성하십시오. 예를 들어, Debian Linux 기반 운영 체제를 실행하는 베어 메탈 및 GPU 구성을 사용하여 고성능 데이터베이스 로직 또는 기계 학습을 작성할 수 있습니다.

자세한 정보는 [가상 서버에 배치](/docs/apps/vsi-deploy.html#vsi-deploy)를 참조하십시오.

