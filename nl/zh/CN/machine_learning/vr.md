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

# 添加 Visual Recognition
{: #assistant-vr}

可以使用 {{site.data.keyword.visualrecognitionshort}} 服务来构建可理解自然语言输入并通过类似真人的对话来响应用户的 Go 应用程序。

集成工作方式：

* 用户与应用程序的前端用户界面进行交互。
* 应用程序使用 {{site.data.keyword.watson}} Go SDK 将用户输入发送到 {{site.data.keyword.visualrecognitionshort}}。
* {{site.data.keyword.watson}} Go SDK 连接到工作空间，此工作空间是用于对话流和训练数据的容器。
* 工作空间解读用户输入并定向对话流，以向应用程序发送响应。
* 应用程序为用户显示响应。

## 开始之前
{: #prereqs-vr}

安装 {{site.data.keyword.watson}} Go SDK：
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## 向应用程序添加 Visual Recognition
{: #add-vr}

1. 下载代码后，打开项目。 
2. 添加用于 {{site.data.keyword.conversationshort}} 的 import 语句
3. 实例化服务。示例使用名为 `visualRecognition` 的服务。
4. 以下是如何与服务交互的示例。此示例包含工作空间的“GET”和“LIST”。 

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

## 使用入门模板工具包
{: #starterkits-vr}

通过入门模板工具包，可以快速、轻松地利用 {{site.data.keyword.cloud_notm}} 的功能。可以使用入门模板工具包将 {{site.data.keyword.visualrecognitionshort}} 添加到任何服务器端后端。Chatbot for iOS with Watson 入门模板工具包说明了如何通过将自然语言界面添加到自动最终用户与进行交互的应用程序，从而使用 {{site.data.keyword.visualrecognitionshort}} 的深度学习功能。

1. 选择要使用的[入门模板工具包](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}。
2. 使用缺省服务创建项目。
3. 单击**添加资源 > Watson > {{site.data.keyword.visualrecognitionshort}}**。
4. 通过单击**下载代码**来下载项目。可以在 `config/local-dev.json` 文件中找到服务凭证。

## 后续步骤
{: #next-vr}

太棒了！您已将 Visual Recognition 添加到应用程序。请一鼓作气尝试下列其中一个选项：
* 请查看 [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}。
* 利用 [{{site.data.keyword.visualrecognitionshort}}](/docs/services/vr/index.html) 可以提供的所有功能。
