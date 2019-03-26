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

# 新增聊天機器人
{: #assistant-chatbot}

您可以使用 {{site.data.keyword.conversationshort}} 服務來建置可瞭解自然語言輸入，並以擬人交談回應使用者的 Go 應用程式。

整合運作方式：

* 使用者會與應用程式的前端使用者介面互動。
* 您的應用程式會透過 {{site.data.keyword.ibmwatson}} Go SDK 將使用者輸入傳送至 {{site.data.keyword.conversationshort}}。
* {{site.data.keyword.watson}} Go SDK 會連接至一個工作區，該工作區是對話流程及訓練資料的容器。
* 此工作區解譯使用者輸入並指揮對話流程，然後傳送回應至您的應用程式。
* 您的應用程式前端會以可播放的 `mp3` 或可下載的檔案形式，與使用者共用音訊回應。

您可以將虛擬助理新增至新的 Go 入門範本套件應用程式或現有的 Go 應用程式。

## 開始之前
{: #prereqs-chatbot}

安裝 {{site.data.keyword.watson}} Go SDK：
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## 將虛擬助理新增至現有的 Go 應用程式
{: #existing-chatbot}

1. 在下載程式碼之後，開啟您的專案。

2. 為 {{site.data.keyword.conversationshort}} 新增 import 陳述式。

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

3. 將服務實例化。下列範例使用一個稱為 `assistant` 的服務。

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

4. 使用工作區的 'GET' 及 'LIST' 方法與服務互動。

  列出工作區：
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

  取得工作區：
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

## 使用入門範本套件新增聊天機器人
{: #starterkits-chatbot}

運用「入門範本套件」，您可以快速且輕鬆地使用 {{site.data.keyword.cloud_notm}} 的原生功能。您可以使用「入門範本套件」，將 {{site.data.keyword.conversationshort}} 新增至任何伺服器端後端。Chatbot for iOS with Watson 入門範本套件說明如何在自動化使用者互動的應用程式中新增自然語言介面，藉以使用 {{site.data.keyword.conversationshort}} 的深入學習功能。

1. 選取您要使用的[入門範本套件](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}。
2. 建立含有預設服務的專案。
3. 按一下**新增資源 > Watson > {{site.data.keyword.conversationshort}}**。
4. 按一下**下載程式碼**，以下載專案。您可以在 `config/local-dev.json` 檔案中找到服務認證。

## 後續步驟
{: #next-chatbot}

做得好！您已將 AI 助理新增至您的應用程式。嘗試下列其中一個選項，以保持動力：
* 查看 [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}。
* 充分運用 [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) 提供的所有特性。
