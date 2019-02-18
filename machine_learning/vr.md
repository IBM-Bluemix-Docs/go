---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Adding Visual Recognition
{: #assistant-vr}

You can use the {{site.data.keyword.vr}} service to build Go applications that understand natural-language input, and respond to users with human-like conversation.

How the integration works:

* Users interact with the front-end user interface of your app.
* Your app sends user input to the {{site.data.keyword.vr}} by using the {{site.data.keyword.watson}} Go SDK.
* The {{site.data.keyword.watson}} Go SDK connects to a workspace, which is a container for your dialog flow and training data.
* The workspace interprets the user input and directs the flow of the conversation, sending a response to your app.
* Your app displays the response for the user.

## Before you begin
{: #prereqs-vr}

Install the {{site.data.keyword.watson}} Go SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## Adding a Visual Recognition to your app
{: #add-vr}

1. After you have downloaded your code, open your project. 
2. Add an import statement for {{site.data.keyword.conversationshort}}
3. Instantiate the service. The example uses one called `visualRecognition`.
4. Below is an example of how you can interact with the service. This example includes 'GET' and 'LIST' of a workspace. 

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

## Using starter kits
{: #starterkits-vr}

With starter kits, you can quickly and easily leverage the capabilities of {{site.data.keyword.cloud_notm}}. You can add {{site.data.keyword.vr}} to any server-side back end by using the starter kits. The Chatbot for iOS with Watson starter kit illustrates how to use the deep learning capabilities of {{site.data.keyword.vr}}, by adding a natural language interface to your application that automates interactions with your end users.

1. Select the [starter kit](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window} with which you want to work.
2. Create the project with the default services.
3. Click **Add Resources > Watson > {{site.data.keyword.vr}}**.
4. Download the project by clicking **Download Code**. You can find the service credentials in the `config/local-dev.json` file.

## Next steps
{: #next-vr}

Great job! You added Visual Recognition to your app. Keep the momentum by trying one of the following options:
* Check out the [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Take advantage of all of the features that [{{site.data.keyword.vr}}](/docs/services/vr/index.html) has to offer.