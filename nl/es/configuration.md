---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Configuración del entorno Go
{: #configure-go-env}

Las directrices estandarizadas están disponibles para el desarrollo de aplicaciones Go que ayudan a mantener una portabilidad coherente. Entre las consideraciones que se deben realizar se incluyen la gestión de credenciales, el ahorro de datos, el almacenamiento de datos y la publicación de contenido en la nube. Mediante la implementación de principios nativos en la nube, una app Go puede moverse de un entorno a otro fácilmente. Por ejemplo, desde prueba a producción, sin cambiar el código ni utilizar vías de acceso de código no probadas.

Tanto si necesita añadir soporte para la nube a las apps Go existentes como crear apps Go con los Kits de inicio, el objetivo es proporcionar portabilidad para utilizarlas en cualquier plataforma de desarrollo.

## Adición del soporte de Cloud a apps Go existentes
{: #go-add-cloud-support}

El módulo [`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") agrega variables de entorno procedentes de diversos proveedores de nube, como CloudFoundry y Kubernetes, para que la app no dependa del entorno.

### Instalación del módulo `ibm-cloud-env-golang`
{: #go-install-env-module}

1. Instale el módulo `ibm-cloud-env-golang` con el siguiente mandato:
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. Inicialice el módulo en el código haciendo referencia al archivo `mappings.json`:
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Puesto que GO no ofrece soporte a los parámetros predeterminados, no se proporciona un archivo `mappings.json` predeterminado. Si la vía de acceso del archivo de correlaciones no se especifica en `IBMCloudEnv.Initialize()`, se registra un error. 
  {: tip}

  Archivo `mappings.json` de ejemplo:
  ```javascript
  {
      "service1-credentials": {
          "searchPatterns": [
              "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name",
              "env:my-service1-credentials",
              "file:/localdev/my-service1-credentials.json"
          ]
      },
      "service2-username": {
         "searchPatterns":[
              "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### Recuperación de las credenciales de servicio
{: #go-get-creds}

Recupere los valores de la app utilizando los mandatos siguientes.

1. Recuperar la variable `service1credentials`:
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. Recuperar la variable `service2username`:
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

Ahora la app se puede implementar en cualquier entorno de ejecución abstrayendo las diferencias que se introducen desde distintos proveedores de cálculo de nube.

### Filtrado de los valores de etiquetas y códigos
{: #go-filter-creds}

Puede filtrar las credenciales generadas por el módulo en función de los códigos y las etiquetas de servicio, tal como se muestra en el ejemplo siguiente:
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // devuelve un Json con credenciales para la etiqueta del servicio especificado
```
{: codeblock}

## Utilización del gestor de configuración de Go desde apps del kit de inicio
{: #go-config-manager}

Las apps Go que se crean con [kits de inicio](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") incluyen de forma automática credenciales y configuraciones necesarias para su ejecución en muchos destinos de despliegue en la nube, como
[Kubernetes](/docs/containers?topic=containers-getting-started), [Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf), [{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about), [un servidor virtual (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) o
[{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started).

  El despliegue de VSI está disponible para algunos kits de inicio. Para utilizar esta característica, vaya al
[panel de control de {{site.data.keyword.cloud_notm}}](https://{DomainName}) y pulse **Crear una app** en el mosaico **Apps**.
  {: note}

### Comprensión de las credenciales de servicio
{: #go-credentials-config}

La información de configuración de app de servicios se almacena en el archivo `localdev-config.json` en el directorio `/server/config`. El archivo se encuentra en el directorio `.gitignore` para evitar que se almacene información confidencial en Git. La información de conexión para cualquier servicio configurado que se ejecute localmente, como el nombre de usuario, la contraseña y el nombre de host, se almacena en este archivo.

La app utiliza el gestor de configuración para leer la información de conexión y configuración del entorno y este archivo. Utiliza un `mappings.json` personalizado, que se encuentra en el directorio `server/config`, para comunicar dónde se pueden encontrar las credenciales para cada servicio.

Si la app se ejecuta localmente, se puede conectar a los servicios de {{site.data.keyword.cloud_notm}} utilizando credenciales desenlazadas que se leen desde el archivo `mappings.json`. Pero, si tiene que crear credenciales desenlazadas, puede hacerlo desde la consola web de {{site.data.keyword.cloud_notm}} (ejemplo) o mediante el mandato de la [CLI de CloudFoundry](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") `cf create-service-key`.

Cuando envía la app a {{site.data.keyword.cloud_notm}}, estos valores ya no se utilizan. En su lugar, la app se conecta automáticamente a los servicios enlazados utilizando variables de entorno. 

* **Cloud Foundry**: Las credenciales de servicio se toman de la variable de entorno `VCAP_SERVICES`.

* **Kubernetes**: Las credenciales de servicio se toman de variables de entorno individuales por servicio.

* **{{site.data.keyword.cloud_notm}} Container Service**: Las credenciales de servicio se toman de las instancias de servidor virtual o {{site.data.keyword.openwhisk}}.

## Pasos siguientes
{: #go-next-steps-config notoc}

`ibm-cloud-env-golang` soporta la búsqueda de valores utilizando cuatro tipos de patrón de búsqueda: `user-provided`, `cloudfoundry`, `env` y `file`. Si desea consultar otros patrones de búsqueda admitidos y ejemplos de patrones de búsqueda, consulte la sección [Uso](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").
