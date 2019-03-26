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

# Go-Apps bereitstellen und integrieren
{: #deploy_apps}

Sie können Ihre Go-Apps mit einer Toolchain oder über die Befehlszeilenschnittstelle bereitstellen. Eine Toolchain ist eine Gruppe von Toolintegrationen, die bei der Erstellung und Automatisierung der Anwendungsbereitstellung helfen. Weitere Informationen zu Toolchains und ihrer Funktionsweise finden Sie in [Continuous Delivery](/docs/services/ContinuousDelivery/index.html#cd_getting_started). Dieser Abschnitt hilft Ihnen, nützliche Informationen zu unterschiedlichen Bereitstellungsmethoden für Go-Anwendungen wie CLI, Kubernetes, Container, VSI und Private Cloud zu finden.

## In Kubernetes-Cluster bereitstellen
{: #deploy_kube-go}

Sie können erfahren, wie Sie den Kubernetes-Service von {{site.data.keyword.cloud_notm}} zur Bereitstellung einer containerisierten Go-App verwenden. Weitere Informationen zum Einrichten eines Kubernetes-Clusters in {{site.data.keyword.cloud_notm}} finden Sie in den entsprechenden [Schritten des Lernprogramms](/docs/containers/cs_cluster.html#cs_cluster).

Zugehörige Blogs, die die Befehlszeilenschnittstelle (CLI) verwenden:
* [Deploying to Kubernetes with the IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Deploying to IBM Cloud Private with IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## In Container-Services mit der CLI bereitstellen
{: #go-deploy-container}

Verwenden Sie den Befehl `ibmcloud dev deploy` für die Bereitstellung in {{site.data.keyword.cloud_notm}}. 

Verwenden Sie für die Bereitstellung in IBM Container Services in {{site.data.keyword.cloud_notm}} den folgenden Befehl:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Weitere Informationen zu `ibmcloud dev`-Befehlen finden Sie in [Apps entwickeln und bereitstellen](/docs/cli/index.html). 

## In Cloud Foundry bereitstellen
{: #go-deploy-cf}

Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen. 

Wenn Sie die App in [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html) bereitstellen möchten, müssen Sie [Ihr {{site.data.keyword.cloud_notm}}-Konto vorbereiten](/docs/cloud-foundry/prepare-account.html). 

Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Anwendungen für Ihr Unternehmen verwendet werden kann. 

## Auf virtuellem Server implementieren
{: #go-vsi-deploy}

Stellen Sie Apps für {{site.data.keyword.cloud}} App Service auf virtuellen Serverinstanzen bereit, um ein Zusammenspiel der Plattform- und Infrastrukturentwicklungsaktivitäten zu ermöglichen und die Steuerungsmöglichkeiten und Flexibilität bei Apps zu steigern.

Eine virtuelle Serverinstanz bietet im Vergleich zu anderen Konfigurationen mehr Transparenz, Vorhersagbarkeit und Automatisierungsmöglichkeiten für alle Workloadtypen. Kombinieren Sie die virtuelle Instanz mit einer Bare-Metal-Server-Instanz, um eindeutige Workloadkombinationen zu bilden. Sie können z. B. eine leistungsfähige Datenbanklogik oder effizientes maschinelles Lernen mit Bare-Metal- und GPU-Konfigurationen (GPU = Graphics Processing Unit, Grafik-Verarbeitungseinheit) erstellen, die unter einem auf Linux basierenden Debian-Betriebssystem ausgeführt werden.

Weitere Informationen finden Sie in [Auf virtuellem Server bereitstellen](/docs/apps/vsi-deploy.html#vsi-deploy).

