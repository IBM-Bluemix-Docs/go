---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-30"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry, go deploy virtual

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 部署和集成 Go 应用程序
{: #go-deploy-apps}

可以使用工具链或命令行界面来部署 Go 应用程序。工具链是一组工具集成，可帮助构建和自动化应用程序部署。要了解有关工具链及其工作方式的更多信息，请参阅 [Continuous Delivery](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started)。本主题帮助您查找有关 Go 应用程序的不同部署方法（例如，CLI、Kubernetes、容器、VSI 和私有云）的有用信息。

## 部署到 Kubernetes 集群
{: #deploy_kube-go}

您可以了解如何使用 {{site.data.keyword.cloud_notm}} Kubernetes Service 来部署容器化 Go 应用程序。请查看[教程步骤](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)以获取有关在 {{site.data.keyword.cloud_notm}} 中设置 Kubernetes 集群的更多信息。

使用 CLI 的相关博客：
* [使用 {{site.data.keyword.dev_cli_long}} CLI 部署到 Kubernetes](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。
* [使用 {{site.data.keyword.dev_cli_short}} CLI 部署到 IBM Cloud Private](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。

## 使用 CLI 部署到 Container Service
{: #go-deploy-container}

使用 `ibmcloud dev deploy` 命令以部署到 {{site.data.keyword.cloud_notm}}。 

要部署到 {{site.data.keyword.cloud_notm}} 中的 IBM Container Services，请使用以下命令：
```
ibmcloud dev deploy –target container 
```
{: codeblock}

有关 `ibmcloud dev` 命令的更多信息，请参阅[开发和部署应用程序](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)。

## 部署到 Cloud Foundry
{: #go-deploy-cf}

此选项可部署云本机应用程序，而无需管理底层基础架构。

如果计划将应用程序部署到 [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about#about)，那么必须[准备 {{site.data.keyword.cloud_notm}} 帐户](/docs/cloud-foundry?topic=cloud-foundry-prepare#prepare)。

如果您的帐户有权访问 {{site.data.keyword.cfee_full_notm}}，那么可以选择部署程序类型**[公共云](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)**或**[企业环境](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)**，可使用这些类型来创建和管理隔离的环境，以用于专门为您的企业托管 Cloud Foundry 应用程序。

## 部署虚拟服务器
{: #go-vsi-deploy}

将 {{site.data.keyword.cloud}} App Service 应用程序部署到虚拟服务器实例，以支持平台和基础架构开发者活动协同工作并提高应用程序控制和灵活性。

与其他配置相比，虚拟服务器实例可以为所有工作负载类型提供更好的透明度、可预测性和自动化功能。与裸机服务器相结合，还可以创建出独特的工作负载组合。例如，可以使用运行基于 Debian Linux 的操作系统的裸机和 GPU 配置来创建高性能的数据库逻辑或机器学习。

有关更多信息，请参阅[部署到虚拟服务器](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy)。

