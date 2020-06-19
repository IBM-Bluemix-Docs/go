---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-05"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:external: target="_blank" .external}

# Getting started tutorial
{: #getting-started}

The following tutorial walks you through the steps to create and deploy a Go application by using {{site.data.keyword.cloud_notm}} provided tools. You can use the [{{site.data.keyword.dev_cli_short}} (`ibmcloud dev`) commands](/docs/cli?topic=cli-getting-started) on the command line or the web-based [{{site.data.keyword.cloud_notm}} App Development console](https://{DomainName}/developer/appservice/dashboard){: external} as shown in the following tutorial steps. By using either of these methods, you can generate a production-ready Go app in just minutes.

## Step 1. Creating a custom Go app
{: #create-go-app}

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you don't have an account, you can [register for a free account](https://{DomainName}/registration){: external}.
2. Go to the [{{site.data.keyword.cloud_notm}} App Development console](https://{DomainName}/developer/appservice/starter-kits){: external}, and select a starter kit that is written in `Go`, or select the **Create App** tile to use a blank starter kit. Then select the **Create** tab.
3. Name your app, and select a resource group.
4. Optional. Provide tags to classify your app. For more information, see [Working with tags](/docs/resources?topic=resources-tag).
5. Ensure that **Go** is selected as the platform, and then click **Create**. After your app is created, you can add services and then deploy it by using a toolchain, or you can continue to build and deploy your project from the [command line](/docs/cli?topic=cli-getting-started).

## Step 2. Adding {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

You can now add {{site.data.keyword.watson}} services to your Go app. For this tutorial, add the {{site.data.keyword.texttospeechshort}} service to your Go app, which takes verbal input and converts it to text by using a cloud API.

1. From the App details page, click **Create service**.
2. Select **AI**, and click **Next**.
3. Select **{{site.data.keyword.texttospeechshort}}**, and click **Next**.
4. Select the Lite pricing plan, and click **Create**.

You can see that the [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: external} dependency is added to the `Gopkg.toml` file, and simple instrumentation code is available for the service in the `services/` directory. Additionally, configuration information for accessing the service credentials in the respective environments is included.

To download the code, click **Download code** on the App details page. The downloaded code comes with the [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: external} included, and some basic initialization code.

## Step 3. Deploying your app from the console
{: #deploy-go}

To deploy your app, you must select your deployment target and configure continuous delivery. This process automatically creates a toolchain and starts the app deployment.

To deploy your app, complete the following steps:

1. On the **Deployment Automation** card, click **Deploy your app**.
2. On the Deployment Automation page, select a deployment target. Set up your deployment target according to the instructions for the target that you select:
  * **IBM Kubernetes Service**. With this option, you can either create a cluster or [deploy to an existing Kubernetes cluster](/docs/containers?topic=containers-app). If you create a cluster, allow 10 - 20 minutes for the cluster to be provisioned. Select a deployment type of **Helm** or **Knative**, and then select the region and cluster name.
  * **Red Hat OpenShift on IBM Cloud**. If you have an available [OpenShift cluster](/docs/openshift?topic=openshift-openshift_apps), you can select it from the **Cluster name** list. If you don't have an available cluster, you must create one before you continue. Your cluster creation might take some time to complete. After the cluster state shows **Normal**, the cluster network and load-balancing component take about 10 more minutes to deploy and update the cluster domain that you use for the OpenShift web console and other routes.
  * **Cloud Foundry**. This option deploys your cloud-native app without you needing to manage the underlying infrastructure. For more information, see [Deploying apps to Cloud Foundry Public](/docs/cloud-foundry-public?topic=cloud-foundry-public-deployingapps).
3. Provide a name for your toolchain.
4. Select the region to create your toolchain in, and then select the [resource group](/docs/ContinuousDelivery?topic=ContinuousDelivery-toolchains-iam-security) that provides access to your new toolchain.
5. Click **Create**.

The DevOps toolchain is created automatically, and the deployment process begins.

  You can deploy your app from the command line by running the [**ibmcloud dev deploy**](/docs/cli?topic=cli-idt-cli#deploy) command. For more information, see [Deploying apps with the CLI](/docs/apps?topic=apps-create-deploy-app-cli).
  {: note}

For more information about deploying your app, see [Deploying apps](/docs/apps?topic=apps-deploying-apps).

## Step 4. Testing and accessing your Go app locally
{: #run_local-go}

You can use the {{site.data.keyword.cloud_notm}} CLI to work with your Go app locally by using `ibmcloud dev` commands. These tools help you quickly iterate locally and push your changes to the cloud.

1. Use the `ibmcloud dev build` command to build your app.
2. Use the `ibmcloud dev run` command to run the app locally. Your app is run in the Docker containers on your local system. You can test your app in a browser by accessing `http://localhost:8080`.
3. A health check endpoint is available at `http://localhost:8080/health`.

To learn more about the {{site.data.keyword.dev_cli_notm}}, see the complete [CLI documentation](/docs/cli?topic=cli-getting-started).

You are now ready to build out your app with the {{site.data.keyword.texttospeechshort}} service!

## Exploring alternative deployment methods
{: #alt-deploy-go notoc}

Learn how to deploy your app under the [Deployments tab](/docs/go?topic=go-go-deploy-apps).

To expand your Go app, continue checking out the topics in the Go programming guide.
