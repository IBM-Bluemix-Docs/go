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

# Visual Recognition hinzufügen
{: #assistant-vr}

Mit dem {{site.data.keyword.visualrecognitionshort}}-Service können Sie Go-Anwendungen erstellen, die Eingaben in natürlicher Sprache verstehen und mit einer menschenähnlichen Unterhaltung auf Benutzer reagieren.

Funktionsweise der Integration:

* Benutzer interagieren mit der Front-End-Benutzerschnittstelle Ihrer App.
* Ihre App sendet Benutzereingaben unter Verwendung des {{site.data.keyword.watson}} Go-SDKs an den {{site.data.keyword.visualrecognitionshort}}.
* Das {{site.data.keyword.watson}} Go-SDK stellt eine Verbindung zu einem Arbeitsbereich her, der ein Container für Ihren Dialogablauf und die Trainingsdaten ist.
* Der Arbeitsbereich interpretiert die Benutzereingaben und steuert den Ablauf des Dialogs. Dabei wird eine Antwort an Ihre App gesendet.
* Ihre App zeigt die Antwort für den Benutzer an.

## Vorbereitende Schritte
{: #prereqs-vr}

Installieren Sie das {{site.data.keyword.watson}} Go-SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## Visual Recognition zu Ihrer App hinzufügen
{: #add-vr}

1. Öffnen Sie nach dem Herunterladen des Codes Ihr Projekt. 
2. Fügen Sie eine Importanweisung für {{site.data.keyword.conversationshort}} hinzu.
3. Erstellen Sie eine Instanz des Service. Das Beispiel verwendet einen Service namens `visualRecognition`.
4. Das nachfolgende Beispiel zeigt, wie Sie mit dem Service interagieren können. Das Beispiel schließt die Methoden 'GET' und 'LIST' für einen Arbeitsbereich ein. 

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
  // Instanz von Watson Visual Recognition erstellen
  visualRecognition, visualRecognitionErr := NewVisualRecognitionV3(&ServiceCredentials{
    ServiceURL: "YOUR SERVICE URL",
    Version: "2018-03-19",
    APIkey: "YOUR SERVICE API KEY",
  })

  // Erfolgreiche Instanzerstellung prüfen
  if visualRecognitionErr != nil {
    fmt.Println(visualRecognitionErr)
    return
  }

  /* CLASSIFY */
  // Datei mit zu klassifizierendem Bild öffnen
  pwd, _ := os.Getwd()
  imageFile, imageFileErr := os.Open(pwd + "/resources/kitty.jpg")

  // Erfolgreiches Lesen der Datei prüfen
  if imageFileErr != nil {
    fmt.Println(imageFileErr)
    return
  }

  classifyOptions := NewClassifyOptions().
    SetImagesFile(*imageFile).
    SetURL("https://www.readersdigest.ca/wp-content/uploads/2011/01/4-ways-cheer-up-depressed-cat.jpg").
    SetThreshold(0.6).
    SetClassifierIds([]string{ "default", "food", "explicit" })

  // Visual Recognition-Methode 'Classify' aufrufen
  classify, classifyErr := visualRecognition.Classify(classifyOptions)

  // Erfolgreichen Aufruf prüfen
  if classifyErr != nil {
    fmt.Println(classifyErr)
    return
  }

  // Ergebnis von 'classify.Result' in den von 'ListWorkspaces' zurückgegebenen spezifischen Datentyp umsetzen
  // HINWEIS: Die meisten Methoden besitzen eine entsprechende Funktion Get<methodName>Result()
  classifyResult := GetClassifyResult(classify)

  // Erfolgreiche Umsetzung prüfen
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

  // Klassifikationsmerkmal testen
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

## Starter-Kits verwenden
{: #starterkits-vr}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von {{site.data.keyword.cloud_notm}} rasch und ohne großen Aufwand nutzen. Mit Starter-Kits können Sie {{site.data.keyword.visualrecognitionshort}} zu jedem beliebigen serverseitigen Back-End-Programm hinzufügen. Das Starter-Kit 'Chatbot for iOS with Watson' veranschaulicht, wie Sie die Deep-Learning-Funktionalität von {{site.data.keyword.visualrecognitionshort}} nutzen können, indem Sie zu Ihrer Anwendung eine Schnittstelle für natürliche Sprache hinzufügen, die die Interaktionen mit den Benutzern automatisiert.

1. Wählen Sie das [Starter-Kit](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} aus, mit dem Sie arbeiten möchten.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Klicken Sie auf **Ressourcen hinzufügen > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Laden Sie das Projekt herunter, indem Sie auf **Code herunterladen** klicken. Die entsprechenden Serviceberechtigungsnachweise befinden sich in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #next-vr}

Hervorragende Arbeit! Sie haben Visual Recognition zu Ihrer App hinzugefügt. Erhalten Sie die Dynamik aufrecht, indem Sie eine der folgenden Optionen ausprobieren:
* Informieren Sie sich über das [Go-SDK für {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Nutzen Sie alle Features, die [{{site.data.keyword.visualrecognitionshort}}](/docs/services/vr/index.html) zu bieten hat.
