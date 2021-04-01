---

copyright:
  years: 2018, 2021
lastupdated: "2021-04-01"

keywords: how to log in go, go logging, debug go apps, go troubleshooting, logrus go, go stdout

subcollection: go

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}

# Logging in Go
{: #logging-golang}

Log messages are strings with contextual information in them about the state and activity of the microservice at the time that the log entry is made. Logs are required to diagnose how and why services fail.

Given the transient nature of processes in cloud environments, logs must be collected and sent elsewhere, usually to a centralized location for analysis. The most consistent way to log in cloud environments is to send log entries to standard output and error streams, which leaves the infrastructure to handle the rest.

## Adding Logrus support to Go app
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: external} is a popular logging framework for Go and provides many native benefits, which include: 
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

For more information about customizing the log messages with appenders, log levels, and configuration details, see the official [Logrus documentation](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: external}.

## Next steps
{: #go-logging-next notoc}

Learn more about viewing logs in the following deployment environments:
* [Managing Kubernetes cluster logs with {{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-kube)
* [Viewing logs in Cloud Foundry](/docs/log-analysis?topic=log-analysis-monitor_cfapp_logs)
* [Viewing logs in {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=openwhisk-logs)
