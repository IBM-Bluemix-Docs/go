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

# Incluindo texto para fala 
{: #assistant}

É possível usar o serviço {{site.data.keyword.text_to_speech}} para construir aplicativos Go que entendem a entrada de texto de língua natural e a convertem em áudio. 

Como a integração funciona:

* Os usuários interagem com a interface com o usuário de front-end de seu app.
* Seu app envia a entrada do usuário para o {{site.data.keyword.text_to_speech}} usando o {{site.data.keyword.watson}} Go SDK.
* O {{site.data.keyword.watson}} Go SDK se conecta a uma área de trabalho, que é um contêiner para o seu fluxo de diálogo e os seus dados de treinamento.
* A área de trabalho interpreta a entrada do usuário e envia uma resposta de áudio para seu app.
* Seu app exibe a resposta para o usuário.

## Antes de começar
{: #before-you-begin}

Instale o {{site.data.keyword.watson}} Go SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## Incluindo texto para fala em seu app
{: #add-a-text-to-speech-to-your-app}

1. Depois de fazer download do código, abra seu projeto. 
2. Inclua uma instrução de importação para {{site.data.keyword.text_to_speech}}
3. Instancie o serviço. O exemplo usa um chamado `textToSpeech`. 
4. O exemplo a seguir mostra como é possível interagir com o SDK para sintetizar um serviço de Texto para Fala.

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

## Usando kits iniciadores
{: #text_to_speech_starterkits}

Com kits do iniciador, é possível usar os recursos do {{site.data.keyword.cloud_notm}} de maneira rápida e fácil. É possível incluir o {{site.data.keyword.text_to_speech}} em qualquer back-end do lado do servidor usando os kits do iniciador. O kit do iniciador do Chatbot for iOS with Watson ilustra como usar os recursos de deep learning do {{site.data.keyword.text_to_speech}}, incluindo uma interface de língua natural em seu aplicativo que automatiza interações com seus usuários finais.

1. Selecione o [kit do iniciador](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} com o qual você deseja trabalhar.
2. Crie o projeto com os serviços padrão.
3. Clique em **Incluir recursos > Watson > {{site.data.keyword.text_to_speech}}**.
4. Faça download do projeto clicando em **Fazer download do código**. É possível localizar as credenciais de serviço no arquivo `config/local-dev.json`.

## Próximas Etapas
{: #assistant_next}

Ótimo trabalho! Você incluiu Texto para Fala em seu app. Tente uma das opções a seguir para manter o ritmo:
* Confira o [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Tire proveito de todos os recursos que o [{{site.data.keyword.text_to_speech}}](/docs/services/text_to_speech/index.html) oferece.
