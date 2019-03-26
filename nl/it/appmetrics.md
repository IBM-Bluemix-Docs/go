---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilizzo delle metriche dell'applicazione con le applicazioni Go
{: #appmetrics}

Le metriche dell'applicazione sono importanti per il monitoraggio delle prestazioni della tua applicazione. Avere una visualizzazione in diretta delle metriche come CPU, Memoria, Latenza e HTTP è essenziale per garantire che la tua applicazione sia in esecuzione in modo efficace nel tempo. Puoi utilizzare un servizio cloud come il [ridimensionamento automatico](/docs/services/Auto-Scaling/index.html) di Cloud Foundry che si basa sulle metriche per modificare dinamicamente le dimensioni delle istanze in modo che corrispondano all'attuale carico di lavoro. Utilizzando le metriche dell'applicazione, sei informato precisamente quando aumentare o ridurre le istanze oppure quando eliminare quelle che non sono più necessarie per mantenere bassi i costi.

Le metriche dell'applicazione vengono acquisite come dati delle serie temporali. L'aggregazione e la visualizzazione delle metriche acquisite possono essere di ausilio nell'identificazione di problemi delle prestazioni comuni quali:

* Tempi di risposta HTTP lenti su qualche instradamento o su tutti gli instradamenti
* Una scarsa velocità effettiva nell'applicazione
* Dei picchi di richiesta che causano un rallentamento
* Un utilizzo della CPU più elevato del previsto per il livello di velocità effettiva/carico
* Un utilizzo della memoria elevato o crescente (potenziale perdita di memoria)

## Aggiunta di metriche dell'applicazione alla tua applicazione Go esistente
{: #add-appmetrics-existing}

Per aggiungere il monitoraggio delle prestazioni alla tua applicazione Go, puoi utilizzare l'aggregazione completa delle metriche fornita dal pacchetto `promhttp`.

Il pacchetto `promhttp` ha molti punti di estensione, compresa la [configurazione Prometheus](https://github.com/prometheus/client_golang).

1. Ad esempio, utilizza il seguente “Hello World” Go di esempio + l'applicazione Gin:
    ```go
    // imports above
    func main() {
        router := gin.Default()
      router.GET("/", func(c *gin.Context) {
            c.String(http.StatusOK, "Hello, World!")
      }
        router.Run(":3000")
  }
    ```
    {: codeblock}

2. Ottieni il pacchetto con il seguente comando:
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. Aggiungi `promhttp` alle importazioni:
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  Deve inoltre essere aggiunto un ulteriore instradamento delle metriche al router.

  Il codice revisionato è ora simile al seguente esempio:
  ```go
  // imports above
  func main() {
      router := gin.Default()
      router.GET("/", func(c *gin.Context) {
          c.String(http.StatusOK, "Hello, World!")
      }
      router.GET("/metrics", gin.WrapH(promhttp.Handler()))
      router.Run(":3000")
  }
  ```
  {: codeblock}

## Utilizzo delle metriche dell'applicazione nei kit starter
{: #starterkits-appmetrics}

Le applicazioni Go lato server create dai kit starter vengono fornite automaticamente con l'[endpoint Prometheus](https://prometheus.io/) in `http://<hostname>:<port>/metrics`. Il codice per questo endpoint si trova in `server.go`.

Per ulteriori informazioni, consulta il [Repository GitHub per Prometheus](https://github.com/prometheus/client_golang/).
