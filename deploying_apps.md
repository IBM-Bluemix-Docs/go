---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-16"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry, go deploy virtual

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Deploying and integrating Go apps
{: #go-deploy-apps}

You can deploy your Go applications with a toolchain or by using the command-line interface. A toolchain is a set of tool integrations to help build and automate app deployment. To understand more about toolchains and how they work, see [Continuous Delivery](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started). Find useful information on different deployment targets for Go apps such as CLI, Kubernetes, containers, VSI, and private cloud.

## Deploying to a Kubernetes cluster
{: #deploy_kube-go}

You can learn how to use {{site.data.keyword.containerlong_notm}} to deploy a containerized Go app. Check out the [tutorial steps](/docs/containers?topic=containers-cs_cluster_tutorial) for more information on setting up a Kubernetes cluster in {{site.data.keyword.cloud_notm}}.

Related blogs that use the CLI:
* [Deploying to Kubernetes with the {{site.data.keyword.dev_cli_long}} CLI](https://www.ibm.com/blogs/cloud-archive/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
* [Deploying to IBM Cloud Private with {{site.data.keyword.dev_cli_short}} CLI](https://www.ibm.com/cloud/blog/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").

## Deploying to container services with the CLI
{: #go-deploy-container}

Use the `ibmcloud dev deploy` command to deploy to {{site.data.keyword.cloud_notm}}. 

To deploy to containers in {{site.data.keyword.containerlong_notm}}, use the following command:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

For more information about `ibmcloud dev` commands, see [Developing and deploying your apps](/docs/cli?topic=cloud-cli-getting-started).

## Deploying to Cloud Foundry
{: #go-deploy-cf}

This option deploys your cloud-native app without you needing to manage the underlying infrastructure.

If you plan to deploy your app to [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about), you must [prepare your {{site.data.keyword.cloud_notm}} account](/docs/cloud-foundry?topic=cloud-foundry-prepare).

If your account has access to {{site.data.keyword.cfee_full_notm}}, you can select a deployer type of either [Public Cloud or Enterprise Environment](/docs/cloud-foundry?topic=cloud-foundry-what-is-cloud-foundry#ibmcf-offerings), which you can use to create and manage isolated environments for hosting Cloud Foundry apps exclusively for your enterprise.

## Deploying to a virtual server
{: #go-vsi-deploy}

Deploy {{site.data.keyword.cloud}} App Service apps into virtual server instances to enable your platform and infrastructure developer activities to work together and increase app control and flexibility.

A virtual server instance offers better transparency, predictability, and automation for all workload types when compared to other configurations. Combine it with a bare metal server to create unique workload combinations. For example, you can create high-performance database logic or machine learning with bare metal and GPU configurations that run a Debian Linux-based operating system.

  VSI deployment is available for some starter kits. To use this feature, go to the [{{site.data.keyword.cloud_notm}} dashboard](https://{DomainName}), and click **Create an app** in the **Apps** tile.
  {: note}

For more information, see [Deploying to a virtual server](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server).

