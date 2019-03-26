---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Go 中的日志记录
{: #logging_golang}

日志消息是其中包含上下文信息的字符串，这些信息与创建日志条目时微服务的状态和活动相关。需要日志才能诊断服务失败的方式和原因，将[度量值](/docs/go/appmetrics.html)与日志配置使用后，可以更好地监视应用程序运行状况。

考虑到云环境中进程的瞬态性质，必须收集日志并将其发送到其他位置，通常发送到集中位置以供分析。在云环境中进行日志记录最一致的方法是将日志条目发送到标准输出和错误流，从而使基础架构处理其余事务。

## 向 Go 应用程序添加 Logrus 支持
{: #add_logrus}

[Logrus](https://github.com/sirupsen/logrus) 是 Go 的常用日志记录框架，提供了许多本机优点，包括： 
 * 将日志记录到 `stdout` 或 `stderr`
 * 各种附加选项
 * 可配置的日志消息布局和模式
 * 对不同日志类别使用日志级别

1. 要使用 Logrus，请在应用程序的根目录中运行以下命令，此命令将安装软件包并将其添加到 `Gopkg.toml` 文件。
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. 要在应用程序中使用 Log4js，请添加以下代码行：
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Stdout 输出**：
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

有关使用附加器、日志级别和配置详细信息来定制日志消息的更多信息，请参阅官方的 [Logrus 文档](https://godoc.org/gopkg.in/Sirupsen/logrus.v0)。

## 后续步骤
{: #next_steps-logging}

了解有关查看每个部署环境中日志的详细信息：
* [Kubernetes 日志](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
* [Cloud Foundry 日志](/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Cloud Foundry Enterprise Environment - 审计和日志记录](/docs/cloud-foundry/auditing-logging.html#auditing-logging)
* [{{site.data.keyword.openwhisk}} 日志和监视](/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

了解有关使用日志聚集器的更多信息：
* [{{site.data.keyword.cloud_notm}} Log Analysis](/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} Private ELK 堆栈](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)
