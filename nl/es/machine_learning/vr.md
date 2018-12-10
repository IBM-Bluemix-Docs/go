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

# Adición de Visual Recognition
{: #assistant}

Puede utilizar el servicio {{site.data.keyword.vr}} para crear aplicaciones Go que comprendan la entrada de lenguaje natural y respondan a los usuarios con conversación de tipo humano.

Funcionamiento de la integración:

* Los usuarios interactúan con la interfaz de usuario frontal de su app.
* La app envía la entrada de usuario a {{site.data.keyword.vr}} utilizando el SDK de Go de {{site.data.keyword.watson}}.
* El SDK de Go de {{site.data.keyword.watson}} se conecta a un espacio de trabajo, que es un contenedor para el flujo de diálogo y los datos de entrenamiento.
* El espacio de trabajo interpreta la entrada de usuario y dirige el flujo de la conversación, enviando una respuesta a la app.
* La app muestra la respuesta para el usuario.

## Antes de empezar
{: #before-you-begin}

Instale el SDK de Go de {{site.data.keyword.watson}}:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## Adición del servicio Visual Recognition a la app
{: #add-a-virtual-assistant-to-your-app}

1. Tras descargar su código, abra el proyecto. 
2. Añada una sentencia de importación para {{site.data.keyword.conversationshort}}.
3. Cree una instancia del servicio. En el ejemplo se utiliza uno llamado `visualRecognition`.
4. A continuación se muestra un ejemplo de cómo puede interactuar con el servicio. Este ejemplo incluye la obtención ('GET') y listado ('LIST') de un espacio de trabajo. 

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
  // Crear instancia del servicio Watson Visual Recognition
  visualRecognition, visualRecognitionErr := NewVisualRecognitionV3(&ServiceCredentials{
    ServiceURL: "YOUR SERVICE URL",
    Version: "2018-03-19",
    APIkey: "YOUR SERVICE API KEY",
  })

  // Comprobar si la instancia es correcta
  if visualRecognitionErr != nil {
    fmt.Println(visualRecognitionErr)
    return
  }

  /* CLASIFICAR */
  // Abrir archivo con imagen para clasificar
  pwd, _ := os.Getwd()
  imageFile, imageFileErr := os.Open(pwd + "/resources/kitty.jpg")

  // Comprobar si la lectura del archivo es correcta
  if imageFileErr != nil {
    fmt.Println(imageFileErr)
    return
  }

  classifyOptions := NewClassifyOptions().
    SetImagesFile(*imageFile).
    SetURL("https://www.readersdigest.ca/wp-content/uploads/2011/01/4-ways-cheer-up-depressed-cat.jpg").
    SetThreshold(0.6).
    SetClassifierIds([]string{ "default", "food", "explicit" })

  // Llamar al método Classify de Visual Recognition
  classify, classifyErr := visualRecognition.Classify(classifyOptions)

  // Comprobar si la llamada es correcta
  if classifyErr != nil {
    fmt.Println(classifyErr)
    return
  }

  // Indicar a classify.Result el tipo de datos específico devuelto por Classify
  // NOTA: la mayoría de los métodos tienen una función Get<methodName>Result() correspondiente
  classifyResult := GetClassifyResult(classify)

  // Comprobar si la indicación es correcta
  if classifyResult != nil {
    prettyPrint(classifyResult, "Classify")
  }

  /* CREAR CLASIFICADOR */
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

  // Probar clasificador
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

  /* DETECTAR CARAS */
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

## Utilización de los kits de inicio
{: #vr_starterkits}

Con los kits de inicio, puede aprovechar de forma rápida y sencilla las funciones de {{site.data.keyword.cloud_notm}}. Puede añadir {{site.data.keyword.vr}} a cualquier programa de fondo de lado del servidor utilizando los kits de inicio. El Kit de inicio Chatbot for iOS with Watson ilustra cómo utilizar las funciones de aprendizaje profundo de {{site.data.keyword.vr}} añadiendo una interfaz de lenguaje natural a la aplicación que automatiza las interacciones con los usuarios finales.

1. Seleccione el [kit de inicio](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con el que desee trabajar.
2. Cree el proyecto con los servicios predeterminados.
3. Pulse **Añadir recursos > Watson > {{site.data.keyword.vr}}**.
4. Descargue el proyecto pulsando **Descargar código**. Puede encontrar las credenciales de servicio en el archivo `config/local-dev.json`.

## Siguientes pasos
{: #assistant_next}

¡Buen trabajo! Ha añadido el servicio Visual Recognition a la app. Mantenga el ritmo probando una de las opciones siguientes:
* Consulte el [SDK de Go de {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Aproveche todas las características que [{{site.data.keyword.vr}}](/docs/services/vr/index.html) tiene que ofrecer.
