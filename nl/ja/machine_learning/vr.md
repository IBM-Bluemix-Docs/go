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

# Visual Recognition の追加
{: #assistant-vr}

{{site.data.keyword.visualrecognitionshort}} サービスを使用して、自然言語による入力を理解して人間と会話しているかのようにユーザーに応答する Go アプリケーションを構築することができます。

統合の仕組み:

* ユーザーは、アプリのフロントエンド・ユーザー・インターフェースと対話します。
* アプリは、ユーザーの入力内容を {{site.data.keyword.watson}} Go SDK を使用して {{site.data.keyword.visualrecognitionshort}} に送信します。
* {{site.data.keyword.watson}} Go SDK はワークスペースに接続します。ワークスペースはダイアログ・フローとトレーニング・データのコンテナーです。
* ワークスペースはユーザー入力を解釈し、対話のフローを指図し、応答をアプリに送信します。
* アプリはユーザーに応答を表示します。

## 始める前に
{: #prereqs-vr}

以下のようにして {{site.data.keyword.watson}} Go SDK をインストールします。
```bash
go get github.com/watson-developer-cloud/go-sdk
```
## アプリへの Visual Recognition の追加
{: #add-vr}

1. コードをダウンロードしたら、プロジェクトを開きます。 
2. {{site.data.keyword.conversationshort}} の import ステートメントを追加します。
3. サービスをインスタンス化します。 以下の例では、`visualRecognition` というサービスを使用します。
4. 以下に、サービスと対話する方法の例を示します。 この例には、ワークスペースの「GET」および「LIST」が含まれています。 

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

## スターター・キットの使用
{: #starterkits-vr}

スターター・キットを使用すると、{{site.data.keyword.cloud_notm}} の機能を素早く簡単に利用できます。 スターター・キットを使用して、{{site.data.keyword.visualrecognitionshort}} を任意のサーバー・サイドのバックエンドに追加できます。Chatbot for iOS with Watson Starter Kit では、エンド・ユーザーとの対話を自動化するアプリケーションに自然言語インターフェースを追加することによって {{site.data.keyword.visualrecognitionshort}} のディープ・ラーニング機能を使用する方法について説明しています。

1. 使用する[スターター・キット](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}を選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「リソースの追加」>「Watson」>「{{site.data.keyword.visualrecognitionshort}}」**をクリックします。
4. **「コードのダウンロード」**をクリックして、プロジェクトをダウンロードします。 サービス資格情報は、`config/local-dev.json` ファイルにあります。

## 次のステップ
{: #next-vr}

お疲れさまでした。Visual Recognition がアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。
* [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window} を使ってみる。
* [{{site.data.keyword.visualrecognitionshort}}](/docs/services/vr/index.html) が提供するすべての機能を利用する。
