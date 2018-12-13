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

# Incluindo um robô de bate-papo
{: #assistant}

É possível usar o serviço do {{site.data.keyword.conversationshort}} para construir aplicativos Go que entendem a entrada de língua natural e respondem aos usuários com conversa semelhante à humana.

Como a integração funciona:

* Os usuários interagem com a interface com o usuário de front-end de seu app.
* Seu app envia a entrada do usuário para o {{site.data.keyword.conversationshort}} usando o {{site.data.keyword.ibmwatson}} Go SDK.
* O {{site.data.keyword.watson}} Go SDK se conecta a uma área de trabalho, que é um contêiner para o seu fluxo de diálogo e os seus dados de treinamento.
* A área de trabalho interpreta a entrada do usuário e direciona o fluxo da conversa, enviando uma resposta para o app.
* O front-end do app compartilha a resposta de áudio com o usuário como um `mp3` de reprodução ou um arquivo transferível por download.

É possível incluir o assistente virtual em um novo app do kit do iniciador do Go ou em um app Go existente.

## Antes de começar
{: #before-you-begin}

Instale o {{site.data.keyword.watson}} Go SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## Incluindo um assistente virtual em seu app Go existente
{: #add-a-virtual-assistant-to-your-app}

1. Depois de fazer download do código, abra seu projeto.

2. Inclua uma instrução de importação para o {{site.data.keyword.conversationshort}}.

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

3. Instancie o serviço. O exemplo a seguir usa um chamado `assistant`.

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

4. Interaja com o serviço usando os métodos 'GET' e 'LIST' para uma área de trabalho.

  Área de trabalho de List
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

  Área de trabalho de Get:
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

## Incluindo um robô de bate-papo usando kits do iniciador
{: #conversation_starterkits}

Com Kits do iniciador, é possível usar os recursos nativos do {{site.data.keyword.cloud_notm}} de maneira rápida e fácil. É possível incluir {{site.data.keyword.conversationshort}} em qualquer back-end do lado do servidor usando um Kit do Iniciador. O kit do iniciador do Chatbot for iOS with Watson ilustra como usar os recursos de deep learning do {{site.data.keyword.conversationshort}}, incluindo uma interface de língua natural em seu aplicativo que automatiza interações com seus usuários.

1. Selecione o [Kit do iniciador](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} com o qual você deseja trabalhar.
2. Crie o projeto com os serviços padrão.
3. Clique em **Incluir recursos > Watson > {{site.data.keyword.conversationshort}}**.
4. Faça download do projeto clicando em **Fazer download do código**. É possível localizar as credenciais de serviço no arquivo `config/local-dev.json`.

## Etapas Seguintes
{: #assistant_next}

Ótimo trabalho! Você incluiu um assistente de AI em seu app. Tente uma das opções a seguir para manter o ritmo:
* Confira o [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Tire proveito de todos os recursos que o [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) oferece.
