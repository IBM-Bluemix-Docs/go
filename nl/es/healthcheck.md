---

copyright:
  years: 2018
lastupdated: "2018-10-17"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilización de una comprobación de estado en la app Go
{: #healthcheck}

Las comprobaciones de estado proporcionan un mecanismo simple para determinar si una aplicación del lado del servidor se está comportando correctamente. Los entornos de nube, como [Kubernetes](https://www.ibm.com/cloud/container-service) y [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry), se pueden configurar para sondear los puntos finales de estado de forma periódica para determinar si una instancia del servicio está lista para aceptar tráfico.

## Visión general de la comprobación de estado
{: #overview}

Las comprobaciones de estado proporcionan un mecanismo simple para determinar si una aplicación del lado del servidor se está comportando correctamente. Normalmente se accede a ellas a través de HTTP y utilizan códigos de retorno estándares para indicar el estado UP (activo) o DOWN(inactivo). El valor de retorno de una comprobación de estado es variable, pero típicamente es una respuesta JSON mínima, como `{"status": "UP"}`.

Kubernetes tiene una noción matizada del estado del proceso. Define dos análisis:

- Se utiliza una prueba de _**preparación**_ para indicar si el proceso puede manejar solicitudes (es direccionable).

  Kubernetes no direcciona el trabajo a un contenedor con una prueba de actividad anómala. Una prueba de actividad puede fallar si un servicio no ha terminado de inicializarse, o si está ocupado, sobrecargado o no puede procesar las solicitudes.

- Una prueba de _**actividad**_ se utiliza para indicar si el proceso debe reiniciarse.

  Kubernetes detiene y reinicia un contenedor con una prueba de actividad anómala. Utilice una prueba de actividad para limpiar procesos en un estado no recuperable, por ejemplo, si se ha agotado la memoria o si el contenedor no se ha detenido correctamente después de que un proceso interno haya fallado.

A modo de comparación, Cloud Foundry utiliza un punto final de estado. Si la comprobación falla, el proceso se reinicia, pero si se realiza correctamente, las solicitudes se direccionan a la misma. En este entorno, el punto final se realiza correctamente cuando el proceso está activo. Se ha configurado un retraso inicial para posponer la comprobación de estado hasta que el servicio haya finalizado la inicialización para evitar ciclos de reinicio.

La tabla siguiente proporciona una orientación sobre las respuestas que los puntos finales de estado singulares, de actividad y de preparación van a proporcionar.

| Estado    | Preparación                   | Actividad                   | Estado                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | No es correcta y no se carga       | No es correcta y provoca un reinicio      | No es correcto y provoca un reinicio     |
| Iniciando | 503 - No disponible           | 200 - Correcta                   | Utilizar retraso para evitar la prueba   |
| Activo       | 200 - Bien                    | 200 - Bien                   | 200 - Bien                  |
| Deteniendo | 503 - No disponible           | 200 - Correcta                   | 503 - No disponible         |
| Inactivo     | 503 - No disponible           | 503 - No disponible          | 503 - No disponible         |
| Con errores  | 500 - Error del servidor          | 500 - Error del servidor         | 500 - Error del servidor        |

## Adición de una comprobación de estado a una app de Go existente
{: #add-healthcheck-existing}

Puede añadir una comprobación de actividad y estado mínima a una aplicación `Gin-Gonic` existente mediante la introducción de una nueva ruta, como se muestra en el ejemplo siguiente:
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

Compruebe el estado de la app con un navegador accediendo al punto final `/health`.

Hay más bibliotecas extensivas disponibles, como [`http-healthcheck`](https://github.com/robzienert/http-healthcheck), que permiten la definición de comprobaciones de estado extensibles con soporte para almacenar en memoria caché comprobaciones que se ejecutan en servicios de seguridad. En este caso, probablemente desee separar la prueba de actividad simple en el ejemplo de una comprobación de preparación más detallada y solida creada a partir del paquete de comprobación de estado.

## Acceso al punto final de comprobación de estado en las apps del kit de inicio de Go
{: #healthcheck-starterkit}

De forma predeterminada, al generar una app Go utilizando un kit de inicio, hay disponible un punto final de comprobación de estado básico (no autorizado) en `/health` para comprobar el estado de la app (activa/inactiva).

El siguiente archivo `/routers/health.go` proporciona el código de punto final de comprobación de estado:
```go
package routers

import (
  "github.com/gin-gonic/gin"
)

func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
}
```
{: codeblock}

## Recomendaciones para pruebas de actividad y preparación
{: #recommendations}

Las pruebas de comprobación deben incluir la viabilidad de conexiones a servicios en sentido descendente en el resultado cuando no haya una reserva aceptable si el servicio en sentido descendente no está disponible. Esto no implica llamar a la comprobación de estado que proporciona directamente el servicio en sentido descendente, puesto que la infraestructura realiza la comprobación. En su lugar, considere la posibilidad de verificar el estado de las referencias existentes que tiene la aplicación en los servicios en sentido descendente: puede ser una conexión JMS a WebSphere MQ o un consumidor o productor Kafka inicializado. Si comprueba la viabilidad de referencias internas en servicios en sentido descendente, almacene en memoria caché el resultado para minimizar el impacto que tiene la comprobación de estado en la aplicación.

Una prueba de actividad, por el contrario, tiene en cuenta lo que se comprueba, ya que un error puede provocar una terminación inmediata del proceso. Un punto final HTTP simple que siempre devuelva `{"status": "UP"}` con el código de estado `200` es una elección razonable.

## Configuración de pruebas de actividad y preparación en Kubernetes
{: #config_probes}

Declare las pruebas de actividad y preparación junto con el despliegue de Kubernetes. Ambas pruebas utilizan los mismos parámetros de configuración:

* El kubelet espera a `initialDelaySeconds` antes de la primera prueba.

* El kubelet prueba el servicio cada `periodSeconds` segundos. El valor predeterminado es 1.

* La prueba expira pasados `timeoutSeconds` segundos. El valor mínimo y valor predeterminado es 1.

* La prueba se realiza correctamente si es satisfactoria `successThreshold` veces tras un error. El valor predeterminado y mínimo es 1. El valor debe ser 1 en las pruebas de actividad.

* Cuando se inicia un pod y la prueba falla, Kubernetes intenta reiniciar el pod `failureThreshold` veces y luego abandona. El valor mínimo es 1 y el valor predeterminado es 3.
    - En una prueba de actividad, "abandonar" significa reiniciar el pod.
    - En una prueba de preparación, "abandonar" significa marcar el pod como `Unready` (no preparado).

Para evitar los ciclos de reinicio, establezca `livenessProbe.initialDelaySeconds` para que sea seguro más tiempo del que necesita el servicio para inicializarse. Puede utilizar un valor más corto para `readinessProbe.initialDelaySeconds`, para direccionar solicitudes al servicio en cuanto esté listo.

Un ejemplo de configuración tiene un aspecto similar al siguiente:
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```
{: codeblock}

Para obtener más información, consulte cómo [Configurar pruebas de actividad y comprobación](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).
