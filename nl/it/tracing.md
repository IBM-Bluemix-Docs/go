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

# Configurazione della traccia nelle applicazioni Go
{: #e2e-tracing}

La seguente esercitazione si concentra sui pacchetti Opentracing e Jaeger per la traccia delle applicazioni Go. Per ulteriori informazioni sull'utilizzo di Jaeger, consulta il [portale della documentazione di Jaeger](https://www.jaegertracing.io/docs/).

Nella seguente procedura, vengono utilizzate due piccole applicazioni (una di frontend e una di backend) per la traccia tra due endpoint utilizzando il modulo Jaeger. Puoi iniziare da zero oppure applicare i principi qui descritti alle tue applicazioni Go esistenti. 

## Passo 1. Installazione e abilitazione dei pacchetti Opentracing e Jaeger
{: #install-packages}

1. Nella stessa ubicazione del file `Gopkg.toml` della tua applicazione Go, immetti i seguenti comandi per aggiungere i pacchetti richiesti nel tuo elenco di dipendenze:
  ```go
  dep ensure -add "github.com/opentracing/opentracing-go"
  dep ensure -add "github.com/uber/jaeger-client-go"
  dep ensure -add "github.com/uber/jaeger-lib/metrics/prometheus"
  ```
  {: codeblock}

2. Aggiungi le seguenti righe al tuo codice del server Go:
  ```go
  import(
  "github.com/opentracing/opentracing-go"
	"github.com/opentracing/opentracing-go/ext"
	"github.com/uber/jaeger-client-go"
	jaegerprom "github.com/uber/jaeger-lib/metrics/prometheus"
  )
  ```
  {: codeblock}

### Aggiunta della traccia alla tua applicazione server 
Sono necessarie alcune istruzioni per aggiungere la traccia alla tua applicazione server. Prima di tutto, devi creare un programma di traccia.

Per creare un programma di traccia, fornisci quanto segue:
 * Programma di trasporto
 * Programma di report 
 * Programma di esportazione delle metriche facoltativo 
 
In questa esercitazione, Jaeger esporta le metriche in stile Prometheus. Un oggetto delle metriche consente a Jaeger di segnalare le metriche a Prometheus.

1. Per creare un oggetto delle metriche, aggiungi le seguenti istruzioni alla funzione principale:
  ```go
  factory := jaegerprom.New()
  metrics := jaeger.NewMetrics(factory, map[string]string{"lib": "jaeger"})
  ```
  {: codeblock}

2. Fornisci la seguente istruzione per un programma di trasporto, che comunica a Jaeger dove inviare le sue tracce. Per lo sviluppo in locale, la destinazione è `localhost` alla porta `5775`. Mentre il nome host può essere diverso, nella maggior parte dei casi la porta è `5775`.
  ```go
  transport, err := jaeger.NewUDPTransport("<hostname>:<port>", 0)
  if err != nil {
	log.Errorln(err.Error())
  }
  ```
  {: codeblock}

3. Un programma di report comunica a Jaeger come segnalare i propri dati di traccia. Vengono utilizzati tre tipi di programmi di report:
  * Registrazione - stampa i dati di span come un log
  * Remoto - invia i dati di span a un agent Jaeger
  * Composito - combina più di un programma di report

  Poiché i kit starter Go utilizzano Logrus per registrare i dati, la notifica delle estensioni di Jaeger non è possibile per impostazione predefinita. Tuttavia, Jaeger supporta un'interfaccia di registrazione che può essere utilizzata aggiungendo un adattatore di registrazione con la seguente istruzione:
  ```go
  type LogrusAdapter struct{}

  func (l LogrusAdapter) Error(msg string) {
	log.Errorf(msg)
  }

  func (l LogrusAdapter) Infof(msg string, args ...interface{}) {
	log.Infof(msg, args)
  }
  ```
  {: codeblock}

4. Per creare un programma di report che estende sia i log che i report, aggiungi le seguenti istruzioni:
  ```go
  logAdapt := LogrusAdapter{}
  reporter := jaeger.NewCompositeReporter(
	jaeger.NewLoggingReporter(logAdapt),
	jaeger.NewRemoteReporter(transport,
		jaeger.ReporterOptions.Metrics(metrics),
		jaeger.ReporterOptions.Logger(logAdapt),
	),
  )
  defer reporter.Close()
  ```
  {: codeblock}

5. Un oggetto di campionamento determina in quali situazioni o quanto spesso vengono segnalate le estensioni. Per scopi di sviluppo, un'applicazione deve segnalare tutte le estensioni che riceve. Tuttavia, per la produzione, la segnalazione di tutte le estensioni potrebbe non essere fattibile. Per segnalare tutte le estensioni, puoi utilizzare l'oggetto ConstSampler:
  ```go
  sampler := jaeger.NewConstSampler(true)
  ```
  {: codeblock}

6. Ora che hai creato il programma di report, il campionatore e gli oggetti delle metriche, puoi creare un programma di traccia.
  ```go
  tracer, closer := jaeger.NewTracer("<application name>",
	sampler,
	reporter,
	jaeger.TracerOptions.Metrics(metrics),
  )
  defer closer.Close()

  opentracing.SetGlobalTracer(tracer)
  ```
  {: codeblock}

7. Aggiungi una funzione middleware che estrae i dati di span da una richiesta del client o crea un nuovo oggetto di span principale:
  ```go
  func OpenTracing() gin.HandlerFunc {
	return func(c *gin.Context) {
		wireCtx, _ := opentracing.GlobalTracer().Extract(
			opentracing.HTTPHeaders,
			opentracing.HTTPHeadersCarrier(c.Request.Header))

		serverSpan := opentracing.StartSpan(c.Request.URL.Path,
			ext.RPCServerOption(wireCtx))
		defer serverSpan.Finish()
		c.Request = c.Request.WithContext(opentracing.ContextWithSpan(c.Request.Context(), serverSpan))
		c.Next()
	}
  }
  ```
  {: codeblock}

8. Aggiungi il middleware al router. **DEVI** aggiungere questa istruzione prima che vengano creati gli endpoint:
  ```go
  router.Use(OpenTracing)
  ...
  router.GET("/health", routes.HealthGET)
  ```
  {: codeblock}

  Questa istruzione configura un'applicazione che può estrarre un'estensione e quindi segnalare i nuovi dati di span. Per impostazione predefinita, tutti i kit starter Go implementano completamente l'OpenTracing lato server.

9. Per inviare i dati di span da un servizio a un altro, devi aggiungere alcune ulteriori istruzioni. Consulta la seguente richiesta di esempio:
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)
  client.Do(req)
  ```
  {: codeblock}

10. Per inviare i dati di span insieme a questa richiesta, aggiungi due istruzioni dopo la creazione della richiesta:
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)

  span := opentracing.SpanFromContext(c.Request.Context())
  opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

  client.Do(req)
  ```
  {: codeblock}

## Passo 2. Configurazione di un server Jaeger locale
{: #setup-jaeger-server}

I server Jaeger sono composti da tre servizi separati:
 * Agent
 * Raccoglitori
 * Query 
 
I servizi Go si collegano all'agent utilizzando l'oggetto `UDPTransport`. Gli agent segnalano quindi i dati ai raccoglitori, che archiviano i dati di span in un database. Il servizio di query consente quindi all'utente di richiamare le estensioni nel database. I servizi vengono separati per consentire la flessibilità perché tutti i servizi devono avere i propri agent a cui collegarsi, mentre il numero dei servizi di raccolta e di query può essere ridimensionato in base alle esigenze.

Tuttavia, per lo sviluppo in locale, Jaeger fornisce un servizio omnicomprensivo, che viene fornito con i servizi di query, database, raccolta e agent nello stesso pacchetto. Questa configurazione è utile per lo sviluppo in locale, ma non per la produzione.

Prima di distribuire la tua applicazione a un qualsiasi cloud, puoi verificare la traccia localmente.

Per ulteriori informazioni sulla distribuzione di Jaeger in un contenitore utilizzando Kubernetes, consulta [questa procedura](#jaeger-kube).

Per eseguire il servizio omnicomprensivo localmente, immetti il seguente comando:
```bash
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 9411:9411 \
  jaegertracing/all-in-one:latest
 ```
{: codeblock}

L'agent può essere collegato sulla porta `5775`, mentre la query sulla porta `16686`.

### Configurazione di un server Jaeger distribuito a Kubernetes
{: #jaeger-kube}

Come per lo sviluppo in locale, Jaeger fornisce un servizio omnicomprensivo per lo sviluppo in Kubernetes. Utilizza il servizio omnicomprensivo solo per lo sviluppo, non per il codice di produzione. Per ulteriori informazioni sulla distribuzione a Kubernetes per la produzione, consulta la [guida Jaeger Kubernetes Templates](https://github.com/jaegertracing/jaeger-kubernetes#production-setup).

Per distribuire il server Jaeger, completa questa procedura:
1. Assicurati che il tuo cluster sia configurato eseguendo `ibmcloud cs cluster-config <cluster name>` e segui le istruzioni.
2. Immetti il seguente comando:
  ```go
  kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
  ```
  {: codeblock}

  Questo comando distribuisce una query, una raccolta e un agent Jaeger al tuo cluster Kubernetes.
3. Prima di distribuire l'applicazione Go a Kubernetes, devi aggiornare il trasporto UDP in modo che punti correttamente all'agent Jaeger. Il comando `kubectl` nel passo precedente crea un endpoint interno denominato `"jaeger-agent:5775"`. Aggiorna il trasporto con questo nuovo endpoint.
  ```go
  transport, err := jaeger.NewUDPTransport("jaeger-agent:5775", 0)
  ```
  {: codeblock}

4. Dopo aver distribuito l'applicazione, puoi visualizzare i dati di traccia passando all'<*IP cluster pubblico*>:<*porta*>. Puoi trovare l'indirizzo IP del cluster pubblico eseguendo `bx cs workers <*cluster name*>`.
Puoi trovare la porta eseguendo `kubectl get service jaeger-query`.

## Passo 3. Verifica di uno scenario di esempio
{: #example-scenario}

Quando segui la precedente procedura, è facile creare due applicazioni Go separate che supportano la traccia. Puoi aggiungere un instradamento a uno dei progetti con il seguente codice:
```go
client := http.Client{}
req, _ := http.NewRequest("GET", "http://localhost:<other port>", nil)

span := opentracing.SpanFromContext(c.Request.Context())
opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

client.Do(req)
```
{: codeblock}

Questo instradamento invia una richiesta `GET` da un'applicazione a un'altra.

Per visualizzare le estensioni, vai all'indirizzo `http://localhost:16686`. Puoi ricercare le tracce dal servizio, dall'operazione e dalle tag, e poi fare clic su **Find Traces**.

![IU Jaeger](images/JaegerUI.png)

Fai clic su una traccia specifica per visualizzare ulteriori informazioni su di essa:
![Esempio di traccia](images/TraceExample.png)
