---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: fault tolerance go, hystrix go, add fault tolerance, prometheus go, debug go apps

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuración de la tolerancia al error en apps Go
{: #fault-tolerance}

La tolerancia al error permite que una aplicación continúe ejecutándose si un componente falla o deja de responder. Puede añadir la tolerancia a errores a una aplicación Go existente o habilitar estas características desde una aplicación Go generada. Esta guía de aprendizaje se centra en el uso del [paquete de Hystrix](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") para añadir soporte de tolerancia de errores a una aplicación Go.

## Adición de la tolerancia a errores a una app Go existente
{: #add-fault-tolerance}

En la misma ubicación que el archivo `Gopkg.toml` de la aplicación Go, escriba los siguientes mandatos para añadir los paquetes necesarios a la lista de dependencias:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

Para utilizar hystrix, es necesario añadir una función de middleware:
```go
func HystrixHandler(command string) gin.HandlerFunc {
  return func(c *gin.Context) {
    hystrix.Do(command, func() error {
      c.Next()
      return nil
    }, func(err error) error {
      //Gestionar errores
      return err
    })
  }
}
``` 
{: codeblock}

El manejo de fallos puede cambiar en función del tipo de aplicación Go que se está utilizando. En las apps web, un error redirecciona a una página 500 mientras los microservicios devuelven un objeto JSON que especifica el error.

Este middleware toma una serie que es un mandato configurado. La configuración de un mandato debe realizarse en la función `main` de la manera siguiente:
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Después de que se haya configurado un mandato de Hystrix, el middleware se puede añadir al direccionador con la sentencia siguiente:
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Exposición de métricas Hystrix en Prometheus (opcional)
{: #hystrix-optional}

Antes de añadir Hystrix a Prometheus, la app debe estar configurada con las métricas de aplicación. Puede seguir los pasos del tema [Utilización de métricas de aplicación con apps Go](/docs/go/appmetrics.html) para añadir el soporte de appmetrics.

Hystrix proporciona a los usuarios la capacidad de tomar datos de métricas y exponerlos a un recopilador de métricas. Para poder exponer Hystrix a Prometheus, también es necesario añadir el paquete metric_collector:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

Además de `metric_collector`, es necesario añadir un archivo adicional, `prometheus_collector.go`, a la aplicación Go. Encontrará este archivo [aquí](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go). El archivo debe añadirse al paquete `plugins`.

Son necesarias dos importaciones adicionales:
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

En la función `main`, añada las sentencias siguientes:
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

La ejecución del proyecto y el paso a la ruta `/metrics` muestra las métricas proporcionadas por Hystrix.
