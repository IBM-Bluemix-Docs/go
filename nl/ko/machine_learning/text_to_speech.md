---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Text to Speech 추가 
{: #assistant-text}

{{site.data.keyword.texttospeechfull}} 서비스를 사용하여 자연어 텍스트 입력을 이해하고 오디오로 변환하는 Go 애플리케이션을 빌드할 수 있습니다. 

통합이 작동하는 방식:

* 사용자가 앱의 프론트 엔드 사용자 인터페이스와 상호작용합니다.
* 앱이 {{site.data.keyword.watson}} Go SDK를 사용하여 {{site.data.keyword.texttospeechshort}}에 사용자 입력을 보냅니다.
* {{site.data.keyword.watson}} Go SDK는 대화 상자 플로우 및 훈련 데이터를 위한 컨테이너인 작업공간에 연결됩니다.
* 작업공간에서는 사용자 입력을 해석하고 오디오 응답을 앱에 보냅니다.
* 앱이 사용자의 응답을 표시합니다.

## 시작하기 전에
{: #prereqs-text}

{{site.data.keyword.watson}} Go SDK를 설치하십시오.
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## 앱에 Text to Speech 추가
{: #existing-text}

1. 코드를 다운로드한 후 프로젝트를 여십시오. 
2. {{site.data.keyword.texttospeechshort}}에 대한 import 문을 추가하십시오.
3. 서비스를 인스턴스화하십시오. 예에서는 `textToSpeech`를 사용합니다. 
4. 다음 예에서는 SDK와 상호작용하여 Text To Speech 서비스를 통합하는 방법을 보여줍니다.

```golang
package main

import (
  "fmt"
  . "go-sdk/textToSpeechV1"
  "bytes"
  "os"
)

func main() {
  // Instantiate the Watson Text To Speech service
  textToSpeech, textToSpeechErr := NewTextToSpeechV1(&ServiceCredentials{
    ServiceURL: "YOUR SERVICE URL",
    Version: "2017-09-21",
    Username: "YOUR SERVICE USERNAME",
    Password: "YOUR SERVICE PASSWORD",
  })

  // Check successful instantiation
  if textToSpeechErr != nil {
    fmt.Println(textToSpeechErr)
    return
  }

  /* SYNTHESIZE */
  synthesizeOptions := NewSynthesizeOptions("Hello World").
    SetAccept("audio/mp3").
    SetVoice("en-GB_KateVoice")

  // Call the textToSpeech Synthesize method
  synthesize, synthesizeErr := textToSpeech.Synthesize(synthesizeOptions)

  // Check successful call
  if synthesizeErr != nil {
    fmt.Println(synthesizeErr)
    return
  }

  // Cast synthesize.Result to the specific dataType returned by Synthesize
  // NOTE: most methods have a corresponding Get<methodName>Result() function
  synthesizeResult := GetSynthesizeResult(synthesize)

  // Check successful casting
  if synthesizeResult != nil {
    buff := new(bytes.Buffer)
    buff.ReadFrom(synthesizeResult)

    fileName := "synthesize_example_output.mp3"
    file, _ := os.Create(fileName)
    file.Write(buff.Bytes())
    file.Close()

    fmt.Println("Wrote synthesized text to " + fileName)
  }
}
```
{: codeblock}

## 스타터 킷 사용
{: #starterkits-text}

스타터 킷을 사용하면 {{site.data.keyword.cloud_notm}}의 기능을 빠르고 쉽게 사용할 수 있습니다. 스타터 킷을 사용하여 서버 측 백엔드에 {{site.data.keyword.texttospeechshort}}를 추가할 수 있습니다. Watson 스타터 킷을 사용한 iOS용 챗봇은 일반 사용자와의 상호작용을 자동화하는 애플리케이션에 자연어 인터페이스를 추가하여 {{site.data.keyword.texttospeechshort}}의 딥러닝 기능을 사용하는 방법을 보여줍니다.

1. 작업할 [스타터 킷](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}을 선택하십시오.
2. 기본 서비스로 프로젝트를 작성하십시오.
3. **리소스 추가 > Watson > {{site.data.keyword.texttospeechshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. `config/local-dev.json` 파일에서 서비스 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #next-text}

잘하셨습니다. 앱에 Text To Speech를 추가했습니다. 다음 옵션 중 하나를 시도하여 계속하십시오.
* [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}를 확인하십시오.
* [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html)에서 제공하는 모든 기능을 활용하십시오.
