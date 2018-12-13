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

# Registrazione in Go
{: #logging_golang}

I messaggi di log sono stringhe che contengono informazioni contestuali sullo stato e sull'attività del microservizio nel momento in cui viene creata la voce di log. I log sono necessari per diagnosticare la modalità e la causa dei malfunzionamenti dei servizi e hanno un ruolo di supporto per le [metriche](appmetrics.html) nel monitoraggio dell'integrità dell'applicazione.

Data la natura transitoria dei processi negli ambienti cloud, i log devono essere raccolti e inviati altrove, di norma a un'ubicazione centralizzata per l'analisi. Il modo più congruente per registrare negli ambienti cloud consiste nell'inviare le voci di log a flussi di output e di errore standard e lasciare che l'infrastruttura gestisca il resto. 

## Aggiunta del supporto Logrus all'applicazione Go
{: #add_logrus}

[Logrus](https://github.com/sirupsen/logrus) è un diffuso framework di registrazione per Go e fornisce molti vantaggi nativi, che includono: 
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

Per ulteriori informazioni sulla personalizzazione dei messaggi di log con gli appender, i livelli di log e i dettagli della configurazione, consulta la [documentazione di Logrus](https://godoc.org/gopkg.in/Sirupsen/logrus.v0) ufficiale.

## Passi successivi 
{: #next_steps}

Ulteriori informazioni sulla visualizzazione dei log in ciascuno dei nostri ambienti di distribuzione: 
* [Log Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
* [Log Cloud Foundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Attività di registrazione e monitoraggio {{site.data.keyword.openwhisk}}](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Ulteriori informazioni sull'utilizzo di un aggregatore di log: 
* [Analisi log {{site.data.keyword.cloud_notm}} ](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [Stack ELK {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
