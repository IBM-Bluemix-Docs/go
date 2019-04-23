---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: fault tolerance go, hystrix go, add fault tolerance, prometheus go, debug go apps

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurazione della tolleranza dell'errore nelle applicazioni Go
{: #fault-tolerance}

La tolleranza dell'errore consente di continuare l'esecuzione di un'applicazione se un componente ha un malfunzionamento o smette di rispondere. Puoi aggiungere la tolleranza dell'errore a un'applicazione Go esistente o abilitare queste funzioni da un'applicazione Go generata. Questa esercitazione di concentra sull'utilizzo del [pacchetto Hystrix](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") per aggiungere il supporto di tolleranza dell'errore a un'applicazione Go.

## Aggiunta della tolleranza dell'errore a un'applicazione Go esistente
{: #add-fault-tolerance}

Nella stessa ubicazione del file `Gopkg.toml` della tua applicazione Go, immetti i seguenti comandi per aggiungere i pacchetti richiesti nel tuo elenco di dipendenze:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

Per utilizzare hystrix, deve essere aggiunta una funzione middleware:
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

La gestione degli errori può essere diversa a seconda di quale tipo di applicazione Go sta venendo utilizzato. Per le applicazioni web, un errore reindirizza a una pagina 500, mentre i microservizi restituiscono un oggetto JSON che specifica l'errore.

Questo middleware richiede una stringa che è un comando configurato. La configurazione di un comando deve essere effettuata nella funzione `main` nel seguente modo:
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Dopo aver configurato un comando Hystrix, il middleware può essere aggiunto al router con la seguente istruzione:
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Esposizione delle metriche Hystrix a Prometheus (facoltativo)
{: #hystrix-optional}

Prima di aggiungere Hystrix a Prometheus, la tua applicazione deve essere configurata con le metriche dell'applicazione. Puoi seguire la procedura nell'argomento [Utilizzo delle metriche dell'applicazione con le applicazioni Go](/docs/go/appmetrics.html) per aggiungere il supporto delle metriche dell'applicazione.

Hystrix fornisce agli utenti la capacità di prendere i dati delle metriche e di esporli in una raccolta di metriche. Per esporre Hystrix a Prometheus, deve essere aggiunto anche il pacchetto metric_collector:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

In aggiunta a `metric_collector`, deve essere aggiunto un ulteriore file, `prometheus_collector.go`, alla tua applicazione Go. È possibile trovare questo file [qui](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go). Questo file deve essere aggiunto al pacchetto `plugins`.

Sono necessarie due ulteriori importazioni:
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

Nella funzione `main`, aggiungi le seguenti istruzioni:
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

Esegui il progetto e passa all'instradamento `/metrics` che visualizza le metriche fornite da Hystrix.
