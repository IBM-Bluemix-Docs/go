---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry, go deploy virtual

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Go-Apps bereitstellen und integrieren
{: #go-deploy-apps}

Sie können Ihre Go-Anwendungen mit einer Toolchain oder über die Befehlszeilenschnittstelle bereitstellen. Eine Toolchain ist eine Gruppe von Toolintegrationen, die bei der Erstellung und Automatisierung der App-Bereitstellung helfen. Weitere Informationen zu Toolchains und ihrer Funktionsweise finden Sie in [Continuous Delivery](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started). Hier finden Sie auch nützliche Informationen zu unterschiedlichen Bereitstellungszielen für Go-Apps wie CLI, Kubernetes, Container, VSI und Private Cloud. 

## In Kubernetes-Cluster bereitstellen
{: #deploy_kube-go}

Sie können erfahren, wie Sie den Kubernetes-Service von {{site.data.keyword.cloud_notm}} zur Bereitstellung einer containerisierten Go-App verwenden. Weitere Informationen zum Einrichten eines Kubernetes-Clusters in {{site.data.keyword.cloud_notm}} finden Sie in den entsprechenden [Schritten des Lernprogramms](/docs/containers?topic=containers-cs_cluster_tutorial).

Zugehörige Blogs, die die Befehlszeilenschnittstelle (CLI) verwenden:
* [Bereitstellung in Kubernetes mit der {{site.data.keyword.dev_cli_long}}-CLI](https://www.ibm.com/blogs/cloud-archive/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
* [Bereitstellung in IBM Cloud Private mit der {{site.data.keyword.dev_cli_short}}-CLI](https://www.ibm.com/cloud/blog/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")

## In Container-Services mit der CLI bereitstellen
{: #go-deploy-container}

Verwenden Sie den Befehl `ibmcloud dev deploy` für die Bereitstellung in {{site.data.keyword.cloud_notm}}. 

Verwenden Sie für die Bereitstellung in IBM Container Services in {{site.data.keyword.cloud_notm}} den folgenden Befehl:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Weitere Informationen zu `ibmcloud dev`-Befehlen finden Sie in [Apps entwickeln und bereitstellen](/docs/cli?topic=cloud-cli-getting-started).

## In Cloud Foundry bereitstellen
{: #go-deploy-cf}

Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen.

Wenn Sie die App in [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about) bereitstellen möchten, müssen Sie [Ihr {{site.data.keyword.cloud_notm}}-Konto vorbereiten](/docs/cloud-foundry?topic=cloud-foundry-prepare).

Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Apps für Ihr Unternehmen verwendet werden kann. 

## Auf einem virtuellem Server bereitstellen
{: #go-vsi-deploy}

Stellen Sie Apps für {{site.data.keyword.cloud}} App Service auf virtuellen Serverinstanzen bereit, um ein Zusammenspiel der Plattform- und Infrastrukturentwicklungsaktivitäten zu ermöglichen und die Steuerungsmöglichkeiten und Flexibilität bei Apps zu steigern.

Eine virtuelle Serverinstanz bietet im Vergleich zu anderen Konfigurationen mehr Transparenz, Vorhersagbarkeit und Automatisierungsmöglichkeiten für alle Workloadtypen. Kombinieren Sie die virtuelle Instanz mit einer Bare-Metal-Server-Instanz, um eindeutige Workloadkombinationen zu bilden. Sie können z. B. eine leistungsfähige Datenbanklogik oder effizientes maschinelles Lernen mit Bare-Metal- und GPU-Konfigurationen (GPU = Graphics Processing Unit, Grafik-Verarbeitungseinheit) erstellen, die unter einem auf Linux basierenden Debian-Betriebssystem ausgeführt werden.

  VSI-Bereitstellung ist für einige Starter-Kits verfügbar. Wenn Sie diese Funktion verwenden möchten, rufen Sie das [{{site.data.keyword.cloud_notm}}-Dashboard](https://{DomainName}) auf und klicken Sie auf **App erstellen** in der Kachel **Apps**.
  {: note}

Weitere Informationen finden Sie in [Auf einem virtuellem Server bereitstellen](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server). 

