---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Aggiunta di una chatbot
{: #assistant-chatbot}

Puoi utilizzare il servizio {{site.data.keyword.conversationshort}} per creare applicazioni Go che comprendono l'input in linguaggio naturale e rispondono agli utenti con una conversazione simile a quella umana.

Come funziona l'integrazione:

* Gli utenti interagiscono con l'interfaccia utente di frontend della tua applicazione.
* La tua applicazione invia l'input utente a {{site.data.keyword.conversationshort}} utilizzando l'SDK Go {{site.data.keyword.ibmwatson}}.
* L'SDK Go {{site.data.keyword.watson}} stabilisce una connessione a uno spazio di lavoro, che è un contenitore per il tuo flusso di dialogo e i tuoi dati di training.
* Lo spazio di lavoro interpreta l'input utente e indirizza il flusso della conversazione, inviando una risposta alla tua applicazione.
* Il frontend della tua applicazione condivide la risposta audio con l'utente come un `mp3` riproducibile o un file scaricabile.

Puoi aggiungere l'assistente virtuale a una nuova applicazione kit starter Go o a un'applicazione Go esistente.

## Prima di cominciare
{: #prereqs-chatbot}

Installa l'SDK Go {{site.data.keyword.watson}}:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## Aggiunta di un assistente virtuale alla tua applicazione Go esistente
{: #existing-chatbot}

1. Dopo aver scaricato il codice, apri il tuo progetto.

2. Aggiungi un'istruzione di importazione per {{site.data.keyword.conversationshort}}.

  ```golang
  package main

  import (
    "fmt"
    "go-sdk/assistantv1"
    "encoding/json"
  )

  // Used for printing formatted result 
  func prettyPrint(result interface{}, resultName string) {
    output, err := json.MarshalIndent(result, "", "    ")

    if err == nil {
      fmt.Printf("%v:\n%+v\n\n", resultName, string(output))
    }
  }
  ```
  {: codeblock}

3. Istanzia il servizio. Il seguente esempio ne utilizza uno denominato `assistant`.

  ```golang
  func main() {
    // Instantiate the Watson Assistant service
    assistant, assistantErr := assistantv1.NewAssistantV1(&ServiceCredentials{
      ServiceURL: "YOUR SERVICE URL",
      Version: "2018-07-10",
      Username: "YOUR SERVICE USERNAME",
      Password: "YOUR SERVICE PASSWORD",
    })

    // Check successful instantiation
    if assistantErr != nil {
      fmt.Println(assistantErr)
      return
    }
  ```
  {: codeblock}

4. Interagisci con il servizio utilizzando i metodi 'GET' e 'LIST' per uno spazio di lavoro.

  Elenca lo spazio di lavoro:
  ```golang
  // Call the assistant ListWorkspaces method
  list, listErr := assistant.ListWorkspaces(assistantv1.NewListWorkspacesOptions())

  // Check successful call
  if listErr != nil {
    fmt.Println(listErr)
    return
  }

  // Cast list.Result to the specific dataType returned by ListWorkspaces
  // NOTE: most methods have a corresponding Get<methodName>Result() function
  listResult := assistantv1.GetListWorkspacesResult(list)

  // Check successful casting
  if listResult != nil {
    prettyPrint(listResult, "List Workspaces")
  }
  ```
  {: codeblock}

  Ottieni lo spazio di lavoro:
  ```golang
  // Call the assistant GetWorkspace method
  getWorkspaceOptions := assistantv1.NewGetWorkspaceOptions(listResult.Workspaces[0].WorkspaceID)
  get, getErr := assistant.GetWorkspace(getWorkspaceOptions)

  // Check successful call
  if getErr != nil {
    fmt.Println(getErr)
    return
  }

  // Cast result
  getResult := assistantv1.GetGetWorkspaceResult(get)

  // Check successful casting
  if getResult != nil {
    prettyPrint(getResult, "Get Workspace")
    }
  }
  ```
  {: codeblock}

## Aggiunta di una chatbot utilizzando i kit starter
{: #starterkits-chatbot}

Con i kit starter, puoi velocemente e facilmente utilizzare le funzionalità native di {{site.data.keyword.cloud_notm}}. Puoi aggiungere {{site.data.keyword.conversationshort}} a qualsiasi backend lato server utilizzando un kit starter. Il kit starter Watson Chatbot for iOS illustra come utilizzare le funzionalità di apprendimento approfondito di {{site.data.keyword.conversationshort}}, aggiungendo un'interfaccia di linguaggio naturale alla tua applicazione che automatizza le interazioni con i tuoi utenti.

1. Seleziona il [kit starter](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} con cui vuoi lavorare.
2. Crea il progetto con i servizi predefiniti.
3. Fai clic su **Add Resources > Watson > {{site.data.keyword.conversationshort}}**.
4. Scarica il progetto facendo clic su **Download Code**. Puoi trovare le credenziali del servizio nel file `config/local-dev.json`.

## Passi successivi
{: #next-chatbot}

Ottimo lavoro. Hai aggiunto un assistente di intelligenza artificiale alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:
* Consulta l'[SDK Go {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Avvaliti di tutte le funzioni che [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) ha da offrire.
