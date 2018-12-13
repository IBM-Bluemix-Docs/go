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

# Esercitazione introduttiva

La seguente esercitazione ti illustra la procedura per creare e distribuire un'applicazione Go utilizzando gli strumenti {{site.data.keyword.cloud_notm}} forniti. Puoi utilizzare [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) sulla riga di comando oppure l'[IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) basato sul web, come mostrato nei seguenti passi dell'esercitazione. Utilizzando entrambi questi metodi, puoi generare un'applicazione Go pronta per la produzione in pochi minuti.

## Passo 1. Creazione di un'applicazione Go nel dashboard
{: #create_project}

1. Dalla pagina [Starter Kits](https://console.bluemix.net/developer/appservice/starter-kits) in App Service, seleziona un kit starter scritto in `Go`. Puoi anche creare un'applicazione starter vuota facendo clic su **Create app** e selezionando `Go` come linguaggio.

    Devi aver eseguito l'accesso a un account {{site.data.keyword.cloud_notm}} per creare un progetto. Se non hai un account, puoi [registrarti per un account gratuito](https://console.bluemix.net/registration).
    {: tip}

3. Fai clic su **Create app**.
4. **Fornisci un nome** alla tua applicazione o utilizza il nome dell'applicazione generico che viene fornito.
5. Immetti un **nome host univoco**. Il nome host viene utilizzato per accedere alla tua applicazione, ad esempio: `server.mybluemix.net`.
6. Fai clic su **Create**. Dopo che il tuo progetto è stato creato, puoi distribuirlo utilizzando una toolchain oppure puoi continuare a creare e distribuire il tuo progetto dalla [riga di comando](/docs/cli/idt/index.html).

## Passo 2. Distribuzione con il dashboard
{: #deploy_dashboard}

1. Dalla pagina App Details, fai clic su **Deploy to Cloud**.
2. Successivamente, seleziona uno dei seguenti metodi di distribuzione:
    * **Distribuisci a Kubernetes** - Devi creare una serie di nodi di lavoro. Ad esempio, puoi utilizzare le VM per distribuire e gestire i contenitori dell'applicazione ad elevata disponibilità. Puoi creare un cluster o distribuire un cluster esistente. 
    * **Distribuisci a Cloud Foundry** - Non hai bisogno di gestire l'infrastruttura sottostante.
    * **Distribuisci a un Virtual Server (beta)** - È necessario un [account Pagamento a consumo](https://console.bluemix.net/dashboard/ibm-iaas-g1) per accedere ai server virtuali.
3. Seleziona le tue opzioni e fai quindi clic su **Crea**. Viene creata una toolchain per tuo conto e viene distribuita la tua applicazione in automatico.

## Passo 3. Aggiunta di un servizio (facoltativo)
{: #add_service}

Puoi aggiungere velocemente dei servizi come la sicurezza o l'archiviazione alla tua applicazione Go dall'interno del tuo dashboard.

1. Ritorna alla tua applicazione Go nell'[IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard).
2. Fai clic su **Add Resource**, seleziona la categoria del servizio che vuoi aggiungere, fai clic su **Next** e scegli quindi il tuo servizio. {{site.data.keyword.cloud_notm}} App Service crea il servizio per tuo conto in base al piano selezionato. Se hai precedentemente creato il servizio che pensi di utilizzare, scegli la categoria **Existing**.
3. Dopo aver creato il servizio, fai clic su **Download Code** per rigenerare il progetto con l'SDK che si connette al tuo servizio.

## Passo 4. Verifica e accesso alla tua applicazione Go in locale
{: #run_local}

Puoi utilizzare [{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) per lavorare con la tua applicazione Go in locale utilizzando i comandi `ibmcloud dev`.

1. Utilizza il comando `ibmcloud dev build` per creare la tua applicazione. 
2. Utilizza il comando `ibmcloud dev run` per eseguire l'applicazione in locale. La tua applicazione viene eseguita nei contenitori Docker sul tuo sistema locale. Puoi verificare la tua applicazione in un browser accedendo a `http://localhost:8080`.
3. Un endpoint di controllo dell'integrità è disponibile all'indirizzo `http://localhost:8080/health`.
4. Puoi accedere alle metriche all'indirizzo `http://localhost:3000/metrics`.

Per ulteriori informazioni su {{site.data.keyword.dev_cli_long}}, consulta la [documentazione della CLI](/docs/cli/idt/index.html) completa.

## Esplorazione di metodi di distribuzione alternativi 
{: #deploy_cloud notoc}

Impara a distribuire la tua applicazione a Cloud Foundry o Kubernetes nella scheda Deployments [qui](/docs/go/deploying_apps.html). 

Per espandere la tua applicazione Go, continua a consultare gli argomenti nella guida di programmazione Go.
