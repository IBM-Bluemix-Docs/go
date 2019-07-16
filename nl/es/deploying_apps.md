---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

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

# Despliegue e integración de apps Go
{: #go-deploy-apps}

Puede desplegar las aplicaciones Go con una cadena de herramientas o utilizando la interfaz de línea de mandatos. Una cadena de herramientas es un conjunto de integraciones de herramientas para ayudar a compilar y automatizar el despliegue de apps. Para obtener más información sobre las cadenas de herramientas y cómo funcionan, consulte [Entrega continua](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started). Encuentre información útil sobre distintas metodologías de despliegue para apps Go como CLI, Kubernetes, contenedores, VSI y la nube privada.

## Despliegue en un clúster de Kubernetes
{: #deploy_kube-go}

Puede aprender a utilizar el servicio {{site.data.keyword.cloud_notm}} Kubernetes para desplegar una app Go contenerizada. Consulte los [pasos de la guía de aprendizaje](/docs/containers?topic=containers-cs_cluster_tutorial) para obtener más información sobre la configuración de un clúster de Kubernetes en {{site.data.keyword.cloud_notm}}.

Blogs relacionados que utilizan la CLI:
* [Despliegue en Kubernetes con la CLI de {{site.data.keyword.dev_cli_long}}](https://www.ibm.com/blogs/cloud-archive/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").
* [Despliegue en IBM Cloud Private con la CLI de {{site.data.keyword.dev_cli_short}}](https://www.ibm.com/cloud/blog/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").

## Despliegue en servicios de contenedor con la CLI
{: #go-deploy-container}

Utilice el mandato `ibmcloud dev deploy` para desplegar en {{site.data.keyword.cloud_notm}}. 

Para desplegar en IBM Container Services en {{site.data.keyword.cloud_notm}}, utilice el siguiente mandato:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Para obtener más información sobre los mandatos `ibmcloud dev`, consulte [Desarrollo y despliegue de sus apps](/docs/cli?topic=cloud-cli-getting-started).

## Despliegue en Cloud Foundry
{: #go-deploy-cf}

Esta opción despliega la app nativa de la nube sin necesidad de gestionar la infraestructura subyacente.

Si tiene intención de desplegar la app en [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about), debe [preparar la cuenta de {{site.data.keyword.cloud_notm}}](/docs/cloud-foundry?topic=cloud-foundry-prepare).

Si la cuenta tiene acceso a {{site.data.keyword.cfee_full_notm}}, puede seleccionar el tipo de desplegador de **[nube pública](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** o de **[entorno de empresa](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**, que puede utilizar para crear y gestionar entornos aislados para alojar apps de Cloud Foundry exclusivamente para su empresa.

## Despliegue en un servidor virtual
{: #go-vsi-deploy}

Despliegue las apps de {{site.data.keyword.cloud}} App Service en instancias del servidor virtual para permitir que la plataforma y las actividades de desarrollador de la infraestructura funcionen juntas y aumenten el control y la flexibilidad de la app.

Una instancia de servidor virtual ofrece una mejor transparencia, previsibilidad y automatización para todos los tipos de carga de trabajo en comparación con otras configuraciones. Combínela con un servidor nativo para crear combinaciones de carga de trabajo exclusivas. Por ejemplo, puede crear lógica de base de datos de alto rendimiento o aprendizaje automático con configuraciones nativas y GPU que se ejecuten en un sistema operativo Debian basado en Linux.

  El despliegue de VSI está disponible para algunos kits de inicio. Para utilizar esta característica, vaya al
[panel de control de {{site.data.keyword.cloud_notm}}](https://{DomainName}) y pulse **Crear una app** en el mosaico **Apps**.
  {: note}

Para obtener más información, consulte [Despliegue en un servidor virtual](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server).

