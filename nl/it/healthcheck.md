---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: healthcheck go, add healthcheck, healthcheck endpoint, readiness go, liveness go, endpoint go, probes go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilizzo di un controllo di integrità nella tua applicazione Go
{: #go-healthcheck}

I controlli di integrità forniscono un semplice meccanismo per determinare se un'applicazione lato server sta funzionando correttamente o meno. Gli ambienti cloud come [Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") e [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") possono essere configurati per eseguire il polling degli endpoint di integrità periodicamente per determinare se un'istanza del tuo servizio è pronta ad accettare traffico.

## Panoramica sul controllo di integrità
{: #go-healtcheck-overview}

I controlli di integrità forniscono un semplice meccanismo per determinare se un'applicazione lato server sta funzionando correttamente o meno. Normalmente è possibile accedervi tramite HTTP e utilizzano i codici di ritorno standard per indicare lo stato UP o DOWN. Il valore di ritorno di un controllo di integrità è variabile, ma una risposta JSON minima, come `{"status": "UP"}`, è normale.

Kubernetes ha una nozione con più sfumature dell'integrità del processo. Definisce due probe:

- Viene utilizzato un probe di _**disponibilità**_ per indicare se il processo può gestire le richieste (è instradabile).

  Kubernetes non instrada il lavoro a un contenitore con un probe di disponibilità in errore. Un probe di disponibilità può avere esito negativo se un servizio non ha terminato l'inizializzazione o se è altrimenti occupato, sovraccaricato o se non può elaborare le richieste.

- Viene utilizzato un probe di _**attività**_ per indicare se il processo deve essere riavviato.

  Kubernetes arresta e riavvia un contenitore con un probe di attività in errore. Utilizza i probe di attività per ripulire i processi in uno stato irreversibile, ad esempio, se la memoria è esaurita o se il contenitore non si è arrestato correttamente dopo un processo interno non riuscito.

Come titolo di confronto, Cloud Foundry utilizza un endpoint di integrità. Se questo controllo non riesce, il processo viene riavviato, ma se ha esito positivo, gli vengono instradate le richieste. In questo ambiente, l'endpoint come minimo ha esito positivo quando il processo è live. Viene configurato un ritardo iniziale per posporre il controllo di integrità finché il servizio non termina l'inizializzazione per evitare cicli di riavvio.

La seguente tabella fornisce le indicazioni sulle risposte che devono essere fornite dagli endpoint di disponibilità, di attività e di integrità singolare.

| Stato    | Disponibilità                   | Attività                   | Integrità                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Non OK provoca nessun caricamento       | Non OK provoca il riavvio      | Non OK provoca il riavvio     |
| In avvio | 503 - Non disponibile           | 200 - OK                   | Utilizza il ritardo per evitare il test   |
| Attivo       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| In arresto | 503 - Non disponibile           | 200 - OK                   | 503 - Non disponibile         |
| Non attivo     | 503 - Non disponibile           | 503 - Non disponibile          | 503 - Non disponibile         |
| In errore  | 500 - Errore server          | 500 - Errore server         | 500 - Errore server        |

## Aggiunta di un controllo di integrità a un'applicazione Go esistente
{: #go-add-healthcheck-existing}

Puoi aggiungere un controllo di attività o di integrità minimo a un'applicazione `Gin-Gonic` esistente introducendo un nuovo instradamento come illustrato nel seguente esempio:
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

Controlla lo stato dell'applicazione con un browser accedendo all'endpoint `/health`.

Ci sono ulteriori librerie estese disponibili, come [`http-healthcheck`](https://github.com/robzienert/http-healthcheck){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno"), che consentono la definizione di controlli di integrità estensibili con il supporto per la memorizzazione nella cache dei controlli che vengono eseguiti sui servizi di backup. In questo caso, vuoi separare il test di attività semplice nell'esempio dal controllo di disponibilità più dettagliato e solido creato dal pacchetto del controllo di integrità.

## Accesso all'endpoint del controllo di integrità nelle applicazioni kit starter Go
{: #go-healthcheck-starterkit}

Per impostazione predefinita, quando generi un'applicazione Go utilizzando un kit starter,
è disponibile un endpoint del controllo di integrità (non autorizzato) di base in `/health` per verificare lo stato dell'applicazione (UP/DOWN).

Il codice dell'endpoint del controllo di integrità viene fornito dal seguente file `/routers/health.go`:
```go
package routers

import (
  "github.com/gin-gonic/gin"
)

func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
}
```
{: codeblock}

## Suggerimenti per i probe di disponibilità e di attività
{: #go-recommend-healthcheck}

I probe di disponibilità dovrebbero includere l'applicabilità delle connessioni ai servizi in downstream nei propri risultati quando non è presente un fallback accettabile se il servizio in downstream non è disponibile. Questo non significa di chiamare il controllo di integrità fornito direttamente dal servizio in downstream, perché l'infrastruttura esegue il controllo per te. Invece, prendi in considerazione di verificare l'integrità dei riferimenti esistenti che la tua applicazione ha con i servizi in downstream: che potrebbero essere una connessione JMS a WebSphere MQ o un consumatore o produttore Kafka inizializzato. Se non controlli l'applicabilità dei riferimenti interni ai servizi in downstream, memorizza nella cache il risultato per ridurre al minimo l'impatto del controllo di integrità sulla tua applicazione.

Un probe di attività, al contrario, è cauto su cosa viene controllato, perché un errore comporta una terminazione immediata del processo. Un endpoint HTTP semplice che restituisce sempre `{"status": "UP"}` con il codice di stato `200` è una scelta ragionevole.

## Configurazione dei probe di disponibilità e di attività in Kubernetes
{: #go-config-probes-healthcheck}

Dichiara i probe di disponibilità e di attività insieme alla tua distribuzione Kubernetes. Entrambi i probe utilizzano gli stessi parametri di configurazione.

* Il kubelet attende i secondi del ritardo iniziale (`initialDelaySeconds`) prima del primo probe.

* Il kubelet analizza il servizio ogni `periodSeconds` secondi. Il valore predefinito è 1.

* Il probe va in timeout dopo `timeoutSeconds` secondi. Il valore predefinito e minimo è 1.

* Il probe ha esito positivo se riesce `successThreshold` volte dopo un errore. Il valore predefinito e minimo è 1. Il valore deve essere 1 per i probe di attività.

* Quando si avvia un pod e il probe ha esito negativo, Kubernetes tenta `failureThreshold` volte di riavviare il pod dopodiché smette. Il valore minimo è 1 e il valore predefinito è 3.
    - Per un probe di attività, "smettere" significa riavviare il pod.
    - Per un probe di disponibilità, "smettere" significa contrassegnare il pod come `Unready`.

Per evitare i cicli di riavvio, imposta `livenessProbe.initialDelaySeconds` in modo da essere tranquillamente più lungo del tempo necessario al tuo servizio per l'inizializzazione. Puoi poi utilizzare un valore più corto per `readinessProbe.initialDelaySeconds` per instradare le richieste al servizio come è pronto.

Una configurazione di esempio è simile a quanto segue:
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```
{: codeblock}

Per ulteriori informazioni, consulta [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
