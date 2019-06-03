---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-30"

keywords: how to log in go, go logging, debug go apps, go troubleshooting, logrus go, go stdout

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Registro en Go
{: #logging-golang}

Los mensajes de registro son series con información contextual sobre el estado y la actividad del microservicio en el momento en que se realiza la entrada de registro. Los registros son necesarios para diagnosticar cómo y por qué fallan los servicios, y desempeñan un rol de soporte a las [métricas](/docs/go?topic=go-go-appmetrics) en la supervisión del estado de la aplicación.

Dada la naturaleza transitoria de los procesos en entornos de nube, los registros deben recopilarse y enviarse a otro lugar, normalmente a una ubicación centralizada para su análisis. La forma más coherente de iniciar sesión en entornos de nube es enviar entradas de registro a la salida estándar y a las secuencias de error, que deja que la infraestructura maneje el resto.

## Adición de soporte para Logrus en la app Go
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") es una infraestructura de registro muy utilizada para Go y proporciona muchas ventajas nativas, como por ejemplo: 
 * Registro en `stdout` o `stderr`
 * Diversas opciones de adición
 * Diseño y patrones de mensajes de registro configurables
 * Utilización de niveles de registro para distintas categorías de registro

1. Para utilizar Logrus, ejecute el siguiente mandato en el directorio raíz de la aplicación, que instala el paquete y lo añade al archivo `Gopkg.toml`.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. Para utilizarlo en la aplicación, añada las siguientes líneas de código:
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Salida Stdout**:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

Para obtener más información sobre cómo personalizar los mensajes de registro con agregadores, niveles de registro y detalles de configuración, consulte la [documentación de Logrus](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") oficial.

## Pasos siguientes
{: #go-logging-next notoc}

Obtenga más información sobre la visualización de los registros en cada uno de los destinos de despliegue:
* [Registros de Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
* [Registros de Cloud Foundry](/docs/cli/reference/bluemix_cli?topic=cloud-cli-ibmcloud_cli#ibmcloud_app_logs)
* [Cloud Foundry Enterprise Environment - Auditoría y registro](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [Registros y supervisión de {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#openwhisk_logs)

Más información sobre el uso de un agregador de registros:
* [Análisis de registros de {{site.data.keyword.cloud_notm}}](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [Pila de ELK de {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")
