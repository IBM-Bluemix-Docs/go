---

copyright:
  years: 2018, 2021
lastupdated: "2021-02-10"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit, go tutorial
content-type: tutorial
services: go
account-plan: lite
completion-time: 30m
subcollection: go

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}
{:step: data-tutorial-type='step'}

# Getting started tutorial
{: #getting-started}
{: toc-content-type="tutorial"} 
{: toc-services=""} 
{: toc-completion-time="30m"}

The following tutorial walks you through the steps to create and deploy a Go application by using {{site.data.keyword.cloud}} provided tools. You can use the [{{site.data.keyword.dev_cli_short}} (`ibmcloud dev`) commands](/docs/cli?topic=cli-getting-started) on the command line or the web-based [{{site.data.keyword.cloud_notm}} App Development console](https://{DomainName}/developer/appservice/dashboard){: external} as shown in the following tutorial steps. By using either of these methods, you can generate a production-ready Go app in just minutes.
{: shortdesc}

## Before you begin
{: #prereq-go-app}

* Depending on your [{{site.data.keyword.cloud}} account type](https://{DomainName}/registration), access to certain resources might be limited or constrained. Depending on your plan limits, certain capabilities that are required by some toolchains might not be available. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started).
* Install the [{{site.data.keyword.cloud_notm}} Command Line Interface (CLI)](/docs/cli?topic=cli-getting-started), which includes the {{site.data.keyword.dev_cli_short}} (`ibmcloud dev`) commands.
* Create a Docker account, run the Docker app, and sign in. Docker is installed as part of the developer tools. Docker must be running for the build commands to work.
* All of your `go` dependencies are listed in `go.mod`.

## Creating a custom Go app
{: #create-go-app}
{: step}

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you don't have an account, you can [register for a free account](https://{DomainName}/registration){: external}. For more information, see [Setting up your IBM Cloud account](/docs/account?topic=account-account-getting-started).
2. Go to the [{{site.data.keyword.cloud_notm}} App Development console](https://{DomainName}/developer/appservice/starter-kits){: external}, and select a starter kit that is written in `Go`, or select the **Create App** tile to use a blank starter kit. Then, select the **Create** tab.
3. Name your app, and select a resource group.
4. Optional. Provide tags to classify your app. For more information, see [Working with tags](/docs/account?topic=account-tag).
5. Ensure that **Go** is selected as the platform, and then click **Create**. After your app is created, you can add services and then deploy it by using a toolchain, or you can continue to build and deploy your project from the [command line](/docs/cli?topic=cli-getting-started).

## Adding {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}
{: step}

You can now add {{site.data.keyword.watson}} services to your Go app. For this tutorial, add the {{site.data.keyword.texttospeechshort}} service to your Go app, which takes verbal input and converts it to text by using a cloud API.

1. From the App details page, click **Create service**.
2. Select **AI**, and click **Next**.
3. Select **{{site.data.keyword.texttospeechshort}}**, and click **Next**.
4. Select the Lite pricing plan, and click **Create**.

You can see that the [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: external} dependency is added to the `go.mod` file, and simple instrumentation code is available for the service in the `/resources` directory. Additionally, configuration information for accessing the service credentials in the respective environments is included.

To download the code, click **Download code** on the App details page. The downloaded code comes with the [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: external} included, and some basic initialization code.

## Deploying your app from the console
{: #deploy-go}
{: step}

To deploy your app, you must select your deployment target and configure continuous delivery. This process automatically creates a toolchain and starts the app deployment.

To deploy your app, complete the following steps:

1. On the **Deployment Automation** card, click **Deploy your app**.
2. On the Deployment Automation page, select a deployment target. Set up your deployment target according to the instructions for the target that you select:
  * **IBM Kubernetes Service**. With this option, you can either create a cluster or [deploy to an existing Kubernetes cluster](/docs/containers?topic=containers-app). If you create a cluster, allow 10 - 20 minutes for the cluster to be provisioned, and then select the region and cluster name.
  * **Red Hat OpenShift on IBM Cloud**. If you have an available [OpenShift cluster](/docs/openshift?topic=openshift-openshift_apps), you can select it from the **Cluster name** list. If you don't have an available cluster, you must create one before you continue. Your cluster creation might take some time to complete. After the cluster state shows **Normal**, the cluster network and load-balancing component take about 10 more minutes to deploy and update the cluster domain that you use for the OpenShift web console and other routes.
  * **Cloud Foundry**. This option deploys your cloud-native app without you needing to manage the underlying infrastructure. For more information, see [Deploying apps to Cloud Foundry Public](/docs/cloud-foundry-public?topic=cloud-foundry-public-deployingapps).
3. Provide a name for your toolchain.
4. Select the region to create your toolchain in, and then select the [resource group](/docs/ContinuousDelivery?topic=ContinuousDelivery-toolchains-iam-security) that provides access to your new toolchain.
5. Click **Create**.

The DevOps toolchain is created automatically, and the deployment process begins.

  You can deploy your app from the command line by running the [**ibmcloud dev deploy**](/docs/cli?topic=cli-idt-cli#deploy) command. For more information, see [Deploying apps with the CLI](/docs/apps?topic=apps-create-deploy-app-cli).
  {: note}

For more information about deploying your app, see [Deploying apps](/docs/apps?topic=apps-deploying-apps).

## Testing and accessing your Go app locally
{: #build-locally-go}
{: step}

You can use the {{site.data.keyword.cloud_notm}} CLI to work with your Go app locally by using `ibmcloud dev` commands. These tools help you quickly iterate locally and push your changes to the cloud.

1. Use the `ibmcloud dev build` command to build your app.
2. Use the `ibmcloud dev run` command to run the app locally. Your app is run in the Docker containers on your local system. You can test your app in a browser by accessing `http://localhost:8080`.
3. A health check endpoint is available at `http://localhost:8080/health`.

To learn more about the {{site.data.keyword.dev_cli_notm}} commands, see the complete [CLI documentation](/docs/cli?topic=cli-getting-started).

You are now ready to build out your app with the {{site.data.keyword.texttospeechshort}} service!

## Next steps
{: #nextsteps-go notoc}

Learn how to deploy your app under the [Deployments tab](/docs/go?topic=go-go-deploy-apps).

To expand your Go app, continue checking out the topics in the Go programming guide.

Check out the [repo for the Go Gin starter kit](https://github.com/IBM/go-gin-app){: #external}!
