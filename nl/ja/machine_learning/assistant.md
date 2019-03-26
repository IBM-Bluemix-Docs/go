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

# チャットボットの追加
{: #assistant-chatbot}

{{site.data.keyword.conversationshort}} サービスを使用して、自然言語による入力を理解して人間と会話しているかのようにユーザーに応答する Go アプリケーションを構築することができます。

統合の仕組み:

* ユーザーは、アプリのフロントエンド・ユーザー・インターフェースと対話します。
* アプリは、ユーザーの入力内容を {{site.data.keyword.ibmwatson}} Go SDK を使用して {{site.data.keyword.conversationshort}} に送信します。
* {{site.data.keyword.watson}} Go SDK はワークスペースに接続します。ワークスペースはダイアログ・フローとトレーニング・データのコンテナーです。
* ワークスペースはユーザー入力を解釈し、対話のフローを指図し、応答をアプリに送信します。
* アプリ・フロントエンドは、再生可能な `mp3` またはダウンロード可能ファイルとして、音声応答をユーザーと共有します。

新しい Go スターター・キット・アプリまたは既存の Go アプリに、仮想アシスタントを追加できます。

## 始める前に
{: #prereqs-chatbot}

以下のようにして {{site.data.keyword.watson}} Go SDK をインストールします。
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## 既存の Go アプリへの仮想アシスタントの追加
{: #existing-chatbot}

1. コードをダウンロードしたら、プロジェクトを開きます。

2. {{site.data.keyword.conversationshort}} の import ステートメントを追加します。

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

3. サービスをインスタンス化します。 以下の例では、`assistant` というサービスを使用します。

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

4. ワークスペースに対して「GET」メソッドおよび「LIST」メソッドを使用して、サービスと対話します。

  以下のようにしてワークスペースをリストします。
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

  以下のようにしてワークスペースを取得します。
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

## スターター・キットを使用したチャットボットの追加
{: #starterkits-chatbot}

スターター・キットを使用すると、{{site.data.keyword.cloud_notm}} のネイティブ機能を素早く簡単に利用できます。 スターター・キットを使用して、{{site.data.keyword.conversationshort}} を任意のサーバー・サイドのバックエンドに追加できます。 Chatbot for iOS with Watson Starter Kit では、ユーザーとの対話を自動化するアプリケーションに自然言語インターフェースを追加することによって {{site.data.keyword.conversationshort}} のディープ・ラーニング機能を使用する方法について説明しています。

1. 使用する[スターター・キット](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}を選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「リソースの追加」>「Watson」>「{{site.data.keyword.conversationshort}}」**をクリックします。
4. **「コードのダウンロード」**をクリックして、プロジェクトをダウンロードします。 サービス資格情報は、`config/local-dev.json` ファイルにあります。

## 次のステップ
{: #next-chatbot}

お疲れさまでした。AI アシスタントがアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。
* [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window} を使ってみる。
* [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) が提供するすべての機能を利用する。
