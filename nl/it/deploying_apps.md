---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry, go deploy virtual

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Distribuzione e integrazione delle applicazioni Go
{: #go-deploy-apps}

Puoi distribuire le tue applicazioni Go con una toolchain oppure utilizzando l'interfaccia riga di comando. Una toolchain è una serie di integrazioni dello strumento che aiuta a creare e automatizzare la distribuzione dell'applicazione. Per ulteriori informazioni sulle toolchain e su come funzionano, consulta [Fornitura continua](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-cd_getting_started#cd_getting_started). Questo argomento ti aiuta a trovare informazioni utili sulle diverse metodologie di distribuzione per le applicazioni Go come ad esempio la CLI, Kubernetes, i contenitori, la VSI e il cloud privato.

## Distribuzione a un cluster Kubernetes
{: #deploy_kube-go}

Puoi imparare come utilizzare il servizio Kubernetes {{site.data.keyword.cloud_notm}} per distribuire un'applicazione Go inserita nel contenitore. Controlla i [passi dell'esercitazione](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) per ulteriori informazioni sulla configurazione di un cluster Kubernetes in {{site.data.keyword.cloud_notm}}.

Blog correlati che utilizzano la CLI:
* [Distribuzione a Kubernetes con la CLI {{site.data.keyword.dev_cli_long}}](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
* [Distribuzione a IBM Cloud privato con la CLI {{site.data.keyword.dev_cli_short}}](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").

## Distribuzione ai servizi del contenitore con la CLI
{: #go-deploy-container}

Utilizza il comando `ibmcloud dev deploy` per la distribuzione a {{site.data.keyword.cloud_notm}}. 

Per eseguire la distribuzione a IBM Container Services in {{site.data.keyword.cloud_notm}}, utilizza questo comando:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Per ulteriori informazioni sui comandi `ibmcloud dev`, consulta [Sviluppo e distribuzione delle tue applicazioni](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Distribuzione a Cloud Foundry
{: #go-deploy-cf}

Questa opzione distribuisce la tua applicazione nativa del cloud senza che tu debba gestire l'infrastruttura sottostante.

Se intendi distribuire la tua applicazione a [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about#about), devi [preparare il tuo account {{site.data.keyword.cloud_notm}}](/docs/cloud-foundry?topic=cloud-foundry-prepare#prepare).

Se il tuo account ha accesso a {{site.data.keyword.cfee_full_notm}}, puoi selezionare un tipo di deployer **[Cloud pubblico](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)** o **[Ambiente aziendale](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)**, che puoi utilizzare per creare e gestire ambienti isolati per ospitare applicazioni Cloud Foundry esclusivamente per la tua azienda.

## Distribuzione a un server virtuale
{: #go-vsi-deploy}

Distribuisci le applicazioni {{site.data.keyword.cloud}} App Service nelle istanze del server virtuale per consentire alle attività di sviluppo della tua infrastruttura e della tua piattaforma di collaborare e aumentare la flessibilità e il controllo dell'applicazione

Un'istanza del server virtuale offre trasparenza, prevedibilità e automazione migliori per tutti i tipi di carico di lavoro quando confrontata con altre configurazioni. Combinala con un server bare metal per creare combinazioni di carico di lavoro uniche. Ad esempio, puoi creare la logica di database ad alte prestazioni o il machine learning con configurazioni bare metal e GPU eseguite su un sistema operativo basato su Linux Debian.

Per ulteriori informazioni, consulta [Distribuzione a un server virtuale](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy).

