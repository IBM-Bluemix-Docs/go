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

# Aggiunta di Visual Recognition
{: #assistant}

Puoi utilizzare il servizio {{site.data.keyword.vr}} per creare applicazioni Go che comprendono l'input in linguaggio naturale e rispondono agli utenti con una conversazione simile a quella umana.

Come funziona l'integrazione:

* Gli utenti interagiscono con l'interfaccia utente di frontend della tua applicazione.
* La tua applicazione invia l'input utente a {{site.data.keyword.vr}} utilizzando l'SDK Go {{site.data.keyword.watson}}.
* L'SDK Go {{site.data.keyword.watson}} stabilisce una connessione a uno spazio di lavoro, che è un contenitore per il tuo flusso di dialogo e i tuoi dati di training.
* Lo spazio di lavoro interpreta l'input utente e indirizza il flusso della conversazione, inviando una risposta alla tua applicazione.
* La tua applicazione visualizza la risposta per l'utente. 

## Prima di cominciare
{: #before-you-begin}

Installa l'SDK Go {{site.data.keyword.watson}}:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## Aggiunta di un Visual Recognition alla tua applicazione 
{: #add-a-virtual-assistant-to-your-app}

1. Dopo aver scaricato il tuo codice, apri il tuo progetto.  
2. Aggiungi un'istruzione di importazione per {{site.data.keyword.conversationshort}}
3. Istanzia il servizio. L'esempio ne utilizza uno denominato `visualRecognition`.
4. Il seguente è un esempio di come puoi interagire con il servizio. Questo esempio include i metodi 'GET' e 'LIST' di uno spazio di lavoro. 

```golang
package main

import (
  "os"
  "fmt"
  . "go-sdk/visualRecognitionV3"
  "encoding/json"
)

func prettyPrint(result interface{}, resultName string) {
  output, err := json.MarshalIndent(result, "", "    ")

  if err == nil {
    fmt.Printf("%v:\n%+v\n\n", resultName, string(output))
    }
}

func main() {
  // Instantiate the Watson Visual Recognition service
  visualRecognition, visualRecognitionErr := NewVisualRecognitionV3(&ServiceCredentials{
    ServiceURL: "YOUR SERVICE URL",
    Version: "2018-03-19",
    APIkey: "YOUR SERVICE API KEY",
  })

  // Check successful instantiation
  if visualRecognitionErr != nil {
    fmt.Println(visualRecognitionErr)
    return
  }

  /* CLASSIFY */
  // Open file with image to classify
  pwd, _ := os.Getwd()
  imageFile, imageFileErr := os.Open(pwd + "/resources/kitty.jpg")

  // Check successful file read
  if imageFileErr != nil {
    fmt.Println(imageFileErr)
    return
  }

  classifyOptions := NewClassifyOptions().
    SetImagesFile(*imageFile).
    SetURL("https://www.readersdigest.ca/wp-content/uploads/2011/01/4-ways-cheer-up-depressed-cat.jpg").
    SetThreshold(0.6).
    SetClassifierIds([]string{ "default", "food", "explicit" })

  // Call the visual recognition Classify method
  classify, classifyErr := visualRecognition.Classify(classifyOptions)

  // Check successful call
  if classifyErr != nil {
    fmt.Println(classifyErr)
    return
  }

  // Cast classify.Result to the specific dataType returned by Classify
  // NOTE: most methods have a corresponding Get<methodName>Result() function
  classifyResult := GetClassifyResult(classify)

  // Check successful casting
  if classifyResult != nil {
    prettyPrint(classifyResult, "Classify")
  }

  /* CREATE CLASSIFIER */
  carsFile, carsFileErr := os.Open(pwd + "/resources/cars.zip")
  if carsFileErr != nil {
    fmt.Println(carsFileErr)
    return
  }

  trucksFile, trucksFileErr := os.Open(pwd + "/resources/trucks.zip")
  if trucksFileErr != nil {
    fmt.Println(trucksFileErr)
    return
  }

  createClassifierOptions := NewCreateClassifierOptions("Cars vs Trucks", "cars", *carsFile).
    SetNegativeExamples(*trucksFile)

  create, createErr := visualRecognition.CreateClassifier(createClassifierOptions)
  if createErr != nil {
    fmt.Println(createErr)
    return
  }

  createResult := GetCreateClassifierResult(create)
  if createResult != nil {
    prettyPrint(createResult, "Create Classifier")
  }

  // Test classifier
  imageFile, imageFileErr = os.Open(pwd + "/resources/car.jpg")
  if imageFileErr != nil {
    fmt.Println(imageFileErr)
    return
  }

  classifyOptions = NewClassifyOptions().
    SetImagesFile(*imageFile)

  classify, classifyErr = visualRecognition.Classify(classifyOptions)
  if classifyErr != nil {
    fmt.Println(classifyErr)
    return
  }

  classifyResult = GetClassifyResult(classify)
  if classifyResult != nil {
    prettyPrint(classifyResult, "Classify")
  }

  /* DETECT FACES */
  imageFile, imageFileErr = os.Open(pwd + "/resources/face.jpg")
  if imageFileErr != nil {
    fmt.Println(imageFileErr)
    return
  }

  detectFacesOptions := NewDetectFacesOptions().
    SetImagesFile(*imageFile).
    SetURL("https://www.ibm.com/ibm/ginni/images/ginni_bio_780x981_v4_03162016.jpg")

  detect, detectErr := visualRecognition.DetectFaces(detectFacesOptions)
  if detectErr != nil {
    fmt.Println(detectErr)
    return
  }

  detectResult := GetDetectFacesResult(detect)
  if detectResult != nil {
    prettyPrint(detectResult, "Detect Faces")
  }
}
```
{: codeblock}

## Utilizzo dei kit starter
{: #vr_starterkits}

Con i kit starter, puoi velocemente e facilmente utilizzare le funzionalità di {{site.data.keyword.cloud_notm}}. Puoi aggiungere {{site.data.keyword.vr}} a qualsiasi backend lato server utilizzando i kit starter. Il kit starter Watson Chatbot for iOS illustra come utilizzare le funzionalità di apprendimento approfondito di {{site.data.keyword.vr}}, aggiungendo un'interfaccia di linguaggio naturale alla tua applicazione che automatizza le interazioni con i tuoi utenti finali.

1. Seleziona il [kit starter](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con cui vuoi lavorare. 
2. Crea il progetto con i servizi predefiniti.
3. Fai clic su **Add Resources > Watson > {{site.data.keyword.vr}}**.
4. Scarica il progetto facendo clic su **Download Code**. Puoi trovare le credenziali del servizio nel file `config/local-dev.json`.

## Passi successivi
{: #assistant_next}

Ottimo lavoro. Hai aggiunto Visual Recognition alla tua applicazione. Non fermarti ora e continua provando una delle seguenti opzioni:
* Consulta l'[SDK Go {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Avvaliti di tutte le funzioni che [{{site.data.keyword.vr}}](/docs/services/vr/index.html) ha da offrire.
