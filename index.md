---

copyright:
  years: 2018, 2019
lastupdated: "2019-10-29"

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

The following tutorial walks you through the steps to create and deploy a Go application by using {{site.data.keyword.cloud_notm}} provided tools. You can use the [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-getting-started) on the command line or the web-based [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") as shown in the following tutorial steps. By using either of these methods, you can generate a production-ready Go app in just minutes.

## Step 1. Creating a custom Go app in the dashboard
{: #create-go-app}

1. Log in to an {{site.data.keyword.cloud_notm}} account to create an app. If you don't have an account, you can [register for a free account](https://{DomainName}/registration){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
2. From the [Starter Kits](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") page in the {{site.data.keyword.dev_console}}, do one of the following options:
 * Select a starter kit that is written in `Go`, and then click **Create app** on the **Starter kit details** page.
 * Select the blank starter app, and click **Create App**.
3. Provide a name for your app, or use the generic app name that is provided.
4. Ensure that **Go** is selected as the platform, and then click **Create**. After your app is created, you can add services and then deploy it by using a toolchain, or you can continue to build and deploy your project from the [command line](/docs/cli?topic=cloud-cli-getting-started).

## Step 2. Adding {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

You can now add {{site.data.keyword.watson}} services to your Go app. For this tutorial, add the {{site.data.keyword.texttospeechshort}} service to your Go app, which takes verbal input and converts it to text by using a cloud API.

1. From the **App details** page, click **Add service**.
2. Select **AI**, and click **Next**.
3. Select **{{site.data.keyword.texttospeechshort}}**, and click **Next**.
4. Click **Create**.

You can see that the [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") dependency is added to the `Gopkg.toml` file, and simple instrumentation code is available for the service in the `services/` directory. Additionally, configuration information for accessing the service credentials in the respective environments is included.

To download the code, click **Download code** on the **App details** page. The downloaded code comes with the [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") included, and some basic initialization code.

## Step 3. Deploying your app from the console
{: #deploy-go}

1. From the **App details** page, click **Configure continuous delivery**.
2. Set up your deployment target according to the instructions for the method you choose:
  * **Deploy to [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-app)**. This option creates a cluster of hosts, called worker nodes, to deploy and manage highly available app containers. You can create a cluster or deploy to an existing cluster.
  * **Deploy to Cloud Foundry**. This option deploys your cloud-native app without you needing to manage the underlying infrastructure. If your account has access to {{site.data.keyword.cfee_full_notm}}, you can select a deployer type of either **[Public Cloud or Enterprise Environment](/docs/cloud-foundry?topic=cloud-foundry-what-is-cloud-foundry#ibmcf-offerings)**. You can use the deployer type to create and manage isolated environments for hosting Cloud Foundry apps exclusively for your enterprise.
  * **Deploy to a [Virtual Server](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)**. This option provisions a virtual server instance, loads an image that includes your app, creates a DevOps toolchain, and initiates the deployment cycle for you.

3. After you set up your deployment target, click **Next**.
4. Select your configuration options, and then click **Create**. A toolchain is created for you, and your app is automatically deployed.

## Step 4. Testing and accessing your Go app locally
{: #run_local-go}

On the **App details** page, you can:
* View your repo by clicking **View repo**.
* View and modify your toolchain by clicking **View toolchain**.

You can use the {{site.data.keyword.dev_cli_long}} to work with your Go app locally by using `ibmcloud dev` commands. These tools help you quickly iterate locally and push your changes to the cloud.

1. Use the `ibmcloud dev build` command to build your app.
2. Use the `ibmcloud dev run` command to run the app locally. Your app is run in the Docker containers on your local system. You can test your app in a browser by accessing `http://localhost:8080`.
3. A health check endpoint is available at `http://localhost:8080/health`.

To learn more about the {{site.data.keyword.dev_cli_notm}}, see the complete [CLI documentation](/docs/cli?topic=cloud-cli-getting-started).

You are now ready to build out your app with the {{site.data.keyword.texttospeechshort}} service!

## Exploring alternative deployment methods
{: #alt-deploy-go notoc}

Learn how to deploy your app under the [Deployments tab](/docs/go?topic=go-go-deploy-apps).

To expand your Go app, continue checking out the topics in the Go programming guide.
