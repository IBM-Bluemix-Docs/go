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

# Ajout de Text to Speech 
{: #assistant-text}

Vous pouvez utiliser le service {{site.data.keyword.texttospeechfull}} pour générer des applications Go qui comprennent les entrées de texte en langage naturel et les convertissent en audio. 

L'intégration fonctionne de la manière suivante :

* Les utilisateurs interagissent avec l'interface utilisateur frontale de votre application.
* Votre application envoie une entrée utilisateur à {{site.data.keyword.texttospeechshort}} à l'aide du logiciel SDK Go {{site.data.keyword.watson}}.
* Le logiciel SDK Go {{site.data.keyword.watson}} se connecte à un espace de travail, qui est un conteneur pour votre flux de dialogues et vos données d'apprentissage.
* L'espace de travail interprète l'entrée de l'utilisateur et envoie une réponse audio à votre application.
* Votre application affiche la réponse à l'utilisateur.

## Avant de commencer
{: #prereqs-text}

Installez le logiciel SDK Go {{site.data.keyword.watson}} :
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## Ajout de Text to Speech à votre application
{: #existing-text}

1. Après avoir téléchargé le code, ouvrez votre projet. 
2. Ajoutez une instruction d'importation pour {{site.data.keyword.texttospeechshort}}
3. Instanciez le service. L'exemple utilise `textToSpeech`. 
4. L'exemple suivant montre comment interagir avec le logiciel SDK pour synthétiser un service Text To Speech.

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

## Utilisation des kits de démarrage
{: #starterkits-text}

Les kits de démarrage vous permettent d'utiliser les fonctionnalités d'{{site.data.keyword.cloud_notm}} de manière rapide et facile. Vous pouvez ajouter {{site.data.keyword.texttospeechshort}} à un système de back-end côté serveur avec les kits de démarrage. Le kit de démarrage Chatbot for iOS with Watson illustre comment utiliser les fonctions d'apprentissage en profondeur de {{site.data.keyword.texttospeechshort}} par l'ajout d'une interface en langage naturel à votre application qui automatise les interactions avec vos utilisateurs finaux.

1. Sélectionnez le [kit de démarrage](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} avec lequel vous souhaitez travailler.
2. Créez le projet avec les services par défaut.
3. Cliquez sur **Ajouter des ressources > Watson > {{site.data.keyword.texttospeechshort}}**.
4. Téléchargez le projet en cliquant sur **Télécharger le code**. Vous trouverez les données d'identification du service dans le fichier `config/local-dev.json`.

## Etapes suivantes
{: #next-text}

Félicitations ! Vous avez ajouté Text To Speech à votre application. Continuez sur votre lancée en essayant l'une des options suivantes :
* Consultez le [logiciel SDK Go {{site.data.keyword.watson}} ](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Tirez parti des toutes les fonctions offertes par [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html).
