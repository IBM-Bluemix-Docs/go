---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: how to log in go, go logging, debug go apps, go troubleshooting, logrus go, go stdout

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Registrazione in Go
{: #logging-golang}

I messaggi di log sono stringhe che contengono informazioni contestuali sullo stato e sull'attività del microservizio nel momento in cui viene creata la voce di log. I log sono necessari per diagnosticare la modalità e la causa dei malfunzionamenti dei servizi e hanno un ruolo di supporto per le [metriche](/docs/go?topic=go-go-appmetrics) nel monitoraggio dell'integrità dell'applicazione.

Data la natura transitoria dei processi negli ambienti cloud, i log devono essere raccolti e inviati altrove, di norma a un'ubicazione centralizzata per l'analisi. Il modo più congruente per registrare negli ambienti cloud consiste nell'inviare le voci di log a flussi di output e di errore standard e lasciare che l'infrastruttura gestisca il resto.

## Aggiunta del supporto Logrus all'applicazione Go
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") è un diffuso framework di registrazione per Go e fornisce molti vantaggi nativi, che includono: 
 * Registrazione in `stdout` o `stderr`
 * Varie opzioni di accodamento
 * Layout e modelli dei messaggi di log configurabili
 * Utilizzo dei livelli di log per diverse categorie di log

1. Per utilizzare Logrus, immetti il seguente comando nella directory root della tua applicazione, che installa il pacchetto e lo aggiunge al tuo file `Gopkg.toml`.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. Per utilizzarlo nella tua applicazione, aggiungi queste righe di codice:
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Output stdout**:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

Per ulteriori informazioni sulla personalizzazione dei messaggi di log con gli appender, i livelli di log e i dettagli della configurazione, consulta la [documentazione di Logrus](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") ufficiale.

## Passi successivi
{: #go-logging-next notoc}

Ulteriori informazioni sulla visualizzazione dei log in ciascuna delle tue destinazioni di distribuzione;
* [Log Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
* [Log Cloud Foundry](/docs/services/CloudLogAnalysis/cfapps?topic=cloudloganalysis-logging_cf_apps)
* [Cloud Foundry Enterprise Environment - Controllo e registrazione](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [Log e monitoraggio {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-logs)

Ulteriori informazioni sull'utilizzo di un aggregatore di log:
* [Analisi log {{site.data.keyword.cloud_notm}} ](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [Stack ELK {{site.data.keyword.cloud_notm}} privato](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")
