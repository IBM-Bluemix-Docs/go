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

# Distribuzione e integrazione delle applicazioni Go 
{: #deploy_apps}

Puoi distribuire le tue applicazioni Go con una toolchain oppure utilizzando l'interfaccia riga di comando. Una toolchain è una serie di integrazioni dello strumento che aiuta a creare e automatizzare la distribuzione dell'applicazione. Per ulteriori informazioni sulle toolchain e su come funzionano, consulta [Fornitura continua](/docs/services/ContinuousDelivery/index.html). Questo argomento ti aiuta a trovare informazioni utili sulle diverse metodologie di distribuzione per le applicazioni Go come ad esempio la CLI, Kubernetes, i contenitori, la VSI e il cloud privato.

## Distribuzione a un cluster Kubernetes 
{: #deploy_kube}

Puoi imparare come utilizzare il servizio Kubernetes {{site.data.keyword.cloud_notm}} per distribuire un'applicazione Go inserita nel contenitore. Controlla i [passi dell'esercitazione](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_cluster) per ulteriori informazioni sulla configurazione di un cluster Kubernetes in {{site.data.keyword.cloud_notm}}.

Blog correlati che utilizzano la CLI:
* [Deploying to Kubernetes with the IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Deploying to IBM Cloud Private with IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## Distribuzione ai servizi del contenitore con la CLI 
{: #cf-deploy}

Utilizza il comando `ibmcloud dev deploy` per la distribuzione a {{site.data.keyword.cloud_notm}}. 

Per eseguire la distribuzione a IBM Container Services in {{site.data.keyword.cloud_notm}}, utilizza questo comando: 
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Per ulteriori informazioni sui comandi `ibmcloud dev`, consulta [Sviluppo e distribuzione delle tue applicazioni](/docs/cli/idt/index.html)

## Distribuzione a un server virtuale 
{: #vsi-deploy}

Distribuisci le applicazioni {{site.data.keyword.cloud}} App Service nelle istanze del server virtuale per consentire alle attività di sviluppo della tua infrastruttura e della tua piattaforma di collaborare e aumentare la flessibilità e il controllo dell'applicazione

Un'istanza del server virtuale offre trasparenza, prevedibilità e automazione migliori per tutti i tipi di carico di lavoro quando confrontata con altre configurazioni. Combinala con un server bare metal per creare combinazioni di carico di lavoro uniche. Ad esempio, puoi creare la logica di database ad alte prestazioni o il machine learning con configurazioni bare metal e GPU eseguite su un sistema operativo basato su Linux Debian. 

Per ulteriori informazioni, consulta [Distribuzione a un server virtuale](/docs/apps/vsi-deploy.html).



