---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-12"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Getting started tutorial
{: #getting-started}

The following tutorial walks you through the steps to create and deploy a Go app by using {{site.data.keyword.cloud_notm}} provided tools. You can use the [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) on the command line or the web-based [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") as shown in the following tutorial steps. By using either of these methods, you can generate a production-ready Go application in just minutes.

## Step 1. Creating a Go app in the dashboard
{: #create-go-app}

1. From the [Starter Kits](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") page in the {{site.data.keyword.dev_console}}, select a Starter Kit that is written in `Go`. You can also create a blank starter app by clicking **Create app** and selecting `Go` as the language.

    You must be logged in to an {{site.data.keyword.cloud_notm}} account to create an app. If you don't have an account, you can [register for a free account](https://cloud.ibm.com/registration){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
    {: tip}

3. Click **Create app**.
4. **Name** your app or use the generic app name that is provided.
5. Enter a **unique host name**. The host name is used to access your application, for example: `server.mybluemix.net`.
6. Click **Create**. After your app is created, you can deploy by using a toolchain or you can continue to build, and deploy your project from the [command line](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Step 2. Deploying with the dashboard
{: #deploy-go}

1. From the **App details** page, click **Deploy**.
2. Set up your deployment target according to the instructions for the method you choose:
  * **Deploy to [IBM Kubernetes Service](/docs/apps/deploying?topic=creating-apps-containers-kube#containers)**. This option creates a cluster of hosts, called worker nodes, to deploy and manage highly available application containers. You can create a cluster or deploy to an existing cluster.
  * **Deploy to Cloud Foundry**. This option deploys your cloud-native app without you needing to manage the underlying infrastructure. If your account has access to {{site.data.keyword.cfee_full_notm}}, you can select a deployer type of either **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)** or **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)**, which you can use to create and manage isolated environments for hosting Cloud Foundry applications exclusively for your enterprise.
  * **Deploy to a [Virtual Server](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy)**. This option provisions a virtual server instance, loads an image that includes your app, creates a DevOps toolchain, and initiates the first deployment cycle for you.

3. Select your options, and then click **Create**. A toolchain is created for you and deploys your app automatically.

## Step 3. Adding a service (Optional)
{: #add-resource-go}

You can quickly add services like security or storage to your Go app from within the dashboard.

1. Return to your Go app in the [{{site.data.keyword.cloud_notm}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
2. Click **Add service**, select the category of the service you want to add, click **Next**, then choose your service. {{site.data.keyword.cloud_notm}} {{site.data.keyword.dev_console}} creates the service for you based on the selected plan. If you previously created the service that you plan to use, choose the **Existing** category.
3. After the service is created, click **Download code** to regenerate the project with the SDK that connects to your service.

## Step 4. Testing and accessing your Go app locally
{: #run_local-go}

You can use the {{site.data.keyword.dev_cli_long}} to work with your Go app locally by using `ibmcloud dev` commands.

1. Use the `ibmcloud dev build` command to build your application.
2. Use the `ibmcloud dev run` command to run the application locally. Your application is run in the Docker containers on your local system. You can test your application in a browser by accessing `http://localhost:8080`.
3. A health check endpoint is available at `http://localhost:8080/health`.
4. You can access metrics at `http://localhost:3000/metrics`.

To learn more about the {{site.data.keyword.dev_cli_notm}}, see the complete [CLI documentation](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Exploring alternative deployment methods
{: #alt-deploy-go notoc}

Learn how to deploy your application under the Deployments tab [here](/docs/go?topic=go-deploy_apps#deploy_apps). 

To expand your Go application, continue checking out the topics in the Go programming guide.
