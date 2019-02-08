---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-04"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Logging in Go
{: #logging_golang}

Log messages are strings with contextual information in them about the state and activity of the microservice at the time that the log entry is made. Logs are required to diagnose how and why services fail, and plays a supporting role to [metrics](appmetrics.html) in monitoring application health.

Given the transient nature of processes in Cloud environments, logs must be collected and sent elsewhere, usually to a centralized location for analysis. The most consistent way to log in cloud environments is to send log entries to standard output and error streams, which leaves the infrastructure to handle the rest.

## Adding Logrus support to Go app
{: #add_logrus}

[Logrus](https://github.com/sirupsen/logrus) is a popular logging framework for Go and provides many native benefits, which include: 
 * Logging to `stdout` or `stderr`
 * Various appending options
 * Configurable log message layout and patterns
 * Using Log Levels for different log categories

1. To use Logrus, run the following command in the root directory of your application, which installs the package and adds it to your `Gopkg.toml` file.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. To use it in your application, add the following lines of code:
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Stdout Output**:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

For more information about customizing the log messages with appenders, log levels, and configuration details, see the official [Logrus documentation](https://godoc.org/gopkg.in/Sirupsen/logrus.v0).

## Next Steps
{: #next_steps}

Learn more about viewing the logs in each of our deployment environments:
* [Kubernetes logs](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
* [Cloud Foundry logs](https://cloud.ibm.com/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Cloud Foundry Enterprise Environment - Auditing and logging](docs/cloud-foundry/auditing-logging.html#auditing-logging)
* [{{site.data.keyword.openwhisk}} Logs and monitoring](https://cloud.ibm.com/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Learn more about using a log aggregator:
* [{{site.data.keyword.cloud_notm}} Log analysis](https://cloud.ibm.com/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK stack](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
