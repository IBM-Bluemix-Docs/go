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

# Visual Recognition 추가
{: #assistant-vr}

{{site.data.keyword.visualrecognitionshort}} 서비스를 사용하여 자연어 입력을 이해하고 인간과 유사한 대화로 사용자에게 응답하는 Go 애플리케이션을 빌드할 수 있습니다.

통합이 작동하는 방식:

* 사용자가 앱의 프론트 엔드 사용자 인터페이스와 상호작용합니다.
* 앱이 {{site.data.keyword.watson}} Go SDK를 사용하여 {{site.data.keyword.visualrecognitionshort}}에 사용자 입력을 보냅니다.
* {{site.data.keyword.watson}} Go SDK는 대화 상자 플로우 및 훈련 데이터를 위한 컨테이너인 작업공간에 연결됩니다.
* 작업공간에서는 사용자 입력을 해석하고 대화의 플로우를 지시하며 응답을 앱에 보냅니다.
* 앱이 사용자의 응답을 표시합니다.

## 시작하기 전에
{: #prereqs-vr}

{{site.data.keyword.watson}} Go SDK를 설치하십시오.
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## 앱에 Visual Recognition 추가
{: #add-vr}

1. 코드를 다운로드한 후 프로젝트를 여십시오. 
2. {{site.data.keyword.conversationshort}}에 대한 import 문을 추가하십시오.
3. 서비스를 인스턴스화하십시오. 예에서는 `visualRecognition`을 사용합니다.
4. 다음은 서비스와 상호작용하는 방법에 대한 예입니다. 이 예에는 작업공간의 'GET' 및 'LIST'가 포함되어 있습니다. 

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

## 스타터 킷 사용
{: #starterkits-vr}

스타터 킷을 사용하면 {{site.data.keyword.cloud_notm}}의 기능을 빠르고 쉽게 활용할 수 있습니다. 스타터 킷을 사용하여 서버 측 백엔드에 {{site.data.keyword.visualrecognitionshort}}를 추가할 수 있습니다. Watson 스타터 킷을 사용한 iOS용 챗봇은 일반 사용자와의 상호작용을 자동화하는 애플리케이션에 자연어 인터페이스를 추가하여 {{site.data.keyword.visualrecognitionshort}}의 딥러닝 기능을 사용하는 방법을 보여줍니다.

1. 작업할 [스타터 킷](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}을 선택하십시오.
2. 기본 서비스로 프로젝트를 작성하십시오.
3. **리소스 추가 > Watson > {{site.data.keyword.visualrecognitionshort}}**을 클릭하십시오.
4. **코드 다운로드**를 클릭하여 프로젝트를 다운로드하십시오. `config/local-dev.json` 파일에서 서비스 인증 정보를 찾을 수 있습니다.

## 다음 단계
{: #next-vr}

잘하셨습니다. 앱에 Visual Recognition을 추가했습니다. 다음 옵션 중 하나를 시도하여 계속하십시오.
* [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}를 확인하십시오.
* [{{site.data.keyword.visualrecognitionshort}}](/docs/services/vr/index.html)에서 제공해야 하는 모든 기능을 활용하십시오.
