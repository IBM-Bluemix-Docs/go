---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-19"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入门教程
{: #getting-started}

以下教程指导您完成使用 {{site.data.keyword.cloud_notm}} 提供的工具来创建和部署 Go 应用程序的步骤。您可以在命令行上使用 [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) 或使用基于 Web 的 [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")，如以下教程步骤中所示。通过使用这其中任一种方法，您可以在仅仅几分钟内就生成生产就绪型 Go 应用程序。

## 步骤 1. 在仪表板中创建定制 Go 应用程序
{: #create-go-app}

1. 登录到 {{site.data.keyword.cloud_notm}} 帐户以创建应用程序。如果您没有帐户，那么可以[注册免费帐户](https://{DomainName}/registration){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。
2. 在 {{site.data.keyword.dev_console}} 的[入门模板工具包](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 页面中，执行下列其中一个选项：
 * 选择使用 `Go` 编写的入门模板工具包，然后在**入门模板工具包详细信息**页面上，单击**创建应用程序**。
 * 选择空白入门模板应用程序，然后单击**创建应用程序**。
3. 为应用程序提供名称，或者使用提供的通用应用程序名称。
4. 确保将 **Go** 选作平台，然后单击**创建**。创建应用程序后，可以添加服务，然后使用工具链对其进行部署，也可以继续通过[命令行](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)构建并部署项目。

## 步骤 2. 添加 {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

现在，可以将 {{site.data.keyword.watson}} 服务添加到 Go 应用程序。对于本教程，将 {{site.data.keyword.texttospeechshort}} 服务添加到 Go 应用程序，该服务将接受言语输入，然后使用云 API 将其转换为文本。

1. 在**应用程序详细信息**页面中，单击**添加服务**。
2. 选择 **AI**，然后单击**下一步**。
3. 选择 **{{site.data.keyword.texttospeechshort}}**，然后单击**下一步**。
4. 单击**创建**。

将此服务添加到应用程序后，您将看到 [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 依赖项已添加到 `Gopkg.toml`，并且 `services/` 目录中有可用于此服务的简单的检测代码。此外，还包含用于访问相应环境中服务凭证的配置信息。

要下载代码，请单击**应用程序详细信息**页面上的**下载代码**。下载的代码随附 [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 以及一些基本初始化代码。

## 步骤 3. 通过控制台部署应用程序
{: #deploy-go}

1. 在**应用程序详细信息**页面中，单击**配置持续交付**。
2. 根据您所选方法的指示信息来设置部署目标：
  * **部署到 [IBM Kubernetes Service](/docs/apps/deploying?topic=creating-apps-containers-kube)**。此选项将创建一个主机集群（称为工作程序节点）来部署和管理高可用性应用程序容器。可以创建集群或部署到现有集群。
  * **部署到 Cloud Foundry**。此选项可部署云本机应用程序，而无需管理底层基础架构。如果您的帐户有权访问 {{site.data.keyword.cfee_full_notm}}，那么可以选择部署程序类型**[公共云](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)**或**[企业环境](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**，可使用这些类型来创建和管理隔离的环境，以用于专门为您的企业托管 Cloud Foundry 应用程序。
  * **部署到[虚拟服务器](/docs/apps?topic=creating-apps-vsi-deploy)**。此选项会供应虚拟服务器实例，装入包含您的应用程序的映像，创建 DevOps 工具链，并为您启动第一个部署周期。

3. 设置部署目标后，单击**下一步**。
4. 选择配置选项，然后单击**创建**。这将为您创建工具链，并自动部署应用程序。

## 步骤 4. 本地测试和访问 Go 应用程序
{: #run_local-go}

在**应用程序详细信息**页面上，可以：
* 通过单击**查看存储库**来查看存储库。
* 通过单击**查看工具链**来查看和修改工具链。

您可以使用 {{site.data.keyword.dev_cli_long}} 通过 `ibmcloud dev` 命令在本地处理 Go 应用程序。这些工具可帮助您快速在本地进行迭代，然后将您的更改推送到云。

1. 使用 `ibmcloud dev build` 命令构建应用程序。
2. 使用 `ibmcloud dev run` 命令在本地运行应用程序。应用程序将在本地系统上的 Docker 容器中运行。可以在浏览器中通过访问 `http://localhost:8080` 来测试应用程序。
3. 运行状况检查端点在 `http://localhost:8080/health` 处可用。
4. 您可以在 `http://localhost:3000/metrics` 中访问度量值。

要了解有关 {{site.data.keyword.dev_cli_notm}} 的更多信息，请参阅完整的 [CLI 文档](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)。

现在，您已准备好使用 {{site.data.keyword.texttospeechshort}} 服务构建应用程序了！

## 探索替代部署方法
{: #alt-deploy-go notoc}

了解如何在[“部署”选项卡](/docs/go?topic=go-go-deploy-apps)下部署应用程序。

要扩展 Go 应用程序，请继续查看 Go 编程指南中的主题。
