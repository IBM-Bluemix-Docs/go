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

# Despliegue e integración de apps Go
{: #deploy_apps}

Puede desplegar las apps Go con una cadena de herramientas o utilizando la interfaz de línea de mandatos. Una cadena de herramientas es un conjunto de integraciones de herramientas para ayudar a compilar y automatizar el despliegue de aplicaciones. Para obtener más información sobre las cadenas de herramientas y cómo funcionan, consulte [Entrega continua](/docs/services/ContinuousDelivery/index.html). Este tema le ayuda a encontrar información útil sobre distintas metodologías de despliegue para aplicaciones Go como CLI, Kubernetes, contenedores, VSI y la nube privada.

## Despliegue en un clúster de Kubernetes
{: #deploy_kube}

Puede aprender a utilizar el servicio {{site.data.keyword.cloud_notm}} Kubernetes para desplegar una app Go contenerizada. Consulte los [pasos de la guía de aprendizaje](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_cluster) para obtener más información sobre la configuración de un clúster de Kubernetes en {{site.data.keyword.cloud_notm}}.

Blogs relacionados que utilizan la CLI:
* [Despliegue en Kubernetes con la CLI de IBM Cloud Developer Tools](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Despliegue en una instancia de IBM Cloud privada con la CLI de IBM Cloud Developer Tools](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## Despliegue en servicios de contenedor con la CLI
{: #cf-deploy}

Utilice el mandato `ibmcloud dev deploy` para desplegar en {{site.data.keyword.cloud_notm}}. 

Para desplegar en IBM Container Services en {{site.data.keyword.cloud_notm}}, utilice el siguiente mandato:
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Para obtener más información sobre los mandatos `ibmcloud dev`, consulte [Desarrollo y despliegue de sus apps](/docs/cli/idt/index.html)

## Despliegue en un servidor virtual
{: #vsi-deploy}

Despliegue las apps de {{site.data.keyword.cloud}} App Service en instancias del servidor virtual para permitir que la plataforma y las actividades de desarrollador de la infraestructura funcionen juntas y aumenten el control y la flexibilidad de la app.

Una instancia de servidor virtual ofrece una mejor transparencia, previsibilidad y automatización para todos los tipos de carga de trabajo en comparación con otras configuraciones. Combínela con un servidor nativo para crear combinaciones de carga de trabajo exclusivas. Por ejemplo, puede crear lógica de base de datos de alto rendimiento o aprendizaje automático con configuraciones nativas y GPU que se ejecuten en un sistema operativo Debian basado en Linux.

Para obtener más información, consulte [Despliegue en un servidor virtual](/docs/apps/vsi-deploy.html).


