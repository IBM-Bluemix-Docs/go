---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurazione dell'ambiente Go
{: #configure-go-env}

Sono disponibili delle linee guida standard da seguire per lo sviluppo delle applicazioni Go che ti aiutano a mantenere la portabilità congruente. Alcune considerazioni da fare includono la gestione delle credenziali, il salvataggio dei dati, l'archiviazione dei dati e la pubblicazione del contenuto nel cloud. Seguendo le prassi native cloud, un'applicazione Go può essere facilmente spostata da un'ambiente a un altro. Ad esempio, dal test alla produzione, senza modificare il codice o attuare percorsi di codice non testati in precedenza.

Se devi aggiungere il supporto cloud alle applicazioni Go esistenti o creare applicazioni Go con i kit starter, l'obiettivo è quello di fornire la portabilità per l'utilizzo su qualsiasi piattaforma di distribuzione.

## Aggiunta del supporto cloud alle applicazioni Go esistenti
{: #go-add-cloud-support}

Il modulo [`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") aggrega le variabili di ambiente da diversi provider cloud, come CloudFoundry e Kubernetes, in modo che l'applicazione sia indipendente dall'ambiente.

### Installazione del modulo `ibm-cloud-env-golang`
{: #go-install-env-module}

1. Installa il modulo `ibm-cloud-env-golang` con il seguente comando:
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. Inizializza il modulo nel tuo codice facendo riferimento al file `mappings.json`:
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Poiché Go non supporta i parametri predefiniti, non viene fornito un file `mappings.json` predefinito. Se il percorso del file delle associazioni non viene specificato in `IBMCloudEnv.Initialize()`, viene registrato un errore. 
  {: tip}

  File `mappings.json` di esempio:
  ```javascript
  {
      "service1-credentials": {
          "searchPatterns": [
              "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name", 
              "env:my-service1-credentials", 
              "file:/localdev/my-service1-credentials.json" 
          ]
      },
      "service2-username": {
         "searchPatterns":[
              "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### Richiamo delle credenziali del servizio
{: #go-get-creds}

Richiama i valori nella tua applicazione utilizzando i seguenti comandi.

1. Richiama la variabile `service1credentials`:
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. Richiama la variabile `service2username`:
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

Ora la tua applicazione può essere implementata in ogni ambiente di runtime astraendo le differenze introdotte dai diversi provider di elaborazione cloud.

### Filtro dei valori per tag ed etichette
{: #go-filter-creds}

Puoi filtrare le credenziali generate dal modulo in base alle tag di servizio e alle etichette di servizio, come mostrato nel seguente esempio:
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## Utilizzo del gestore configurazione Go dalle applicazioni kit starter
{: #go-config-manager}

Le applicazioni Go create con i [Kit starter](https://cloud.ibm.com/developer/appservice/starter-kits/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") vengono automaticamente fornite con le credenziali e le configurazioni necessarie per l'esecuzione in molte destinazioni di distribuzione cloud, come ad esempio Cloud Foundry, Kubernetes, VSI e Functions).

### Descrizione delle credenziali del servizio
{: #go-credentials-config}

Le tue informazioni sulla configurazione dell'applicazione per i servizi vengono memorizzate nel file `localdev-config.json` nella directory `/server/config`. Il file si trova nella directory `.gitignore` per evitare che informazioni sensibili vengano memorizzate in Git. Le informazioni sulla connessione per qualsiasi servizio configurato che viene eseguito localmente, come nome utente, password e nome host, sono memorizzate in questo file.

L'applicazione utilizza il gestore configurazione per leggere le informazioni sulla connessione e sulla configurazione dall'ambiente e da questo file. Utilizza un `mappings.json` integrato e personalizzato, che si trova nella directory `server/config`, per comunicare dove è possibile trovare le credenziali per ciascun servizio.

Se l'applicazione viene eseguita in locale, può essere collegata ai servizi {{site.data.keyword.cloud_notm}} utilizzando le credenziali non associate lette dal file `mappings.json`. Se devi invece creare delle credenziali in uscita, puoi farlo dalla console web {{site.data.keyword.cloud_notm}} (esempio) oppure utilizzando il comando `cf create-service-key` della [CLI CloudFoundry](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").

Quando esegui il push della tua applicazione a {{site.data.keyword.cloud_notm}}, questi valori non vengono più utilizzati. L'applicazione stabilisce invece automaticamente una connessione ai servizi di cui è stato eseguito il bind utilizzando le variabili di ambiente. 

* **Cloud Foundry**: le credenziali del servizio vengono prese dalla variabile di ambiente `VCAP_SERVICES`.

* **Kubernetes**: le credenziali del servizio vengono prese dalle singole variabili di ambiente per ogni servizio.

* **{{site.data.keyword.cloud_notm}} Container Service**: le credenziali del servizio vengono prese dalle istanze del server virtuale o {{site.data.keyword.openwhisk}} (Openwhisk).

## Passi successivi
{: #go-next-steps-config notoc}

`ibm-cloud-env-golang` supporta la ricerca dei valori utilizzando quattro tipi di modello di ricerca: `user-provided`, `cloudfoundry`, `env` e `file`. Se desideri controllare gli altri modelli di ricerca supportati e gli esempi del modello di ricerca, consulta la sezione [Usage](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
