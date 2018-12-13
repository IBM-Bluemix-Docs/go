---

copyright:
  years: 2018
lastupdated: "2018-09-25"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Ajout d'un agent conversationnel
{: #assistant}

Vous pouvez utiliser le service {{site.data.keyword.conversationshort}} pour créer des applications Go qui comprennent les entrées en langage naturel et répondent aux utilisateurs par une conversation humaine.

L'intégration fonctionne de la manière suivante :

* Les utilisateurs interagissent avec l'interface utilisateur frontale de votre application.
* Votre application envoie une entrée utilisateur à {{site.data.keyword.conversationshort}} à l'aide du logiciel SDK Go {{site.data.keyword.ibmwatson}}.
* Le logiciel SDK Go {{site.data.keyword.watson}} se connecte à un espace de travail, qui est un conteneur pour votre flux de dialogues et vos données d'apprentissage.
* L'espace de travail interprète l'entrée utilisateur et dirige le flux des conversations, en envoyant une réponse à votre application.
* L'interface frontale de votre application partage la réponse audio avec l'utilisateur sous la forme d'un fichier `mp3` reproductible ou d'un fichier téléchargeable.

Vous pouvez ajouter l'assistant virtuel à une nouvelle application Go du kit de démarrage ou à une application Go existante.

## Avant de commencer
{: #before-you-begin}

Installez le logiciel SDK Go {{site.data.keyword.watson}} :
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## Ajout d'un assistant virtuel à votre application Go existante
{: #add-a-virtual-assistant-to-your-app}

1. Après avoir téléchargé le code, ouvrez votre projet.

2. Ajoutez une instruction d'importation pour {{site.data.keyword.conversationshort}}.

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

3. Instanciez le service. L'exemple suivant utilise `assistant`.

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

4. Interagissez avec le service à l'aide des méthodes 'GET' et 'LIST' pour un espace de travail.

  Pour afficher l'espace de travail avec la méthode LIST :
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

  Pour obtenir l'espace de travail avec la méthode GET :
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

## Ajout d'un agent conversationnel à l'aide des kits de démarrage
{: #conversation_starterkits}

Les kits de démarrage vous permettent d'utiliser les fonctionnalités natives d'{{site.data.keyword.cloud_notm}} de manière rapide et facile. Vous pouvez ajouter {{site.data.keyword.conversationshort}} à n'importe quel système de back-end côté serveur à l'aide d'un kit de démarrage. Le kit de démarrage Chatbot for iOS with Watson illustre comment utiliser les fonctions d'apprentissage en profondeur de {{site.data.keyword.conversationshort}} par l'ajout d'une interface en langage naturel à votre application qui automatise les interactions avec vos utilisateurs.

1. Sélectionnez le [kit de démarrage](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} avec lequel vous souhaitez travailler.
2. Créez le projet avec les services par défaut.
3. Cliquez sur **Ajouter des ressources > Watson > {{site.data.keyword.conversationshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Vous trouverez les données d'identification du service dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #assistant_next}

Félicitations ! Vous avez ajouté un assistant d'intelligence artificielle à votre application. Continuez sur votre lancée en essayant l'une des options suivantes :
* Consultez le [logiciel SDK Go {{site.data.keyword.watson}} ](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Tirez parti des toutes les fonctions offertes par [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html).
