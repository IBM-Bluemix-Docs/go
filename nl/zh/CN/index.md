---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入门教程
{: #getting-started}

以下教程指导您完成使用 {{site.data.keyword.cloud_notm}} 提供的工具来创建和部署 Go 应用程序的步骤。您可以在命令行上使用 [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) 或使用基于 Web 的 [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard)，如以下教程步骤中所示。通过使用这其中任一种方法，您可以在仅仅几分钟内就生成生产就绪型 Go 应用程序。

## 步骤 1. 在仪表板中创建 Go 应用程序
{: #create-go-app}

1. 从 App Service 中的[入门模板工具包](https://cloud.ibm.com/developer/appservice/starter-kits)页面中，选择使用 `Go` 编写的入门模板工具包。此外，还可以通过单击**创建应用程序**并选择 `Go` 作为语言来创建空白的入门模板应用程序。

    您必须登录到 {{site.data.keyword.cloud_notm}} 帐户才能创建应用程序。如果您没有帐户，那么可以[注册免费帐户](https://cloud.ibm.com/registration)。
  {: tip}

3. 单击**创建应用程序**。
4. **命名**应用程序或者使用提供的通用应用程序名称。
5. 输入**唯一主机名**。主机名用于访问应用程序，例如：`server.mybluemix.net`。
6. 单击**创建**。创建应用程序后，可以使用工具链进行部署，也可以继续构建并通过[命令行](/docs/cli/index.html#ibmcloud-cli)部署项目。

## 步骤 2. 使用仪表板进行部署
{: #deploy-go}

1. 在“应用程序详细信息”页面中，单击**部署到云**。
2. 根据您所选方法的指示信息来设置部署方法：
  * **部署到 [Kubernetes](/docs/apps/deploying/containers.html#containers)**。此选项将创建一个主机集群（称为工作程序节点）来部署和管理高可用性应用程序容器。可以创建集群或部署到现有集群。
  * **部署到 Cloud Foundry**。此选项可部署云本机应用程序，而无需管理底层基础架构。如果您的帐户有权访问 {{site.data.keyword.cfee_full_notm}}，那么可以选择部署程序类型**[公共云](/docs/cloud-foundry-public/about-cf.html#about-cf)**或**[企业环境](/docs/cloud-foundry-public/cfee.html#cfee)**，可使用这些类型来创建和管理隔离的环境，以用于专门为您的企业托管 Cloud Foundry 应用程序。
  * **部署到[虚拟服务器](/docs/apps/vsi-deploy.html#vsi-deploy)**。此选项会供应虚拟服务器实例，装入包含您的应用程序的映像，创建 DevOps 工具链，并为您启动第一个部署周期。

3. 选择选项，然后单击**创建**。将为您创建工具链，并自动部署应用程序。

## 步骤 3. 添加资源（可选）
{: #add-resource-go}

您可以从仪表板中快速向 Go 应用程序添加服务，例如，安全性或存储器。

1. 在 [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard) 中返回到 Go 应用程序。
2. 单击**添加资源**，选择要添加的服务的类别，单击**下一步**，然后选择服务。{{site.data.keyword.cloud_notm}} App Service 会根据所选套餐创建服务。
如果先前已创建您计划使用的服务，请选择**现有**类别。
3. 创建服务后，单击**下载代码**以使用连接到服务的 SDK 重新生成项目。

## 步骤 4. 本地测试和访问 Go 应用程序
{: #run_local-go}

您可以使用 [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) 通过 `ibmcloud dev` 命令在本地处理 Go 应用程序。

1. 使用 `ibmcloud dev build` 命令构建应用程序。
2. 使用 `ibmcloud dev run` 命令在本地运行应用程序。应用程序将在本地系统上的 Docker 容器中运行。可以在浏览器中通过访问 `http://localhost:8080` 来测试应用程序。
3. 运行状况检查端点在 `http://localhost:8080/health` 处可用。
4. 您可以在 `http://localhost:3000/metrics` 中访问度量值。

要了解有关 {{site.data.keyword.dev_cli_long}} 的更多信息，请参阅完整的 [CLI 文档](/docs/cli/index.html#ibmcloud-cli)。

## 探索替代部署方法
{: #deploy_cloud-go notoc}

在[此处](/docs/go/deploying_apps.html)了解如何在“部署”选项卡下部署应用程序。 

要扩展 Go 应用程序，请继续查看 Go 编程指南中的主题。
