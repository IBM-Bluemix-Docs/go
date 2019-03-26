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

# 新增 Visual Recognition
{: #assistant-vr}

您可以使用 {{site.data.keyword.visualrecognitionshort}} 服務來建置可瞭解自然語言輸入，並以擬人交談回應使用者的 Go 應用程式。

整合運作方式：

* 使用者會與應用程式的前端使用者介面互動。
* 您的應用程式會透過 {{site.data.keyword.watson}} Go SDK 將使用者輸入傳送至 {{site.data.keyword.visualrecognitionshort}}。
* {{site.data.keyword.watson}} Go SDK 會連接至一個工作區，該工作區是對話流程及訓練資料的容器。
* 此工作區解譯使用者輸入並指揮對話流程，然後傳送回應至您的應用程式。
* 您的應用程式顯示對使用者的回應。

## 開始之前
{: #prereqs-vr}

安裝 {{site.data.keyword.watson}} Go SDK：
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## 將 Visual Recognition 新增至應用程式
{: #add-vr}

1. 在下載程式碼之後，開啟您的專案。 
2. 為 {{site.data.keyword.conversationshort}} 新增 import 陳述式。
3. 將服務實例化。此範例使用一個稱為 `visualRecognition` 的服務。
4. 以下是您可以如何與服務互動的範例。這個範例包括工作區的 'GET' 及 'LIST'。 

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

## 使用入門範本套件
{: #starterkits-vr}

使用入門範本套件，您可以快速且輕鬆地運用 {{site.data.keyword.cloud_notm}} 的功能。您可以使用入門範本套件，將 {{site.data.keyword.visualrecognitionshort}} 新增至任何伺服器端後端。Chatbot for iOS with Watson 入門範本套件說明如何在自動化一般使用者互動的應用程式中新增自然語言介面，藉以使用 {{site.data.keyword.visualrecognitionshort}} 的深入學習功能。

1. 選取您要使用的[入門範本套件](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}。
2. 建立含有預設服務的專案。
3. 按一下**新增資源 > Watson > {{site.data.keyword.visualrecognitionshort}}**。
4. 按一下**下載程式碼**，以下載專案。您可以在 `config/local-dev.json` 檔案中找到服務認證。

## 後續步驟
{: #next-vr}

做得好！您已將 Visual Recognition 新增至您的應用程式。嘗試下列其中一個選項，以保持動力：
* 查看 [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}。
* 充分運用 [{{site.data.keyword.visualrecognitionshort}}](/docs/services/vr/index.html) 提供的所有特性。
