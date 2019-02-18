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

# Getting started tutorial
{: #getting-started}

The following tutorial walks you through the steps to create and deploy a Go app by using {{site.data.keyword.cloud_notm}} provided tools. You can use the [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) on the command line or the web-based [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) as shown in the following tutorial steps. By using either of these methods, you can generate a production-ready Go application in just minutes.

## Step 1. Creating a Go app in the dashboard
{: #create-go-app}

1. From the [Starter Kits](https://cloud.ibm.com/developer/appservice/starter-kits) page in the App Service, select a Starter Kit that is written in `Go`. You can also create a blank starter app by clicking **Create app** and selecting `Go` as the language.

    You must be logged in to an {{site.data.keyword.cloud_notm}} account to create an app. If you don't have an account, you can [register for a free account](https://cloud.ibm.com/registration).
    {: tip}

3. Click **Create app**.
4. **Name** your app or use the generic app name that is provided.
5. Enter a **unique host name**. The host name is used to access your application, for example: `server.mybluemix.net`.
6. Click **Create**. After your app is created, you can deploy by using a toolchain or you can continue to build, and deploy your project from the [command line](/docs/cli/index.html#ibmcloud-cli).

## Step 2. Deploying with the dashboard
{: #deploy-go}

1. From the App Details page, click **Deploy to Cloud**.
2. Set up your deployment method according to the instructions for the method you choose:
  * **Deploy to [Kubernetes](/docs/apps/deploying/containers.html#containers)**. This option creates a cluster of hosts, called worker nodes, to deploy and manage highly available application containers. You can create a cluster or deploy to an existing cluster.
  * **Deploy to Cloud Foundry**. This option deploys your cloud-native app without you needing to manage the underlying infrastructure. If your account has access to {{site.data.keyword.cfee_full_notm}}, you can select a deployer type of either **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** or **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)**, which you can use to create and manage isolated environments for hosting Cloud Foundry applications exclusively for your enterprise.
  * **Deploy to a [Virtual Server](/docs/apps/vsi-deploy.html#vsi-deploy)**. This option provisions a virtual server instance, loads an image that includes your app, creates a DevOps toolchain, and initiates the first deployment cycle for you.

3. Select your options, and then click **Create**. A toolchain is created for you and deploys your app automatically.

## Step 3. Adding a resource (Optional)
{: #add-resource-go}

You can quickly add services like security or storage to your Go app from within the dashboard.

1. Return to your Go app in the [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard).
2. Click **Add Resource**, select the category of the service you want to add, click **Next**, then choose your service. {{site.data.keyword.cloud_notm}} App Service creates the service for you based on the selected plan. If you previously created the service that you plan to use, choose the **Existing** category.
3. After the service is created, click **Download Code** to regenerate the project with the SDK that connects to your service.

## Step 4. Testing and accessing your Go app locally
{: #run_local-go}

You can use the [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) to work with your Go app locally by using `ibmcloud dev` commands.

1. Use the `ibmcloud dev build` command to build your application.
2. Use the `ibmcloud dev run` command to run the application locally. Your application is run in the Docker containers on your local system. You can test your application in a browser by accessing `http://localhost:8080`.
3. A health check endpoint is available at `http://localhost:8080/health`.
4. You can access metrics at `http://localhost:3000/metrics`.

To learn more about the {{site.data.keyword.dev_cli_long}}, see the complete [CLI documentation](/docs/cli/index.html#ibmcloud-cli).

## Exploring alternative deployment methods
{: #deploy_cloud-go notoc}

Learn how to deploy your application under the Deployments tab [here](/docs/go/deploying_apps.html). 

To expand your Go application, continue checking out the topics in the Go programming guide.
