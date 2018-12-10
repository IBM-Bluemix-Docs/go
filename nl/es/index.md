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

# Guía de aprendizaje de iniciación

En la siguiente guía de aprendizaje se indican los pasos para crear y desplegar una app Go utilizando las herramientas proporcionadas por {{site.data.keyword.cloud_notm}}. Puede utilizar la [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) en la línea de mandatos o [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) basado en web como se muestran en los siguientes pasos de la guía de aprendizaje. Al utilizar cualquiera de estos métodos, puede generar una aplicación Go lista para producción en tan solo unos minutos.

## Paso 1. Creación de una app Go en el panel de control
{: #create_project}

1. En la página [Kits de inicio](https://console.bluemix.net/developer/appservice/starter-kits) de App Service, seleccione un Kit de inicio escrito en `Go`. También puede crear una app de inicio en blanco pulsando **Crear app** y seleccionando `Go` como lenguaje.

    Debe haber iniciado sesión en una cuenta de {{site.data.keyword.cloud_notm}} para crear un proyecto. Si no tiene una cuenta, puede [registrarse para una cuenta gratuita](https://console.bluemix.net/registration).
    {: tip}

3. Pulse **Crear app**.
4. **Asigne un nombre** a su app o utilice el nombre de app genérico que se proporciona.
5. Especifique un **nombre de host exclusivo**. El nombre de host se utiliza para acceder a la aplicación, por ejemplo: `server.mybluemix.net`.
6. Pulse **Crear**. Una vez que se haya creado el proyecto, puede desplegarlo utilizando una cadena de herramientas o puede seguir compilando y desplegando el proyecto desde la [línea de mandatos](/docs/cli/idt/index.html).

## Paso 2. Despliegue con el panel de control
{: #deploy_dashboard}

1. En la página Detalles de la app, pulse **Desplegar en la nube**.
2. A continuación, seleccione uno de los siguientes métodos de despliegue:
    * **Desplegar en Kubernetes**: Debe crear un conjunto de nodos trabajadores. Por ejemplo, puede utilizar máquinas virtuales para desplegar y gestionar contenedores de aplicaciones de alta disponibilidad. Puede crear un clúster o desplegar en un clúster existente.
    * **Desplegar en Cloud Foundry**: No necesita gestionar la infraestructura subyacente.
    * **Desplegar en un servidor virtual (Beta)** -Es necesaria una [cuenta de pago según uso](https://console.bluemix.net/dashboard/ibm-iaas-g1) para acceder a los servidores virtuales.
3. Seleccione sus opciones y pulse **Crear**. Se crea una cadena de herramientas y despliega la app automáticamente.

## Paso 3. Adición de un servicio (opcional)
{: #add_service}

Puede añadir rápidamente servicios como seguridad o almacenamiento a la app Go desde el panel de instrumentos.

1. Vuelva a la app Go en [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard).
2. Pulse **Añadir recurso**, seleccione la categoría del servicio que desea añadir, pulse **Siguiente** y, a continuación, elija el servicio. {{site.data.keyword.cloud_notm}} App Service crea el servicio automáticamente de acuerdo con el plan seleccionado. Si anteriormente ha creado el servicio que tiene previsto utilizar, elija la categoría **Existente**.
3. Cuando se haya creado el servicio, pulse **Descargar código** para volver a generar el proyecto con el SDK que se conecta al servicio.

## Paso 4. Comprobación y acceso a la app Go localmente
{: #run_local}

Puede utilizar [{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) para trabajar con la app Go localmente utilizando los mandatos `ibmcloud dev`.

1. Utilice el mandato `ibmcloud dev build` para crear la aplicación.
2. Utilice el mandato `ibmcloud dev run` para ejecutar la aplicación localmente. La aplicación se ejecuta en los contenedores de Docker en el sistema local. Puede probar la aplicación en un navegador accediendo a `http://localhost:8080`.
3. Un punto final de comprobación de estado está disponible en `http://localhost:8080/health`.
4. Puede acceder a las métricas en `http://localhost:3000/metrics`.

Para obtener más información sobre {{site.data.keyword.dev_cli_long}}, consulte la [documentación de la CLI](/docs/cli/idt/index.html) completa.

## Exploración de métodos de despliegue alternativos
{: #deploy_cloud notoc}

Obtenga información sobre cómo desplegar la aplicación en Cloud Foundry o Kubernetes en el separador Despliegues [aquí](/docs/go/deploying_apps.html). 

Para ampliar su aplicación Go, siga consultando los temas de la guía de programación de Go.
