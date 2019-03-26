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

# 添加聊天机器人
{: #assistant-chatbot}

可以使用 {{site.data.keyword.conversationshort}} 服务来构建可理解自然语言输入并通过类似真人的对话来响应用户的 Go 应用程序。

集成工作方式：

* 用户与应用程序的前端用户界面进行交互。
* 应用程序使用 {{site.data.keyword.ibmwatson}} Go SDK 将用户输入发送到 {{site.data.keyword.conversationshort}}。
* {{site.data.keyword.watson}} Go SDK 连接到工作空间，此工作空间是用于对话流和训练数据的容器。
* 工作空间解读用户输入并定向对话流，以向应用程序发送响应。
* 应用程序前端以可播放 `mp3` 或可下载文件的形式与用户共享音频响应。

您可以向新的 Go 入门模板工具包应用程序或者现有 Go 应用程序添加虚拟助手。

## 开始之前
{: #prereqs-chatbot}

安装 {{site.data.keyword.watson}} Go SDK：
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## 向现有 Go 应用程序添加虚拟助手
{: #existing-chatbot}

1. 下载代码后，打开项目。

2. 添加用于 {{site.data.keyword.conversationshort}} 的 import 语句。

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

3. 实例化服务。以下示例使用名为 `assistant` 的服务。

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

4. 使用工作空间的“GET”和“LIST”方法与服务交互。

  列出工作空间：
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

  获取工作空间：
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

## 使用入门模板工具包添加聊天机器人
{: #starterkits-chatbot}

通过入门模板工具包，可以快速、轻松地使用 {{site.data.keyword.cloud_notm}} 的本机功能。可以使用入门模板工具包将 {{site.data.keyword.conversationshort}} 添加到任何服务器端后端。Chatbot for iOS with Watson 入门模板工具包说明了如何通过将自然语言界面添加到自动与用户进行交互的应用程序，从而使用 {{site.data.keyword.conversationshort}} 的深度学习功能。

1. 选择要使用的[入门模板工具包](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}。
2. 使用缺省服务创建项目。
3. 单击**添加资源 > Watson > {{site.data.keyword.conversationshort}}**。
4. 通过单击**下载代码**来下载项目。可以在 `config/local-dev.json` 文件中找到服务凭证。

## 后续步骤
{: #next-chatbot}

太棒了！您已将 AI 助手添加到应用程序。请一鼓作气尝试下列其中一个选项：
* 请查看 [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}。
* 利用 [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) 提供的所有功能。
