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

# Text to Speech の追加 
{: #assistant-text}

{{site.data.keyword.texttospeechfull}} サービスを使用して、自然言語のテキスト入力を理解して音声に変換する Go アプリケーションを作成できます。 

統合の仕組み:

* ユーザーは、アプリのフロントエンド・ユーザー・インターフェースと対話します。
* アプリは、ユーザーの入力内容を {{site.data.keyword.watson}} Go SDK を使用して {{site.data.keyword.texttospeechshort}} に送信します。
* {{site.data.keyword.watson}} Go SDK はワークスペースに接続します。ワークスペースはダイアログ・フローとトレーニング・データのコンテナーです。
* ワークスペースはユーザー入力を解釈し、音声応答をアプリに送信します。
* アプリはユーザーに応答を表示します。

## 始める前に
{: #prereqs-text}

以下のようにして {{site.data.keyword.watson}} Go SDK をインストールします。
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## アプリへの Text to Speech の追加
{: #existing-text}

1. コードをダウンロードしたら、プロジェクトを開きます。 
2. {{site.data.keyword.texttospeechshort}} の import ステートメントを追加します。
3. サービスをインスタンス化します。 以下の例では、`textToSpeech` というサービスを使用します。 
4. 以下の例は、SDK と対話して Text To Speech サービスを合成する方法を示しています。

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

## スターター・キットの使用
{: #starterkits-text}

スターター・キットを使用すると、{{site.data.keyword.cloud_notm}} の機能を素早く簡単に利用できます。 スターター・キットを使用して、{{site.data.keyword.texttospeechshort}} を任意のサーバー・サイドのバックエンドに追加できます。Chatbot for iOS with Watson Starter Kit では、エンド・ユーザーとの対話を自動化するアプリケーションに自然言語インターフェースを追加することによって {{site.data.keyword.texttospeechshort}} のディープ・ラーニング機能を使用する方法について説明しています。

1. 使用する[スターター・キット](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}を選択します。
2. デフォルト・サービスを使用してプロジェクトを作成します。
3. **「リソースの追加」>「Watson」>「{{site.data.keyword.texttospeechshort}}」**をクリックします。
4. **「コードのダウンロード」**をクリックして、プロジェクトをダウンロードします。 サービス資格情報は、`config/local-dev.json` ファイルにあります。

## 次のステップ
{: #next-text}

お疲れさまでした。Text To Speech がアプリに追加されました。 この調子で、以下のいずれかのオプションを試してみてください。
* [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window} を使ってみる。
* [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html) が提供するすべての機能を利用する。
