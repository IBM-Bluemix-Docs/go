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

# Aggiunta di Text to Speech 
{: #assistant}

Puoi utilizzare il servizio {{site.data.keyword.text_to_speech}} per creare applicazioni Go che comprendono l'input del testo in linguaggio naturale e lo convertono in audio. 

Come funziona l'integrazione:

* Gli utenti interagiscono con l'interfaccia utente di frontend della tua applicazione.
* La tua applicazione invia l'input utente a {{site.data.keyword.text_to_speech}} utilizzando l'SDK Go {{site.data.keyword.watson}}.
* L'SDK Go {{site.data.keyword.watson}} stabilisce una connessione a uno spazio di lavoro, che è un contenitore per il tuo flusso di dialogo e i tuoi dati di training.
* Lo spazio di lavoro interpreta l'input utente e invia una risposta audio alla tua applicazione. 
* La tua applicazione visualizza la risposta per l'utente. 

## Prima di cominciare
{: #before-you-begin}

Installa l'SDK Go {{site.data.keyword.watson}}:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## Aggiunta di Text to Speech alla tua applicazione 
{: #add-a-text-to-speech-to-your-app}

1. Dopo aver scaricato il tuo codice, apri il tuo progetto.  
2. Aggiungi un'istruzione di importazione per {{site.data.keyword.text_to_speech}}
3. Istanzia il servizio. L'esempio ne utilizza uno denominato `textToSpeech`. 
4. Il seguente esempio mostra come puoi interagire con l'SDK per sintetizzare un servizio Text To Speech.

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

## Utilizzo dei kit starter
{: #text_to_speech_starterkits}

Con i kit starter, puoi velocemente e facilmente utilizzare le funzionalità di {{site.data.keyword.cloud_notm}}. Puoi aggiungere {{site.data.keyword.text_to_speech}} a qualsiasi backend lato server utilizzando i kit starter. Il kit starter Watson Chatbot for iOS illustra come utilizzare le funzionalità di apprendimento approfondito di {{site.data.keyword.text_to_speech}}, aggiungendo un'interfaccia di linguaggio naturale alla tua applicazione che automatizza le interazioni con i tuoi utenti finali.

1. Seleziona il [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con cui vuoi lavorare. 
2. Crea il progetto con i servizi predefiniti.
3. Fai clic su **Add Resources > Watson > {{site.data.keyword.text_to_speech}}**.
4. Scarica il progetto facendo clic su **Download Code**. Puoi trovare le credenziali del servizio nel file `config/local-dev.json`.

## Passi successivi
{: #assistant_next}

Ottimo lavoro. Hai aggiunto Text To Speech alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:
* Consulta l'[SDK Go {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Avvaliti di tutte le funzioni che [{{site.data.keyword.text_to_speech}}](/docs/services/text_to_speech/index.html) ha da offrire.
