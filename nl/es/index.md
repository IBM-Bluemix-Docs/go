---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-19"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Guía de aprendizaje de iniciación
{: #getting-started}

En la siguiente guía de aprendizaje se indican los pasos para crear y desplegar una app Go utilizando las herramientas proporcionadas por {{site.data.keyword.cloud_notm}}. Puede utilizar [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) en la línea de mandatos o [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") basado en web, tal como se muestra en los siguientes pasos de la guía de aprendizaje. Al utilizar cualquiera de estos métodos, puede generar una aplicación Go lista para producción en tan solo unos minutos.

## Paso 1. Creación de una app Go personalizada en el panel de control
{: #create-go-app}

1. Inicie una sesión en una cuenta de {{site.data.keyword.cloud_notm}} para crear una app. Si no tiene una cuenta, puede [registrarse para una cuenta gratuita](https://{DomainName}/registration){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").
2. En la página [Kits de inicio](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") de la {{site.data.keyword.dev_console}}, elija una de las siguientes opciones:
 * Seleccione un kit de inicio que esté escrito en `Go` y pulse **Crear app** en la página **Detalles del kit de inicio**.
 * Seleccione una app de inicio en blanco y pulse **Crear app**.
3. Especifique un nombre para la app o utilice el nombre genérico de app que se proporciona.
4. Asegúrese de que la plataforma seleccionada sea **Go** y pulse **Crear**. Una vez creada la app, puede añadir servicios y desplegarla utilizando una cadena de herramientas o bien puede continuar para compilar y desplegar el proyecto desde la [línea de mandatos](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Paso 2. Adición de {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

Ahora puede añadir servicios {{site.data.keyword.watson}} a la aplicación Go. En esta guía de aprendizaje añadirá el servicio {{site.data.keyword.texttospeechshort}} a la app Go, que convierte entrada verbal en texto mediante una API de la nube.

1. En la página **Detalles de la app**, pulse **Añadir servicio**.
2. Seleccione **AI** y pulse **Siguiente**.
3. Seleccione **{{site.data.keyword.texttospeechshort}}** y pulse **Siguiente**.
4. Pulse **Crear**.

Cuando este servicio se haya añadido a la aplicación, verá que la dependencia [SDK de Watson Developer Cloud Go](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") se ha añadido a `Gopkg.toml` y que dispone de un código de instrumentación sencillo para el servicio en el directorio `services/`. También se incluye información de configuración para acceder a las credenciales del servicio en los respectivos entornos.

Para descargar el código, pulse **Descargar código** en la página **Detalles de la app**. El código descargado viene con el [SDK de Watson Developer Cloud Go](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo"), así como un código de inicialización básico.

## Paso 3. Despliegue de la app desde la consola
{: #deploy-go}

1. En la página **Detalles de la app**, pulse **Configurar entrega continua**.
2. Configure el destino del despliegue de acuerdo con las instrucciones correspondientes al método que elija:
  * **Despliegue en el [servicio IBM Kubernetes](/docs/apps/deploying?topic=creating-apps-containers-kube)**. Esta opción crea un clúster de hosts, denominado nodos trabajadores, para desplegar y gestionar contenedores de aplicaciones de alta disponibilidad. Puede crear un clúster o desplegar en un clúster existente.
  * **Desplegar en Cloud Foundry**. Esta opción despliega la app nativa de la nube sin necesidad de gestionar la infraestructura subyacente. Si la cuenta tiene acceso a {{site.data.keyword.cfee_full_notm}}, puede seleccionar el tipo de desplegador de **[nube pública](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** o de **[entorno de empresa](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**, que puede utilizar para crear y gestionar entornos aislados para alojar aplicaciones de Cloud Foundry exclusivamente para su empresa.
  * **Desplegar en un [servidor virtual](/docs/apps?topic=creating-apps-vsi-deploy)**. Esta opción proporciona una instancia de servidor virtual, carga una imagen que incluye la app, crea una cadena de herramientas DevOps e inicia automáticamente el primer ciclo de despliegue.

3. Después de configurar el destino del despliegue, pulse **Siguiente**.
4. Seleccione sus opciones de configuración y pulse **Crear**. Se crea automáticamente una cadena de herramientas y la app se despliega.

## Paso 4. Comprobación y acceso a la app Go localmente
{: #run_local-go}

En la página **Detalles de la app**, puede:
* Ver el repositorio pulsando **Ver repositorio**.
* Ver y modificar su cadena de herramientas pulsando **Ver cadena de herramientas**.

Puede utilizar {{site.data.keyword.dev_cli_long}} para trabajar con la app Go localmente utilizando los mandatos `ibmcloud dev`. Estas herramientas le ayudan a realizar iteraciones locales rápidas y a enviar sus cambios a la nube.

1. Utilice el mandato `ibmcloud dev build` para crear la aplicación.
2. Utilice el mandato `ibmcloud dev run` para ejecutar la aplicación localmente. La aplicación se ejecuta en los contenedores de Docker en el sistema local. Puede probar la aplicación en un navegador accediendo a `http://localhost:8080`.
3. Un punto final de comprobación de estado está disponible en `http://localhost:8080/health`.
4. Puede acceder a las métricas en `http://localhost:3000/metrics`.

Para obtener más información sobre {{site.data.keyword.dev_cli_notm}}, consulte la [documentación de la CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) completa.

Ahora está listo para compilar su aplicación con el servicio {{site.data.keyword.texttospeechshort}}.

## Exploración de métodos de despliegue alternativos
{: #alt-deploy-go notoc}

Obtenga más información sobre cómo desplegar la aplicación en el [separador Despliegues](/docs/go?topic=go-go-deploy-apps).

Para ampliar su aplicación Go, siga consultando los temas de la guía de programación de Go.
