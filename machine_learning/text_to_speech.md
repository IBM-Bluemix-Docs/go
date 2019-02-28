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

# Adding Text to Speech 
{: #assistant-text}

You can use the {{site.data.keyword.texttospeechfull}} service to build Go applications that understand natural-language text input, and convert it into audio. 

How the integration works:

* Users interact with the front-end user interface of your app.
* Your app sends user input to the {{site.data.keyword.texttospeechshort}} by using the {{site.data.keyword.watson}} Go SDK.
* The {{site.data.keyword.watson}} Go SDK connects to a workspace, which is a container for your dialog flow and training data.
* The workspace interprets the user input and sends an audio response to your app.
* Your app displays the response for the user.

## Before you begin
{: #prereqs-text}

Install the {{site.data.keyword.watson}} Go SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## Adding Text to Speech to your app
{: #existing-text}

1. After you download your code, open your project. 
2. Add an import statement for {{site.data.keyword.texttospeechshort}}
3. Instantiate the service. The example uses one called `textToSpeech`. 
4. The following example shows how you can interact with the SDK to synthesize a Text To Speech service.

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

## Using starter kits
{: #starterkits-text}

With starter kits, you can quickly and easily use the capabilities of {{site.data.keyword.cloud_notm}}. You can add {{site.data.keyword.texttospeechshort}} to any server-side back end by using the starter kits. The Chatbot for iOS with Watson starter kit illustrates how to use the deep learning capabilities of {{site.data.keyword.texttospeechshort}}, by adding a natural language interface to your application that automates interactions with your end users.

1. Select the [starter kit](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} with which you want to work.
2. Create the project with the default services.
3. Click **Add Resources > Watson > {{site.data.keyword.texttospeechshort}}**.
4. Download the project by clicking **Download Code**. You can find the service credentials in the `config/local-dev.json` file.

## Next steps
{: #next-text}

Great job! You added Text To Speech to your app. Keep the momentum by trying one of the following options:
* Check out the [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Take advantage of all of the features that [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html) offers.