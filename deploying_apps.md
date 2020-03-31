---

copyright:
  years: 2018, 2019
lastupdated: "2019-12-19"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Deploying and integrating Go apps
{: #go-deploy-apps}

You can deploy your Go applications with a toolchain or by using the command-line interface. A toolchain is a set of tool integrations to help build and automate app deployment. To understand more about toolchains and how they work, see [Continuous Delivery](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started). Find useful information on different deployment targets for Go apps such as CLI, Kubernetes, Cloud Foundry, and private cloud.

For more information, see [Deploying apps](/docs/apps?topic=creating-apps-deploying-apps).

## Deploying to a Kubernetes cluster
{: #deploy-kube}

[{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started) is an open source platform for managing containerized workloads and services across multiple hosts, and offers management tools for deploying, automating, monitoring, and scaling containerized apps with minimal to no manual intervention. [Learn more](https://www.ibm.com/cloud/learn/kubernetes). When you deploy your app to a Kubernetes cluster, you can select either Helm or [Knative](/docs/containers?topic=containers-serverless-apps-knative) as the deployment type.

## Deploying to an OpenShift cluster
{: #deploy-openshift}

[Red Hat OpenShift on {{site.data.keyword.cloud_notm}}](/docs/openshift?topic=openshift-getting-started) is a Kubernetes container platform that provides a trusted environment to run enterprise workloads. It extends the Kubernetes platform with built-in software to enhance app lifecycle development, operations, and security. With OpenShift, you can consistently deploy your workloads across hybrid cloud providers and environments. [View a comparison between Kubernetes and OpenShift clusters](https://cloud.ibm.com/docs/openshift?topic=openshift-cs_ov#openshift_kubernetes).

## Deploying to Cloud Foundry
{: #deploy-cf}

[{{site.data.keyword.cloud_notm}} Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-getting-started) is the premier industry standard Platform-as-a-Service (PaaS) that ensures fast, easy, and reliable deployment of cloud-native apps. Cloud Foundry ensures that the build and deploy aspects of coding remain carefully coordinated with any attached services — resulting in quick, consistent, and reliable iterating of applications. Cloud Foundry has a Lite plan that allows quick deployments for testing purposes.

## Deploying to container services with the CLI
{: #go-deploy-container}

Use the `ibmcloud dev deploy` command to deploy to {{site.data.keyword.cloud_notm}}. 

To deploy to containers in {{site.data.keyword.containerlong_notm}}, use the following command:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

For more information about `ibmcloud dev` commands, see [Getting started with the {{site.data.keyword.cloud_notm}} CLI and {{site.data.keyword.dev_cli_short}} (`ibmcloud dev`) commands](/docs/cli?topic=cloud-cli-getting-started).

Related blogs that use the CLI:
* [Deploying to Kubernetes with the {{site.data.keyword.dev_cli_short}} commands](https://www.ibm.com/blogs/cloud-archive/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: external}.
* [Deploying to IBM Cloud Private with the {{site.data.keyword.dev_cli_short}} commands](https://www.ibm.com/cloud/blog/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli){: external}.
