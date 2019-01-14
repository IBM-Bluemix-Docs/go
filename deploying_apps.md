---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Deploying and integrating Go apps
{: #deploy_apps}

You can deploy your Go apps with a toolchain or by using the command line interface. A toolchain is a set of tool integrations to help build and automate application deployment. To understand more about toolchains and how they work, see [Continuous Delivery](/docs/services/ContinuousDelivery/index.html). This topic helps you find useful information on different deployment methodologies for Go applications such as CLI, Kubernetes, containers, VSI, and private cloud.

## Deploying to a Kubernetes cluster
{: #deploy_kube}

You can learn how to use {{site.data.keyword.cloud_notm}} Kubernetes Service to deploy a containerized Go app. Check out the [tutorial steps](https://cloud.ibm.com/docs/containers/cs_cluster.html#cs_cluster) for more information on setting up a Kubernetes cluster in {{site.data.keyword.cloud_notm}}.

Related blogs that use the CLI:
* [Deploying to Kubernetes with the IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Deploying to IBM Cloud Private with IBM Cloud Developer Tools CLI](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## Deploying to container services with the CLI
{: #cf-deploy}

Use the `ibmcloud dev deploy` command to deploy to {{site.data.keyword.cloud_notm}}. 

To deploy to IBM Container Services in {{site.data.keyword.cloud_notm}}, use the following command:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

For more information about `ibmcloud dev` commands, see [Developing and deploying your apps](/docs/cli/idt/index.html)

## Deploying to a virtual server
{: #vsi-deploy}

Deploy {{site.data.keyword.cloud}} App Service apps into virtual server instances to enable your platform and infrastructure developer activities to work together and increase app control and flexibility.

A virtual server instance offers better transparency, predictability, and automation for all workload types when compared to other configurations. Combine it with a bare metal server to create unique workload combinations. For example, you can create high-performance database logic or machine learning with bare metal and GPU configurations that run a Debian Linux-based operating system.

For more information, see [Deploying to a virtual server](/docs/apps/vsi-deploy.html).



