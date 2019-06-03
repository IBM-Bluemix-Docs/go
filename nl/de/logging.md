---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-30"

keywords: how to log in go, go logging, debug go apps, go troubleshooting, logrus go, go stdout

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# In Go protokollieren
{: #logging-golang}

Protokollnachrichten sind Zeichenfolgen, die Kontextinformationen zum Status und zur Aktivität des Microservice zum Zeitpunkt des Protokolls enthalten. Protokolle sind erforderlich, um zu diagnostizieren, wie und aus welchem Grund Services fehlschlagen, und spielen eine wichtige Rolle für die Überwachung von [Metriken](/docs/go?topic=go-go-appmetrics) bei der Überwachung des Anwendungsstatus.

Angesichts der transienten Natur von Prozessen in Cloudumgebungen müssen Protokolle erfasst und an eine andere Stelle gesendet werden, in der Regel an eine zentrale Position für die Analyse. Die konsistenteste Möglichkeit, sich in Cloudumgebungen anzumelden, besteht darin, Protokolleinträge an die Standardausgabe- und Fehlerdatenströme zu senden; die Infrastruktur ist dann für die restliche Verarbeitung zuständig.

## Logrus-Unterstützung zu einer Go-App hinzufügen
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") ist ein vielfach eingesetztes Protokollierungsframework für Go und bietet zahlreiche native Vorteile, unter anderem die Folgenden: 
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

Weitere Information zum Anpassen der Protokollnachrichten mit Appendern, Protokollebenen und Konfigurationsdetails enthält die offizielle [Logrus-Dokumentation](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link").

## Nächste Schritte
{: #go-logging-next notoc}

Erfahren Sie mehr zum Anzeigen der Protokolle in jedem Ihrer Bereitstellungsziele:
* [Kubernetes-Protokolle](https://kubernetes.io/docs/concepts/cluster-administration/logging/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
* [Cloud Foundry-Protokolle](/docs/cli/reference/bluemix_cli?topic=cloud-cli-ibmcloud_cli#ibmcloud_app_logs)
* [Cloud Foundry Enterprise Environment - Audit und Protokollierung](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [{{site.data.keyword.openwhisk}} - Protokolle und Überwachung](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#openwhisk_logs)

Erfahren Sie mehr zur Verwendung eines Protokollaggregators:
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [Privater ELK-Stack von {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")
