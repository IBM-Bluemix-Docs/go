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

# Adición de un chatbot
{: #assistant}

Puede utilizar el servicio {{site.data.keyword.conversationshort}} para crear aplicaciones Go que comprendan la entrada de lenguaje natural y respondan a los usuarios con conversación de tipo humano.

Funcionamiento de la integración:

* Los usuarios interactúan con la interfaz de usuario frontal de su app.
* La app envía la entrada de usuario a {{site.data.keyword.conversationshort}} utilizando el SDK de Go de {{site.data.keyword.ibmwatson}}.
* El SDK de Go de {{site.data.keyword.watson}} se conecta a un espacio de trabajo, que es un contenedor para el flujo de diálogo y los datos de entrenamiento.
* El espacio de trabajo interpreta la entrada de usuario y dirige el flujo de la conversación, enviando una respuesta a la app.
* La parte frontal de la app comparte la respuesta de audio con el usuario como `mp3` reproducible o archivo descargable.

Puede añadir el asistente virtual a una nueva app del kit de inicio de Go o a una app Go existente.

## Antes de empezar
{: #before-you-begin}

Instale el SDK de Go de {{site.data.keyword.watson}}:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## Adición de un asistente virtual a la app Go
{: #add-a-virtual-assistant-to-your-app}

1. Tras descargar el código, abra el proyecto.

2. Añada una sentencia de importación para {{site.data.keyword.conversationshort}}.

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

3. Cree una instancia del servicio. En el ejemplo siguiente se utiliza uno llamado `assistant`.

  ```golang
  func main() {
    // Crear instancia del servicio Watson Assistant
    assistant, assistantErr := assistantv1.NewAssistantV1(&ServiceCredentials{
      ServiceURL: "YOUR SERVICE URL",
      Version: "2018-07-10",
      Username: "YOUR SERVICE USERNAME",
      Password: "YOUR SERVICE PASSWORD",
    })

    // Comprobar si la instancia es correcta
    if assistantErr != nil {
      fmt.Println(assistantErr)
      return
    }
  ```
  {: codeblock}

4. Interactúe con el servicio utilizando los métodos 'GET' y 'LIST' para un espacio de trabajo.

  Listar espacio de trabajo:
  ```golang
  // Llamar al método ListWorkspaces del asistente
  list, listErr := assistant.ListWorkspaces(assistantv1.NewListWorkspacesOptions())

  // Comprobar si la llamada es correcta
  if listErr != nil {
    fmt.Println(listErr)
    return
  }

  // Indicar a list.Result el tipo de datos específico devuelto por ListWorkspaces
  // NOTA: la mayoría de los métodos tienen una función Get<methodName>Result() correspondiente
  listResult := assistantv1.GetListWorkspacesResult(list)

  // Comprobar si la indicación es correcta
  if listResult != nil {
    prettyPrint(listResult, "List Workspaces")
  }
  ```
  {: codeblock}

  Obtener espacio de trabajo:
  ```golang
  // Llamar al método GetWorkspace del asistente
  getWorkspaceOptions := assistantv1.NewGetWorkspaceOptions(listResult.Workspaces[0].WorkspaceID)
  get, getErr := assistant.GetWorkspace(getWorkspaceOptions)

  // Comprobar si la llamada es correcta
  if getErr != nil {
    fmt.Println(getErr)
    return
  }

  // Indicar el resultado
  getResult := assistantv1.GetGetWorkspaceResult(get)

  // Comprobar si la indicación es correcta
  if getResult != nil {
    prettyPrint(getResult, "Get Workspace")
    }
  }
  ```
  {: codeblock}

## Adición de un chatbot utilizando kits de inicio
{: #conversation_starterkits}

Con los Kits de inicio, puede utilizar de forma rápida y sencilla las funciones nativas de {{site.data.keyword.cloud_notm}}. Puede añadir {{site.data.keyword.conversationshort}} a cualquier programa de fondo de lado del servidor utilizando un Kit de inicio. El Kit de inicio Chatbot for iOS with Watson ilustra cómo utilizar las funciones de aprendizaje profundo de {{site.data.keyword.conversationshort}} añadiendo una interfaz de lenguaje natural a la aplicación que automatiza las interacciones con los usuarios.

1. Seleccione el [Kit de inicio](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con el que desee trabajar.
2. Cree el proyecto con los servicios predeterminados.
3. Pulse **Añadir recursos > Watson > {{site.data.keyword.conversationshort}}**.
4. Descargue el proyecto pulsando **Descargar código**. Puede encontrar las credenciales de servicio en el archivo `config/local-dev.json`.

## Siguientes pasos
{: #assistant_next}

¡Buen trabajo! Ha añadido un asistente de IA a la app. Mantenga el ritmo probando una de las opciones siguientes:
* Consulte el [SDK de Go de {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Aproveche todas las características que [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) ofrece.
