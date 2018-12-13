---

copyright:
  years: 2018
lastupdated: "2018-10-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go-Apps bereitstellen und integrieren
{: #deploy_apps}

Sie können Ihre Go-Apps mit einer Toolchain oder über die Befehlszeilenschnittstelle bereitstellen. Eine Toolchain ist eine Gruppe von Toolintegrationen, die bei der Erstellung und Automatisierung der Anwendungsbereitstellung helfen. Weitere Informationen zu Toolchains und ihrer Funktionsweise finden Sie in [Continuous Delivery](/docs/services/ContinuousDelivery/index.html). Dieser Abschnitt hilft Ihnen, nützliche Informationen zu unterschiedlichen Bereitstellungsmethoden für Go-Anwendungen wie CLI, Kubernetes, Container, VSI und Private Cloud zu finden.

## In Kubernetes-Cluster bereitstellen
{: #deploy_kube}

Sie können erfahren, wie Sie den Kubernetes-Service von {{site.data.keyword.cloud_notm}} zur Bereitstellung einer containerisierten Go-App verwenden. Weitere Informationen zum Einrichten eines Kubernetes-Clusters in {{site.data.keyword.cloud_notm}} finden Sie in den entsprechenden [Schritten des Lernprogramms](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_cluster).

Zugehörige Blogs, die die Befehlszeilenschnittstelle (CLI) verwenden:
* [Deploying to Kubernetes with the IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Deploying to IBM Cloud Private with IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## In Container-Services mit der CLI bereitstellen
{: #cf-deploy}

Verwenden Sie den Befehl `ibmcloud dev deploy` für die Bereitstellung in {{site.data.keyword.cloud_notm}}. 

Verwenden Sie für die Bereitstellung in IBM Container Services in {{site.data.keyword.cloud_notm}} den folgenden Befehl:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Weitere Informationen zu `ibmcloud dev`-Befehlen finden Sie in [Apps entwickeln und bereitstellen](/docs/cli/idt/index.html). 

## Auf virtuellem Server implementieren
{: #vsi-deploy}

Stellen Sie Apps für {{site.data.keyword.cloud}} App Service auf virtuellen Serverinstanzen bereit, um ein Zusammenspiel der Plattform- und Infrastrukturentwicklungsaktivitäten zu ermöglichen und die Steuerungsmöglichkeiten und Flexibilität bei Apps zu steigern.

Eine virtuelle Serverinstanz bietet im Vergleich zu anderen Konfigurationen mehr Transparenz, Vorhersagbarkeit und Automatisierungsmöglichkeiten für alle Workloadtypen. Kombinieren Sie die virtuelle Instanz mit einer Bare-Metal-Server-Instanz, um eindeutige Workloadkombinationen zu bilden. Sie können z. B. eine leistungsfähige Datenbanklogik oder effizientes maschinelles Lernen mit Bare-Metal- und GPU-Konfigurationen (GPU = Graphics Processing Unit, Grafik-Verarbeitungseinheit) erstellen, die unter einem auf Linux basierenden Debian-Betriebssystem ausgeführt werden.

Weitere Informationen finden Sie in [Auf virtuellem Server bereitstellen](/docs/apps/vsi-deploy.html).



