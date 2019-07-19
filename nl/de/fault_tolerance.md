---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: fault tolerance go, hystrix go, add fault tolerance, prometheus go, debug go apps

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Fehlertoleranz in Go-Apps festlegen
{: #fault-tolerance}

Eine Fehlertoleranz ermöglicht einer Anwendung die Fortsetzung ihrer Ausführung, wenn eine Komponente ausfällt oder nicht mehr reagiert. Sie können Fehlertoleranz zu einer vorhandenen Go-App hinzufügen oder diese Features von einer generierten Go-App aus aktivieren. Das vorliegende Lernprogramm konzentriert sich auf die Verwendung des [Hystrix-Pakets](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link"), um einer Go-App Unterstützung für Fehlertoleranz hinzuzufügen. 

## Fehlertoleranz zu einer vorhandenen Go-App hinzufügen
{: #add-fault-tolerance}

Fügen Sie die erforderlichen Pakete in Ihre Abhängigkeitsliste ein. Geben Sie dazu die folgenden Befehle an derselben Position ein, an der sich die Datei `Gopkg.toml` Ihrer Go-App befindet: 
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

Damit Hystrix verwendet werden kann, muss eine Middlewarefunktion wie folgt hinzugefügt werden:
```go
func HystrixHandler(command string) gin.HandlerFunc {
  return func(c *gin.Context) {
    hystrix.Do(command, func() error {
      c.Next()
      return nil
    }, func(err error) error {
      //Handle failures
      return err
    })
  }
}
``` 
{: codeblock}

Die Behandlung von Fehlern kann je nach Art der verwendeten Go-App variieren. Bei Web-Apps erfolgt bei einem Fehler die Weiterleitung zu einer Seite für einen Fehler 500, während Microservices ein JSON-Objekt zurückgeben, das den Fehler angibt.

Diese Middleware verwendet eine Zeichenfolge, bei der es sich um einen konfigurierten Befehl handelt. Das Konfigurieren eines Befehls muss in der Funktion `main` wie folgt vorgenommen werden:
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Nachdem ein Hystrix-Befehl konfiguriert worden ist, kann die Middleware mit der folgenden Anweisung zum Router hinzugefügt werden:
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Hystrix-Metriken gegenüber Prometheus zugänglich machen (optional)
{: #hystrix-optional}

Bevor Sie Hystrix zu Prometheus hinzufügen können, muss Ihre App mit App-Metriken konfiguriert werden. Sie können die im Thema [Anwendungsmetriken mit Go-Apps verwenden](/docs/go?topic=go-go-appmetrics) beschriebenen Schritte ausführen, um Unterstützung für App-Metriken hinzuzufügen. 

Hystrix bietet Benutzern die Möglichkeit, die erfassten Metrikdaten einem Collector für Metriken zugänglich zu machen. Um Hystrix für Prometheus zugänglich zu machen, muss außerdem das Paket 'metric_collector' hinzugefügt werden: 
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

Zusätzlich zu `metric_collector` muss die Datei `prometheus_collector.go` zu Ihrer Go-App hinzugefügt werden. Die Datei können Sie [hier](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go) finden. Fügen Sie diese Datei dem Paket `plugins` hinzu. 

Zwei Importe sind erforderlich: 
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

Fügen Sie in der Funktion `main` die folgenden Anweisungen hinzu: 
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<name_der_anwendung>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

Um anzuzeigen, welche Metriken Hystrix zu Verfügung stellt, führen Sie das Projekt aus und öffnen Sie die Route `/metrics`.
