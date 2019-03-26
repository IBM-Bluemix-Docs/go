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

# 添加 Text to Speech 
{: #assistant-text}

可以使用 {{site.data.keyword.texttospeechfull}} 服务来构建可理解自然语言文本输入并将其转换为音频的 Go 应用程序。 

集成工作方式：

* 用户与应用程序的前端用户界面进行交互。
* 应用程序使用 {{site.data.keyword.watson}} Go SDK 将用户输入发送到 {{site.data.keyword.texttospeechshort}}。
* {{site.data.keyword.watson}} Go SDK 连接到工作空间，此工作空间是用于对话流和训练数据的容器。
* 工作空间解读用户输入并向应用程序发送音频响应。
* 应用程序为用户显示响应。

## 开始之前
{: #prereqs-text}

安装 {{site.data.keyword.watson}} Go SDK：
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: pre}

## 向应用程序添加 Text to Speech
{: #existing-text}

1. 下载代码后，打开项目。 
2. 添加用于 {{site.data.keyword.texttospeechshort}} 的 import 语句
3. 实例化服务。示例使用名为 `textToSpeech` 的服务。 
4. 以下示例显示如何与 SDK 交互以合成 Text To Speech 服务。

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

## 使用入门模板工具包
{: #starterkits-text}

通过入门模板工具包，可以快速、轻松地使用 {{site.data.keyword.cloud_notm}} 的功能。可以使用入门模板工具包将 {{site.data.keyword.texttospeechshort}} 添加到任何服务器端后端。Chatbot for iOS with Watson 入门模板工具包说明了如何通过将自然语言界面添加到自动最终用户与进行交互的应用程序，从而使用 {{site.data.keyword.texttospeechshort}} 的深度学习功能。

1. 选择要使用的[入门模板工具包](https://cloud.ibm.com/developer/appledevelopment/starter-kits){:new_window}。
2. 使用缺省服务创建项目。
3. 单击**添加资源 > Watson > {{site.data.keyword.texttospeechshort}}**。
4. 通过单击**下载代码**来下载项目。可以在 `config/local-dev.json` 文件中找到服务凭证。

## 后续步骤
{: #next-text}

太棒了！您已将 Text To Speech 添加到应用程序。请一鼓作气尝试下列其中一个选项：
* 请查看 [{{site.data.keyword.watson}} Go SDK](https://github.com/watson-developer-cloud/go-sdk){:new_window}。
* 利用 [{{site.data.keyword.texttospeechshort}}](/docs/services/text_to_speech/index.html) 提供的所有功能。
