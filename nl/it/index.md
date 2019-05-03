---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Esercitazione introduttiva
{: #getting-started}

La seguente esercitazione ti illustra la procedura per creare e distribuire un'applicazione Go utilizzando gli strumenti {{site.data.keyword.cloud_notm}} forniti. Puoi utilizzare [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) sulla riga di comando oppure l'[{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) basato sul web, come mostrato nei seguenti passi dell'esercitazione. Utilizzando entrambi questi metodi, puoi generare un'applicazione Go pronta per la produzione in pochi minuti.

## Passo 1. Creazione di un'applicazione Go nel dashboard
{: #create-go-app}

1. Dalla pagina [Starter Kits](https://cloud.ibm.com/developer/appservice/starter-kits) in App Service, seleziona un kit starter scritto in `Go`. Puoi anche creare un'applicazione starter vuota facendo clic su **Create app** e selezionando `Go` come linguaggio.

    Per creare un'applicazione, devi essere collegato a un account {{site.data.keyword.cloud_notm}}. Se non hai un account, puoi [registrarti per un account gratuito](https://cloud.ibm.com/registration).
    {: tip}

3. Fai clic su **Create app**.
4. **Fornisci un nome** alla tua applicazione o utilizza il nome dell'applicazione generico che viene fornito.
5. Immetti un **nome host univoco**. Il nome host viene utilizzato per accedere alla tua applicazione, ad esempio: `server.mybluemix.net`.
6. Fai clic su **Create**. Dopo che la tua applicazione è stata creata, puoi eseguire la distribuzione utilizzando una toolchain oppure puoi continuare l'attività di creazione e distribuire il tuo progetto dalla [riga di comando](/docs/cli/index.html#ibmcloud-cli).

## Passo 2. Distribuzione con il dashboard
{: #deploy-go}

1. Dalla pagina App Details, fai clic su **Deploy to Cloud**.
2. Configura il tuo metodo di distribuzione in base alle istruzioni per il metodo che scegli.
  * **Distribuisci a [Kubernetes](/docs/apps/deploying/containers.html#containers)**. Questa opzione crea un cluster di host, denominati nodi di lavoro, per distribuire e gestire contenitori applicazione ad elevata disponibilità. Puoi creare un cluster o distribuire un cluster esistente.
  * **Distribuisci a Cloud Foundry**. Questa opzione distribuisce la tua applicazione nativa del cloud senza che tu debba gestire l'infrastruttura sottostante. Se il tuo account ha accesso a {{site.data.keyword.cfee_full_notm}}, puoi selezionare un tipo di deployer **[Cloud pubblico](/docs/cloud-foundry-public/about-cf.html#about-cf)** o **[Ambiente aziendale](/docs/cloud-foundry-public/cfee.html#cfee)**, che puoi utilizzare per creare e gestire ambienti isolati per ospitare applicazioni Cloud Foundry esclusivamente per la tua azienda.
  * **Distribuisci a un [Virtual Server](/docs/apps/vsi-deploy.html#vsi-deploy)**. Questa opzione eseguire il provisioning di un'istanza del server virtuale, carica un'immagine che include la tua applicazione, crea una toolchain DevOps e avvia il primo ciclo di distribuzione per tuo conto.

3. Seleziona le tue opzioni e fai quindi clic su **Crea**. Viene creata una toolchain per tuo conto e viene distribuita la tua applicazione in automatico.

## Passo 3: Aggiunta di una risorsa (facoltativo)
{: #add-resource-go}

Puoi aggiungere velocemente dei servizi come la sicurezza o l'archiviazione alla tua applicazione Go dall'interno del tuo dashboard.

1. Ritorna alla tua applicazione Go nell'[IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard).
2. Fai clic su **Add Resource**, seleziona la categoria del servizio che vuoi aggiungere, fai clic su **Next** e scegli quindi il tuo servizio. {{site.data.keyword.cloud_notm}} App Service crea il servizio per tuo conto in base al piano selezionato. Se hai precedentemente creato il servizio che pensi di utilizzare, scegli la categoria **Existing**.
3. Dopo aver creato il servizio, fai clic su **Download Code** per rigenerare il progetto con l'SDK che si connette al tuo servizio.

## Passo 4. Verifica e accesso alla tua applicazione Go in locale
{: #run_local-go}

Puoi utilizzare [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) per lavorare con la tua applicazione Go in locale utilizzando i comandi `ibmcloud dev`.

1. Utilizza il comando `ibmcloud dev build` per creare la tua applicazione.
2. Utilizza il comando `ibmcloud dev run` per eseguire l'applicazione in locale. La tua applicazione viene eseguita nei contenitori Docker sul tuo sistema locale. Puoi verificare la tua applicazione in un browser accedendo a `http://localhost:8080`.
3. Un endpoint di controllo dell'integrità è disponibile all'indirizzo `http://localhost:8080/health`.
4. Puoi accedere alle metriche all'indirizzo `http://localhost:3000/metrics`.

Per ulteriori informazioni su {{site.data.keyword.dev_cli_long}}, consulta la [documentazione della CLI](/docs/cli/index.html#ibmcloud-cli) completa.

## Esplorazione di metodi di distribuzione alternativi
{: #deploy_cloud-go notoc}

Impara a distribuire la tua applicazione nella scheda Distribuzioni [qui](/docs/go/deploying_apps.html). 

Per espandere la tua applicazione Go, continua a consultare gli argomenti nella guida di programmazione Go.
