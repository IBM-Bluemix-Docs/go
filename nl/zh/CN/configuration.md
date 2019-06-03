---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-19"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 配置 Go 环境
{: #configure-go-env}

可遵循标准化准则来开发 Go 应用程序，从而帮助维护一致的可移植性。注意事项包括凭证管理、保存数据、存储数据和将内容发布到云。通过遵循云本机实践，Go 应用程序可轻松地从一个环境移至另一个环境。例如，从测试移至生产，而无需更改代码或执行除此之外未测试的代码路径。

无论您是需要向现有 Go 应用程序添加云支持，还是使用入门模板工具包创建 Go 应用程序，目标都是提供可移植性以用于任何开发平台。

## 向现有 Go 应用程序添加云支持
{: #go-add-cloud-support}

[`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 模块聚集了各种云提供者（例如，Cloud Foundry 和 Kubernetes）的环境变量，因此应用程序独立于环境。

### 安装 `ibm-cloud-env-golang` 模块
{: #go-install-env-module}

1. 使用以下命令安装 `ibm-cloud-env-golang` 模块：
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. 在代码中通过引用 `mappings.json` 文件来初始化模块：
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  因为 Go 不支持缺省参数，因此未提供缺省 `mappings.json` 文件。如果未在 `IBMCloudEnv.Initialize()` 中指定映射文件路径，那么将记录一条错误。
  {: tip}

  示例 `mappings.json` 文件：
  ```javascript
  {
      "service1-credentials": {
        "searchPatterns": [
      "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name", 
              "env:my-service1-credentials", 
              "file:/localdev/my-service1-credentials.json" 
          ]
      },
    "service2-username": {
        "searchPatterns": [
      "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### 检索服务凭证
{: #go-get-creds}

使用以下命令检索应用程序中的值。

1. 检索 `service1credentials` 变量：
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. 检索 `service2username` 变量：
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

现在，通过对从不同云计算提供者引入的差异进行抽象化，应用程序可以在任何运行时环境中实现。

### 过滤标记和标签的值
{: #go-filter-creds}

可以过滤模块基于服务标记和服务标签生成的凭证，如以下示例中所示：
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## 通过入门模板工具包应用程序使用 Go 配置管理器
{: #go-config-manager}

使用[入门模板工具包](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 创建的 Go 应用程序会自动随附在许多云部署目标（如 Cloud Foundry、Kubernetes、VSI 和 Functions）中运行所需的凭证和配置。

### 了解服务凭证
{: #go-credentials-config}

服务的应用程序配置信息存储在 `/server/config` 目录下的 `localdev-config.json` 文件中。此文件位于 `.gitignore` 目录中，以阻止敏感信息存储在 Git 中。本地运行的任何已配置服务的连接信息（例如，用户名、密码和主机名）都存储在此文件中。

应用程序使用配置管理器从环境和此文件中读取连接和配置信息。应用程序将使用定制构建的 `mappings.json`（位于 `server/config` 目录中）来传达在何处可以找到每个服务的凭证。

如果应用程序在本地运行，那么可以使用从 `mappings.json` 文件中读取的未绑定凭证连接到 {{site.data.keyword.cloud_notm}} 服务。但是如果需要创建未绑定的凭证，那么可以在 {{site.data.keyword.cloud_notm}} Web 控制台（示例）中或使用 [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") `cf create-service-key` 命令进行创建。

将应用程序推送到 {{site.data.keyword.cloud_notm}} 时，不会再使用这些值。应用程序将改为使用环境变量来自动连接到绑定的服务。 

* **Cloud Foundry**：从 `VCAP_SERVICES` 环境变量获取服务凭证。

* **Kubernetes**：从每个服务的不同环境变量获取服务凭证。

* **{{site.data.keyword.cloud_notm}} Container Service**：从虚拟服务器实例或 {{site.data.keyword.openwhisk}} (Openwhisk) 获取服务凭证。

## 后续步骤
{: #go-next-steps-config notoc}

`ibm-cloud-env-golang` 支持使用以下四种搜索模式类型来搜索值：`user-provided`、`cloudfoundry`、`env` 和 `file`。如果您想查看其他支持的搜索模式和搜索模式示例，请查看 [Usage](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 部分。
