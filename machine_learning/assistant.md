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

# Adding a chatbot
{: #assistant}

You can use the {{site.data.keyword.conversationshort}} service to build Go applications that understand natural-language input, and respond to users with human-like conversation.

How the integration works:

* Users interact with the front-end user interface of your app.
* Your app sends user input to the {{site.data.keyword.conversationshort}} by using the {{site.data.keyword.ibmwatson}} Go SDK.
* The {{site.data.keyword.watson}} Go SDK connects to a workspace, which is a container for your dialog flow and training data.
* The workspace interprets the user input and directs the flow of the conversation, sending a response to your app.
* Your app front-end shares the audio response with the user as a playable `mp3`, or a downloadable file.

You can add the virtual assistant to a new Go Starter Kit app, or to an existing Go app.

## Before you begin
{: #before-you-begin}

Install the {{site.data.keyword.watson}} Go SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## Adding a virtual assistant to your existing Go app
{: #add-a-virtual-assistant-to-your-app}

1. After you download the code, open your project.

2. Add an import statement for {{site.data.keyword.conversationshort}}.

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

3. Instantiate the service. The following example uses one called `assistant`.

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

4. Interact with the service by using 'GET' and 'LIST' methods for a workspace.

  List workspace:
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

  Get workspace:
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

## Adding a chatbot by using starter kits
{: #conversation_starterkits}

With Starter Kits, you can quickly and easily use the native capabilities of {{site.data.keyword.cloud_notm}}. You can add {{site.data.keyword.conversationshort}} to any server-side back end by using a Starter Kit. The Chatbot for iOS with Watson Starter Kit illustrates how to use the deep learning capabilities of {{site.data.keyword.conversationshort}}, by adding a natural language interface to your application that automates interactions with your users.

1. Select the [Starter Kit](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} with which you want to work.
2. Create the project with the default services.
3. Click **Add Resources > Watson > {{site.data.keyword.conversationshort}}**.
4. Download the project by clicking **Download Code**. You can find the service credentials in the `config/local-dev.json` file.

## Next steps
{: #assistant_next}

Great job! You added an AI assistant to your app. Keep the momentum by trying one of the following options:
* Check out the [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Take advantage of all of the features that [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) offers.