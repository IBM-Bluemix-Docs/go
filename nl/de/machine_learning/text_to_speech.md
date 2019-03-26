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

# Text to Speech hinzufügen 
{: #assistant-text}

Mit dem {{site.data.keyword.texttospeechfull}}-Service können Sie Go-Anwendungen erstellen, die Texteingaben in natürlicher Sprache verstehen und diese in Audiodateien umwandeln. 

Funktionsweise der Integration:

* Benutzer interagieren mit der Front-End-Benutzerschnittstelle Ihrer App.
* Ihre App sendet Benutzereingaben unter Verwendung des {{site.data.keyword.watson}} Go-SDKs an den {{site.data.keyword.texttospeechshort}}.
* Das {{site.data.keyword.watson}} Go-SDK stellt eine Verbindung zu einem Arbeitsbereich her, der ein Container für Ihren Dialogablauf und die Trainingsdaten ist.
* Der Arbeitsbereich interpretiert die Benutzereingaben und sendet eine Audioantwort an Ihre App.
* Ihre App zeigt die Antwort für den Benutzer an.

## Vorbereitende Schritte
{: #prereqs-text}

Installieren Sie das {{site.data.keyword.watson}} Go-SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## Text to Speech zu Ihrer App hinzufügen
{: #existing-text}

1. Öffnen Sie nach dem Herunterladen des Codes Ihr Projekt. 
2. Fügen Sie eine Importanweisung für {{site.data.keyword.texttospeechshort}} hinzu.
3. Erstellen Sie eine Instanz des Service. Das Beispiel verwendet einen Service namens `textToSpeech`. 
4. Das folgende Beispiel zeigt, wie Sie mit dem SDK interagieren können, um einen Text-Sprache-Service synthetisch zu erstellen.

```golang
package main

import (
  "fmt"
  . "go-sdk/textToSpeechV1"
  "bytes"
  "os"
)

func main() {
  // Instanz des Watson-Service Text To Speech erstellen
  textToSpeech, textToSpeechErr := NewTextToSpeechV1(&ServiceCredentials{
    ServiceURL: "YOUR SERVICE URL",
    Version: "2017-09-21",
    Username: "YOUR SERVICE USERNAME",
    Password: "YOUR SERVICE PASSWORD",
  })

  // Erfolgreiche Instanzerstellung prüfen
  if textToSpeechErr != nil {
    fmt.Println(textToSpeechErr)
    return
  }

  /* SYNTHESIZE */
  synthesizeOptions := NewSynthesizeOptions("Hello World").
    SetAccept("audio/mp3").
    SetVoice("en-GB_KateVoice")

  // textToSpeech-Methode 'Synthesize' aufrufen
  synthesize, synthesizeErr := textToSpeech.Synthesize(synthesizeOptions)

  // Erfolgreichen Aufruf prüfen
  if synthesizeErr != nil {
    fmt.Println(synthesizeErr)
    return
  }

  // Ergebnis von 'synthesize.Result' in den von 'Synthesize' zurückgegebenen spezifischen Datentyp umsetzen
  // HINWEIS: Die meisten Methoden besitzen eine entsprechende Funktion Get<methodName>Result()
  synthesizeResult := GetSynthesizeResult(synthesize)

  // Erfolgreiche Umsetzung prüfen
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

## Starter-Kits verwenden
{: #starterkits-text}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von {{site.data.keyword.cloud_notm}} rasch und ohne großen Aufwand ausschöpfen. Mit Starter-Kits können Sie {{site.data.keyword.texttospeechshort}} zu jedem beliebigen serverseitigen Back-End-Programm hinzufügen. Das Starter-Kit 'Chatbot for iOS with Watson' veranschaulicht, wie Sie die Deep-Learning-Funktionalität von {{site.data.keyword.texttospeechshort}} nutzen können, indem Sie zu Ihrer Anwendung eine Schnittstelle für natürliche Sprache hinzufügen, die die Interaktionen mit den Benutzern automatisiert.

1. Wählen Sie das [Starter-Kit](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} aus, mit dem Sie arbeiten möchten.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Klicken Sie auf **Ressourcen hinzufügen > Watson > {{site.data.keyword.texttospeechshort}}**.
4. Laden Sie das Projekt herunter, indem Sie auf **Code herunterladen** klicken. Die entsprechenden Serviceberechtigungsnachweise befinden sich in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #next-text}

Hervorragende Arbeit! Sie haben Text To Speech zu Ihrer App hinzugefügt. Erhalten Sie die Dynamik aufrecht, indem Sie eine der folgenden Optionen ausprobieren:
* Informieren Sie sich über das [Go-SDK für {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Nutzen Sie alle Features, die [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html) bietet.
