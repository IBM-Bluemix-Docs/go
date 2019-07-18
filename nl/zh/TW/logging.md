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

# 使用 Go 進行記載
{: #logging-golang}

日誌訊息是一些字串，其中包含與建立日誌項目時微服務的狀態及活動相關的環境定義資訊。日誌是診斷服務失敗狀況和原因的必要項目，在監視應用程式性能方面擔任[度量值](/docs/go?topic=go-go-appmetrics)的支援角色。

因為在雲端環境中，處理程序都是暫時的，因此必須收集日誌並傳送至其他地方，通常會傳送到一個集中位置，以進行分析。在雲端環境中進行日誌記載最一致的方法是，將日誌項目傳送至標準輸出及錯誤串流，然後讓基礎架構處理剩下的事宜。

## 將 Logrus 支援新增至 Go 應用程式
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示") 是適用於 Go 的熱門記載架構，並提供許多特有好處，其中包括： 
 * 記載至 `stdout` 或 `stderr`
 * 各種附加選項
 * 可配置的日誌訊息佈置及型樣
 * 使用不同日誌種類的「記載層次」

1. 若要使用 Logrus，請在應用程式的根目錄中執行下列指令，這可安裝套件並將其新增至 `Gopkg.toml` 檔案。
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. 若要在應用程式中使用它，請新增下列程式碼行：
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Stdout 輸出**：
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

如需使用附加器、記載層次和配置詳細資料來自訂日誌訊息的相關資訊，請參閱官方的 [Logrus 文件](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")。

## 後續步驟
{: #go-logging-next notoc}

進一步瞭解在每一個部署目標中檢視日誌：
* [Kubernetes 日誌](https://kubernetes.io/docs/concepts/cluster-administration/logging/#basic-logging-in-kubernetes){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")
* [Cloud Foundry 日誌](/docs/services/CloudLogAnalysis/cfapps?topic=cloudloganalysis-logging_cf_apps)
* [Cloud Foundry Enterprise Environment - 審核及記載](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [{{site.data.keyword.openwhisk}} 日誌及監視](/docs/openwhisk?topic=cloud-functions-logs)

進一步瞭解如何使用日誌聚集器：
* [{{site.data.keyword.cloud_notm}} Log analysis](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [{{site.data.keyword.cloud_notm}} 專用 ELK 堆疊](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")
