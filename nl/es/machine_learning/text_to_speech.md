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

# Adición de Text to Speech 
{: #assistant}

Puede utilizar el servicio {{site.data.keyword.text_to_speech}} para crear aplicaciones Go que comprendan la entrada de lenguaje natural y la conviertan en audio. 

Funcionamiento de la integración:

* Los usuarios interactúan con la interfaz de usuario frontal de su app.
* La app envía la entrada de usuario a {{site.data.keyword.text_to_speech}} utilizando el SDK de Go de {{site.data.keyword.watson}}.
* El SDK de Go de {{site.data.keyword.watson}} se conecta a un espacio de trabajo, que es un contenedor para el flujo de diálogo y los datos de entrenamiento.
* El espacio de trabajo interpreta la entrada de usuario y envía una respuesta de audio a la app.
* La app muestra la respuesta para el usuario.

## Antes de empezar
{: #before-you-begin}

Instale el SDK de Go de {{site.data.keyword.watson}}:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## Adición del servicio Text to Speech a la app
{: #add-a-text-to-speech-to-your-app}

1. Tras descargar su código, abra el proyecto. 
2. Añada una sentencia de importación para {{site.data.keyword.text_to_speech}}.
3. Cree una instancia del servicio. En el ejemplo se utiliza uno llamado `textToSpeech`. 
4. En el ejemplo siguiente se muestra cómo interactuar con el SDK para sintetizar un servicio Text to Speech.

```golang
package main

import (
  "fmt"
  . "go-sdk/textToSpeechV1"
  "bytes"
  "os"
)

func main() {
  // Crear instancia del servicio Watson Text To Speech
  textToSpeech, textToSpeechErr := NewTextToSpeechV1(&ServiceCredentials{
    ServiceURL: "YOUR SERVICE URL",
    Version: "2017-09-21",
    Username: "YOUR SERVICE USERNAME",
    Password: "YOUR SERVICE PASSWORD",
  })

  // Comprobar si la instancia es correcta
  if textToSpeechErr != nil {
    fmt.Println(textToSpeechErr)
    return
  }

  /* SINTETIZAR */
  synthesizeOptions := NewSynthesizeOptions("Hello World").
    SetAccept("audio/mp3").
    SetVoice("en-GB_KateVoice")

  // Llamar al método Synthesize de textToSpeech
  synthesize, synthesizeErr := textToSpeech.Synthesize(synthesizeOptions)

  // Comprobar si la llamada es correcta
  if synthesizeErr != nil {
    fmt.Println(synthesizeErr)
    return
  }

  // Indicar a synthesize.Result el tipo de datos específico devuelto por Synthesize
  // NOTA: la mayoría de los métodos tienen una función Get<methodName>Result() correspondiente
  synthesizeResult := GetSynthesizeResult(synthesize)

  // Comprobar si la indicación es correcta
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

## Utilización de los kits de inicio
{: #text_to_speech_starterkits}

Con los kits de inicio, puede utilizar de forma rápida y sencilla las funciones de {{site.data.keyword.cloud_notm}}. Puede añadir {{site.data.keyword.text_to_speech}} a cualquier programa de fondo de lado del servidor utilizando los kits de inicio. El Kit de inicio Chatbot for iOS with Watson ilustra cómo utilizar las funciones de aprendizaje profundo de {{site.data.keyword.text_to_speech}} añadiendo una interfaz de lenguaje natural a la aplicación que automatiza las interacciones con los usuarios finales.

1. Seleccione el [kit de inicio](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} con el que desee trabajar.
2. Cree el proyecto con los servicios predeterminados.
3. Pulse **Añadir recursos > Watson > {{site.data.keyword.text_to_speech}}**.
4. Descargue el proyecto pulsando **Descargar código**. Puede encontrar las credenciales de servicio en el archivo `config/local-dev.json`.

## Siguientes pasos
{: #assistant_next}

¡Buen trabajo! Ha añadido el servicio Text to Speech a la app. Mantenga el ritmo probando una de las opciones siguientes:
* Consulte el [SDK de Go de {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Aproveche todas las características que [{{site.data.keyword.text_to_speech}}](/docs/services/text_to_speech/index.html) ofrece.
