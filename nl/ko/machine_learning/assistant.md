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

# 챗봇 추가
{: #assistant-chatbot}

{{site.data.keyword.conversationshort}} 서비스를 사용하여 자연어 입력을 이해하고 인간과 유사한 대화로 사용자에게 응답하는 Go 애플리케이션을 빌드할 수 있습니다.

통합이 작동하는 방식:

* 사용자가 앱의 프론트 엔드 사용자 인터페이스와 상호작용합니다.
* 앱이 {{site.data.keyword.ibmwatson}} Go SDK를 사용하여 {{site.data.keyword.conversationshort}}에 사용자 입력을 보냅니다.
* {{site.data.keyword.watson}} Go SDK는 대화 상자 플로우 및 훈련 데이터를 위한 컨테이너인 작업공간에 연결됩니다.
* 작업공간에서는 사용자 입력을 해석하고 대화의 플로우를 지시하며 응답을 앱에 보냅니다.
* 앱 프론트 엔드는 사용자와 오디오 응답을 재생 가능한 `mp3` 또는 다운로드 가능한 파일로 공유합니다.

기존 Go 앱 또는 새 Go 스타터 킷 앱에 가상 지원 프로그램을 추가할 수 있습니다.

## 시작하기 전에
{: #prereqs-chatbot}

{{site.data.keyword.watson}} Go SDK를 설치하십시오.
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## 기존 Go 앱에 가상 지원 프로그램 추가
{: #existing-chatbot}

1. 코드를 다운로드한 후 프로젝트를 여십시오.

2. {{site.data.keyword.conversationshort}}에 대한 import 문을 추가하십시오.

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

3. 서비스를 인스턴스화하십시오. 다음 예에서는 `assistant`를 사용합니다.

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

4. 작업공간에 'GET' 및 'LIST' 메소드를 사용하여 서비스와 상호작용하십시오.

  작업공간 나열:
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

  작업공간 가져오기:
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

## 스타터 킷을 사용하여 챗봇 추가
{: #starterkits-chatbot}

스타터 킷을 사용하면 {{site.data.keyword.cloud_notm}}의 기본 기능을 빠르고 쉽게 사용할 수 있습니다. 스타터 킷을 사용하여 서버 측 백엔드에 {{site.data.keyword.conversationshort}}을 추가할 수 있습니다. Watson 스타터 킷을 사용한 iOS용 챗봇은 사용자와의 상호작용을 자동화하는 애플리케이션에 자연어 인터페이스를 추가하여 {{site.data.keyword.conversationshort}}의 딥러닝 기능을 사용하는 방법을 보여줍니다.

1. 작업할 [스타터 킷](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}을 선택하십시오.
2. 기본 서비스로 프로젝트를 작성하십시오.
3. **리소스 추가 > Watson > {{site.data.keyword.conversationshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. `config/local-dev.json` 파일에서 서비스 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #next-chatbot}

잘하셨습니다. 앱에 AI 지원 프로그램을 추가했습니다. 다음 옵션 중 하나를 시도하여 계속하십시오.
* [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}를 확인하십시오.
* [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html)에서 제공하는 모든 기능을 활용하십시오.
