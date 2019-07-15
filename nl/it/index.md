---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Esercitazione introduttiva
{: #getting-started}

La seguente esercitazione ti illustra la procedura per creare e distribuire un'applicazione Go utilizzando gli strumenti {{site.data.keyword.cloud_notm}} forniti. Puoi utilizzare [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-getting-started) sulla riga di comando oppure [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} basata sul web ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno"), come mostrato nei seguenti passi dell'esercitazione. Utilizzando entrambi questi metodi, puoi generare un'applicazione Go pronta per la produzione in pochi minuti.

## Passo 1. Creazione di un'applicazione Go personalizzata nel dashboard
{: #create-go-app}

1. Per creare un'applicazione, accedi a un account {{site.data.keyword.cloud_notm}}. Se non hai un account, puoi [eseguire la registrazione per un account gratuito](https://{DomainName}/registration){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno").
2. Dalla pagina [Kit starter](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") nella console {{site.data.keyword.dev_console}}, esegui una delle seguenti opzioni:
 * Seleziona un kit starter scritto in `Go` e fai quindi clic su **Crea applicazione** nella pagina **Dettagli kit starter**.
 * Seleziona l'applicazione starter vuota e fai clic su **Crea applicazione**.
3. Fornisci un nome per la tua applicazione oppure utilizza il nome di applicazione generico che viene fornito.
4. Assicurati che **Go** sia selezionata come piattaforma e fai quindi clic su **Crea**. Dopo che la tua applicazione è stata creata, puoi aggiungere i servizi e quindi distribuirla utilizzando una toolchain oppure puoi continuare a creare e distribuire il tuo progetto dalla [riga di comando](/docs/cli?topic=cloud-cli-getting-started).

## Passo 2: Aggiunta di {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

Puoi ora aggiungere i servizi {{site.data.keyword.watson}} alla tua applicazione Go. Per questa esercitazione, aggiungi il servizio {{site.data.keyword.texttospeechshort}} alla tua applicazione Go, che prende l'input verbale e lo converte in testo utilizzando un'API cloud.

1. Dalla pagina **Dettagli applicazione**, fai clic su **Aggiungi servizio**.
2. Seleziona **AI** e fai clic su **Avanti**.
3. Seleziona **{{site.data.keyword.texttospeechshort}}** e fai clic su **Next**.
4. Fai clic su **Create**.

Puoi vedere che la dipendenza [SDK Go di Watson Developer Cloud](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") è aggiunta al file `Gopkg.toml` e del semplice codice di strumentazione è disponibile per il servizio nella directory `services/`. Sono inoltre incluse le informazioni di configurazione per accedere alle credenziali del servizio nei rispettivi ambienti.

Per scaricare il codice, fai clic su **Download code** nella pagina **App details**. Il codice scaricato viene fornito con incluso l'[SDK Go di Watson Developer Cloud](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno") e del codice di inizializzazione di base.

## Passo 3: Distribuzione della tua applicazione dalla console
{: #deploy-go}

1. Dalla pagina **App details**, fai clic su **Configure continuous delivery**.
2. Configura la tua destinazione di distribuzione in base alle istruzioni per il metodo che scegli:
  * **Distribuisci a [IBM Kubernetes Service](/docs/containers?topic=containers-app)**. Questa opzione crea un cluster di host, denominati nodi di lavoro, per distribuire e gestire contenitori applicazione ad elevata disponibilità. Puoi creare un cluster o distribuire un cluster esistente.
  * **Distribuisci a Cloud Foundry**. Questa opzione distribuisce la tua applicazione nativa del cloud senza che tu debba gestire l'infrastruttura sottostante. Se il tuo account ha accesso a {{site.data.keyword.cfee_full_notm}}, puoi selezionare un tipo di deployer **[Cloud pubblico](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** o **[Ambiente aziendale](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**, che puoi utilizzare per creare e gestire ambienti isolati per ospitare applicazioni Cloud Foundry esclusivamente per la tua azienda.
  * **Distribuisci a un [Virtual Server](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)**. Questa opzione esegue il provisioning di un'istanza del server virtuale, carica un'immagine che include la tua applicazione, crea una toolchain DevOps e avvia il ciclo di distribuzione per tuo conto.

3. Dopo che hai configurato la tua destinazione di distribuzione, fai clic su **Next**.
4. Seleziona le tue opzioni di configurazione e fai quindi clic su **Create**. Viene creata una toolchain per tuo conto e la tua applicazione viene distribuita automaticamente.

## Passo 4. Verifica e accesso alla tua applicazione Go in locale
{: #run_local-go}

Nella pagina **App details**, puoi:
* Visualizzare il tuo repository facendo clic su **View repo**.
* Visualizzare e modificare la tua toolchain facendo clic su **View toolchain**.

Puoi utilizzare {{site.data.keyword.dev_cli_long}} per lavorare con la tua applicazione Go in locale utilizzando i comandi `ibmcloud dev`. Questi strumenti di aiutano a iterare in locale ed eseguire il push delle tue modifiche al cloud in modo rapido.

1. Utilizza il comando `ibmcloud dev build` per creare la tua applicazione. 
2. Utilizza il comando `ibmcloud dev run` per eseguire l'applicazione in locale. La tua applicazione viene eseguita nei contenitori Docker sul tuo sistema locale. Puoi verificare la tua applicazione in un browser accedendo a `http://localhost:8080`.
3. Un endpoint di controllo dell'integrità è disponibile all'indirizzo `http://localhost:8080/health`.
4. Puoi accedere alle metriche all'indirizzo `http://localhost:3000/metrics`.

Per ulteriori informazioni su {{site.data.keyword.dev_cli_notm}}, consulta la [documentazione della CLI](/docs/cli?topic=cloud-cli-getting-started) completa.

Sei ora pronto a creare la tua applicazione con il servizio {{site.data.keyword.texttospeechshort}}.

## Esplorazione di metodi di distribuzione alternativi
{: #alt-deploy-go notoc}

Impara come distribuire la tua applicazione nella [scheda Distribuzioni](/docs/go?topic=go-go-deploy-apps).

Per espandere la tua applicazione Go, continua a consultare gli argomenti nella guida di programmazione Go.
