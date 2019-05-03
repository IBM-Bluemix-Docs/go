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

# Guía de aprendizaje de iniciación
{: #getting-started}

En la siguiente guía de aprendizaje se indican los pasos para crear y desplegar una app Go utilizando las herramientas proporcionadas por {{site.data.keyword.cloud_notm}}. Puede utilizar la [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) en la línea de mandatos o [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) basado en web, como se muestra en los siguientes pasos de la guía de aprendizaje. Al utilizar cualquiera de estos métodos, puede generar una aplicación Go lista para producción en tan solo unos minutos.

## Paso 1. Creación de una app Go en el panel de control
{: #create-go-app}

1. En la página [Kits de inicio](https://cloud.ibm.com/developer/appservice/starter-kits) de App Service, seleccione un Kit de inicio escrito en `Go`. También puede crear una app de inicio en blanco pulsando **Crear app** y seleccionando `Go` como lenguaje.

    Debe haber iniciado sesión en una cuenta de {{site.data.keyword.cloud_notm}} para crear una app. Si no tiene una cuenta, puede [registrarse para una cuenta gratuita](https://cloud.ibm.com/registration).
    {: tip}

3. Pulse **Crear app**.
4. **Asigne un nombre** a su app o utilice el nombre de app genérico que se proporciona.
5. Especifique un **nombre de host exclusivo**. El nombre de host se utiliza para acceder a la aplicación, por ejemplo: `server.mybluemix.net`.
6. Pulse **Crear**. Una vez que se haya creado la app, puede desplegarla utilizando una cadena de herramientas o puede seguir compilando y desplegando el proyecto desde la [línea de mandatos](/docs/cli/index.html#ibmcloud-cli).

## Paso 2. Despliegue con el panel de control
{: #deploy-go}

1. En la página Detalles de la app, pulse **Desplegar en la nube**.
2. Configure el método de despliegue de acuerdo con las instrucciones correspondientes al método que elija:
  * **Desplegar en [Kubernetes](/docs/apps/deploying/containers.html#containers)**. Esta opción crea un clúster de hosts, denominado nodos trabajadores, para desplegar y gestionar contenedores de aplicaciones de alta disponibilidad. Puede crear un clúster o desplegar en un clúster existente.
  * **Desplegar en Cloud Foundry**. Esta opción despliega la app nativa de la nube sin necesidad de gestionar la infraestructura subyacente. Si la cuenta tiene acceso a {{site.data.keyword.cfee_full_notm}}, puede seleccionar el tipo de desplegador de **[nube pública](/docs/cloud-foundry-public/about-cf.html#about-cf)** o de **[entorno de empresa](/docs/cloud-foundry-public/cfee.html#cfee)**, que puede utilizar para crear y gestionar entornos aislados para alojar aplicaciones de Cloud Foundry exclusivamente para su empresa.
  * **Desplegar en un [servidor virtual](/docs/apps/vsi-deploy.html#vsi-deploy)**. Esta opción proporciona una instancia de servidor virtual, carga una imagen que incluye la app, crea una cadena de herramientas DevOps e inicia automáticamente el primer ciclo de despliegue.

3. Seleccione sus opciones y pulse **Crear**. Se crea una cadena de herramientas y despliega la app automáticamente.

## Paso 3. Adición de un recurso (opcional)
{: #add-resource-go}

Puede añadir rápidamente servicios como seguridad o almacenamiento a la app Go desde el panel de instrumentos.

1. Vuelva a la app Go en [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard).
2. Pulse **Añadir recurso**, seleccione la categoría del servicio que desea añadir, pulse **Siguiente** y, a continuación, elija el servicio. {{site.data.keyword.cloud_notm}} App Service crea el servicio automáticamente de acuerdo con el plan seleccionado. Si anteriormente ha creado el servicio que tiene previsto utilizar, elija la categoría **Existente**.
3. Cuando se haya creado el servicio, pulse **Descargar código** para volver a generar el proyecto con el SDK que se conecta al servicio.

## Paso 4. Comprobación y acceso a la app Go localmente
{: #run_local-go}

Puede utilizar [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) para trabajar con la app Go localmente utilizando los mandatos `ibmcloud dev`.

1. Utilice el mandato `ibmcloud dev build` para crear la aplicación.
2. Utilice el mandato `ibmcloud dev run` para ejecutar la aplicación localmente. La aplicación se ejecuta en los contenedores de Docker en el sistema local. Puede probar la aplicación en un navegador accediendo a `http://localhost:8080`.
3. Un punto final de comprobación de estado está disponible en `http://localhost:8080/health`.
4. Puede acceder a las métricas en `http://localhost:3000/metrics`.

Para obtener más información sobre {{site.data.keyword.dev_cli_long}}, consulte la [documentación de la CLI](/docs/cli/index.html#ibmcloud-cli) completa.

## Exploración de métodos de despliegue alternativos
{: #deploy_cloud-go notoc}

Obtenga información sobre cómo desplegar la aplicación en el separador Despliegues [aquí](/docs/go/deploying_apps.html). 

Para ampliar su aplicación Go, siga consultando los temas de la guía de programación de Go.
