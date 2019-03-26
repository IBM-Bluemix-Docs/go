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

# Ajout de la reconnaissance visuelle
{: #assistant-vr}

Vous pouvez utiliser le service {{site.data.keyword.visualrecognitionshort}} pour créer des applications Go qui comprennent les entrées en langage naturel et répondent aux utilisateurs par une conversation humaine.

L'intégration fonctionne de la manière suivante :

* Les utilisateurs interagissent avec l'interface utilisateur frontale de votre application.
* Votre application envoie une entrée utilisateur à {{site.data.keyword.visualrecognitionshort}} à l'aide du logiciel SDK Go {{site.data.keyword.watson}}.
* Le logiciel SDK Go {{site.data.keyword.watson}} se connecte à un espace de travail, qui est un conteneur pour votre flux de dialogues et vos données d'apprentissage.
* L'espace de travail interprète l'entrée utilisateur et dirige le flux des conversations, en envoyant une réponse à votre application.
* Votre application affiche la réponse à l'utilisateur.

## Avant de commencer
{: #prereqs-vr}

Installez le logiciel SDK Go {{site.data.keyword.watson}} :
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## Ajout d'une reconnaissance visuelle à votre application
{: #add-vr}

1. Après avoir téléchargé vote code, ouvrez votre projet. 
2. Ajoutez une instruction d'importation pour {{site.data.keyword.conversationshort}}
3. Instanciez le service. L'exemple utilise `visualRecognition`.
4. Vous trouverez ci-dessous un exemple de la manière dont vous pouvez interagir avec le service. Cet exemple inclut 'GET' et 'LIST' d'un espace de travail. 

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

## Utilisation des kits de démarrage
{: #starterkits-vr}

Les kits de démarrage vous permettent de tirer profit des fonctionnalités d'{{site.data.keyword.cloud_notm}} de manière rapide et facile. Vous pouvez ajouter {{site.data.keyword.visualrecognitionshort}} à un système de back-end côté serveur avec les kits de démarrage. Le kit de démarrage Chatbot for iOS with Watson illustre comment utiliser les fonctions d'apprentissage en profondeur de {{site.data.keyword.visualrecognitionshort}} par l'ajout d'une interface en langage naturel à votre application qui automatise les interactions avec vos utilisateurs finaux.

1. Sélectionnez le [kit de démarrage](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} avec lequel vous souhaitez travailler.
2. Créez le projet avec les services par défaut.
3. Cliquez sur **Ajouter des ressources > Watson > {{site.data.keyword.visualrecognitionshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Vous trouverez les données d'identification du service dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #next-vr}

Félicitations ! Vous avez ajouté la reconnaissance visuelle à votre application. Continuez sur votre lancée en essayant l'une des options suivantes :
* Consultez le [logiciel SDK Go {{site.data.keyword.watson}} ](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Tirez parti de toutes les fonctionnalités offertes par [{{site.data.keyword.visualrecognitionshort}}](/docs/services/vr/index.html).
