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

# 新增 Text to Speech 
{: #assistant-text}

您可以使用 {{site.data.keyword.texttospeechfull}} 服務來建置可瞭解自然語言文字輸入，並將其轉換成音訊的 Go 應用程式。 

整合運作方式：

* 使用者會與應用程式的前端使用者介面互動。
* 您的應用程式會透過 {{site.data.keyword.watson}} Go SDK 將使用者輸入傳送至 {{site.data.keyword.texttospeechshort}}。
* {{site.data.keyword.watson}} Go SDK 會連接至一個工作區，該工作區是對話流程及訓練資料的容器。
* 此工作區會解譯使用者輸入，並將音訊回應傳送至您的應用程式。
* 您的應用程式顯示對使用者的回應。

## 開始之前
{: #prereqs-text}

安裝 {{site.data.keyword.watson}} Go SDK：
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## 將文字轉語音新增至應用程式
{: #existing-text}

1. 在下載程式碼之後，開啟您的專案。 
2. 為 {{site.data.keyword.texttospeechshort}} 新增 import 陳述式。
3. 將服務實例化。此範例使用一個稱為 `textToSpeech` 的服務。 
4. 下列範例顯示您可以與 SDK 互動來整合 Text To Speech 服務的方式。

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

## 使用入門範本套件
{: #starterkits-text}

運用入門範本套件，您可以快速且輕鬆地使用 {{site.data.keyword.cloud_notm}} 的功能。您可以使用入門範本套件，將 {{site.data.keyword.texttospeechshort}} 新增至任何伺服器端後端。Chatbot for iOS with Watson 入門範本套件說明如何在自動化一般使用者互動的應用程式中新增自然語言介面，藉以使用 {{site.data.keyword.texttospeechshort}} 的深入學習功能。

1. 選取您要使用的[入門範本套件](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}。
2. 建立含有預設服務的專案。
3. 按一下**新增資源 > Watson > {{site.data.keyword.texttospeechshort}}**。
4. 按一下**下載程式碼**，以下載專案。您可以在 `config/local-dev.json` 檔案中找到服務認證。

## 後續步驟
{: #next-text}

做得好！您已將 Text to Speech 新增至您的應用程式。嘗試下列其中一個選項，以保持動力：
* 查看 [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}。
* 充分運用 [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html) 提供的所有特性。
