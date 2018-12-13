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

# Incluindo o Reconhecimento Visual
{: #assistant}

É possível usar o serviço do {{site.data.keyword.vr}} para construir aplicativos Go que entendem a entrada de língua natural e respondem aos usuários com conversa semelhante à humana.

Como a integração funciona:

* Os usuários interagem com a interface com o usuário de front-end de seu app.
* Seu app envia a entrada do usuário para o {{site.data.keyword.vr}} usando o {{site.data.keyword.watson}} Go SDK.
* O {{site.data.keyword.watson}} Go SDK se conecta a uma área de trabalho, que é um contêiner para o seu fluxo de diálogo e os seus dados de treinamento.
* A área de trabalho interpreta a entrada do usuário e direciona o fluxo da conversa, enviando uma resposta para o app.
* Seu app exibe a resposta para o usuário.

## Antes de começar
{: #before-you-begin}

Instale o {{site.data.keyword.watson}} Go SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## Incluindo um Reconhecimento Visual em seu app
{: #add-a-virtual-assistant-to-your-app}

1. Após ter transferido seu código por download, abra seu projeto. 
2. Inclua uma instrução de importação para {{site.data.keyword.conversationshort}}
3. Instancie o serviço. O exemplo usa um chamado `visualRecognition`.
4. Abaixo está um exemplo de como é possível interagir com o serviço. Este exemplo inclui 'GET' e 'LIST' de uma área de trabalho. 

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

## Usando kits iniciadores
{: #vr_starterkits}

Com kits do iniciador, é possível alavancar de forma rápida e fácil os recursos do {{site.data.keyword.cloud_notm}}. É possível incluir o {{site.data.keyword.vr}} em qualquer back-end do lado do servidor usando os kits do iniciador. O kit do iniciador do Chatbot for iOS with Watson ilustra como usar os recursos de deep learning do {{site.data.keyword.vr}}, incluindo uma interface de língua natural em seu aplicativo que automatiza interações com seus usuários finais.

1. Selecione o [kit do iniciador](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} com o qual você deseja trabalhar.
2. Crie o projeto com os serviços padrão.
3. Clique em **Incluir recursos > Watson > {{site.data.keyword.vr}}**.
4. Faça download do projeto clicando em **Fazer download do código**. É possível localizar as credenciais de serviço no arquivo `config/local-dev.json`.

## Próximas Etapas
{: #assistant_next}

Ótimo trabalho! Você incluiu o Reconhecimento Visual em seu app. Tente uma das opções a seguir para manter o ritmo:
* Confira o [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Aproveite todos os recursos que o [{{site.data.keyword.vr}}](/docs/services/vr/index.html) tem para oferecer.
