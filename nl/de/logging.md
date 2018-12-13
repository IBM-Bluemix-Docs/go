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

# In Go protokollieren
{: #logging_golang}

Protokollnachrichten sind Zeichenfolgen, die Kontextinformationen zum Status und zur Aktivität des Microservice zum Zeitpunkt des Protokolls enthalten. Protokolle sind erforderlich, um zu diagnostizieren, wie und aus welchem Grund Services fehlschlagen, und spielen eine wichtige Rolle für die Überwachung von [Metriken](appmetrics.html) bei der Überwachung des Anwendungsstatus.

Aufgrund der temporären Natur von Prozessen in Cloud-Umgebungen müssen Protokolle erfasst und zur Analyse an einen anderen Ort gesendet werden, normalerweise an eine zentrale Stelle. Die konsistenteste Methode für die Anmeldung in Cloud-Umgebungen besteht darin, Protokolleinträge an Standardausgabe- und Fehlerdatenströme zu senden und der Infrastruktur die übrige Verarbeitung zu überlassen.

## Logrus-Unterstützung zu einer Go-App hinzufügen
{: #add_logrus}

[Logrus](https://github.com/sirupsen/logrus) ist ein vielfach eingesetztes Protokollierungsframework für Go und bietet zahlreiche native Vorteile, unter anderem die Folgenden: 
 * Protokollierung in `stdout` oder `stderr`
 * Unterschiedliche Optionen für das Anhängen
 * Konfigurierbares Layout und Muster für Protokollnachrichten
 * Verwendung von Protokollebenen für unterschiedliche Protokollkategorien

1. Wenn Sie Logrus verwenden wollen, führen Sie den folgenden Befehl im Stammverzeichnis Ihrer Anwendung aus, um das Paket zu installieren und zur Datei `Gopkg.toml` hinzuzufügen.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. Um die Verwendung in Ihrer Anwendung zu ermöglichen, fügen Sie die folgenden Zeilen Code hinzu:
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Stdout-Ausgabe**:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

Weitere Information zum Anpassen der Protokollnachrichten mit Appendern, Protokollebenen und Konfigurationsdetails enthält die offizielle [Logrus-Dokumentation](https://godoc.org/gopkg.in/Sirupsen/logrus.v0).

## Nächste Schritte
{: #next_steps}

Hier erhalten Sie weitere Informationen zum Anzeigen der Protokolle in den einzelnen Bereitstellungsumgebungen:
* [Kubernetes-Protokolle](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
* [Cloud Foundry-Protokolle](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Aktivität in {{site.data.keyword.openwhisk}} protokollieren und überwachen](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Hier erfahren Sie mehr über die Verwendung eines Protokollaggregators:
* [{{site.data.keyword.cloud_notm}} Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [ELK-Stack in {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
