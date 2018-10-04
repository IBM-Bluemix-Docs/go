---

copyright:
  years: 2018
lastupdated: "2018-10-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Getting started tutorial

The following tutorial walks you through the steps to create and deploy a Go app by using {{site.data.keyword.cloud_notm}} provided tools. You can use the [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) on the command line or the web-based [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) as shown in the following tutorial steps. By using either of these methods, you can generate a production-ready Go application in just minutes.

## Step 1. Creating a Go app in the dashboard
{: #create_project}

1. From the [Starter Kits](https://console.bluemix.net/developer/appservice/starter-kits) page in the App Service, select a Starter Kit that is written in `Go`. You can also create a blank starter app by clicking **Create app** and selecting `Go` as the language.

    You must be logged in to an {{site.data.keyword.cloud_notm}} account to create a project. If you don't have an account, you can [register for a free account](https://console.bluemix.net/registration).
    {: tip}

3. Click **Create app**.
4. **Name** your app or use the generic app name that is provided.
5. Enter a **unique host name**. The host name is used to access your application, for example: `server.mybluemix.net`.
6. Click **Create**. After your project is created, you can deploy by using a toolchain or you can continue to build, and deploy your project from the [command line](/docs/cli/idt/index.html).

## Step 2. Deploying with the dashboard
{: #deploy_dashboard}

1. From the App Details page, click **Deploy to Cloud**.
2. Next, select one of the following deployment methods:
    * **Deploy to Kubernetes** - You must create a set of worker nodes. For example, you can use VMs to deploy and manage highly available application containers. You can create a cluster or deploy to an existing cluster.
    * **Deploy to Cloud Foundry** - You do not need to manage the underlying infrastructure.
    * **Deploy to a Virtual Server (Beta)** - A [Pay-As-You-Go account](https://console.bluemix.net/dashboard/ibm-iaas-g1) is required to access virtual servers.
3. Select your options, and then click **Create**. A toolchain is created for you and deploys your app automatically.

## Step 3. Adding a service (Optional)
{: #add_service}

You can quickly add services like security or storage to your Go app from within the dashboard.

1. Return to your Go app in the [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard).
2. Click **Add Resource**, select the category of the service you want to add, click **Next**, then choose your service. {{site.data.keyword.cloud_notm}} App Service creates the service for you based on the selected plan. If you previously created the service that you plan to use, choose the **Existing** category.
3. After the service is created, click **Download Code** to regenerate the project with the SDK that connects to your service.

## Step 4. Testing and accessing your Go app locally
{: #run_local}

You can use the [{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) to work with your Go app locally by using `ibmcloud dev` commands.

1. Use the `ibmcloud dev build` command to build your application.
2. Use the `ibmcloud dev run` command to run the application locally. Your application is run in the Docker containers on your local system. You can test your application in a browser by accessing `http://localhost:8080`.
3. A health check endpoint is available at `http://localhost:8080/health`.
4. You can access metrics at `http://localhost:3000/metrics`.

To learn more about the {{site.data.keyword.dev_cli_long}}, see the complete [CLI documentation](/docs/cli/idt/index.html).

## Exploring alternative deployment methods
{: #deploy_cloud notoc}

Learn how to deploy your application to Cloud Foundry or Kubernetes under the Deployments tab [here](/docs/go/deploying_apps.html). 

To expand your Go application, continue checking out the topics in the Go programming guide.
