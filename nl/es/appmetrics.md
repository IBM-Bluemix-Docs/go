---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: prometheus go, application metrics go, view metrics go app, add metrics go, promhttp go, autoscaling go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilización de métricas de aplicación con apps Go
{: #go-appmetrics}

Las métricas de aplicación son importantes para supervisar el rendimiento de la aplicación. Tener una vista en directo de métricas como CPU, memoria, latencia y métricas HTTP es esencial para asegurarse de que la aplicación se ejecuta de forma efectiva a lo largo del tiempo. Puede utilizar un servicio de nube como el [escalado automático](/docs/services/Auto-Scaling?topic=services/Auto-Scaling-get-started#get-started) de Cloud Foundry, que se basa en métricas para escalar dinámicamente las instancias para que coincidan con la carga de trabajo actual. Con el uso de métricas de aplicación, estará informado forma precisa para saber cuando se deben aumentar, reducir o borrar las instancias que ya no se necesitan para mantener los costes bajos.

Las métricas de aplicación se capturan como datos de serie temporal. La agregación y visualización de métricas capturadas puede ayudar a identificar problemas de rendimiento comunes como, por ejemplo:

* Tiempos de respuesta HTTP lentos en algunas o en todas las rutas
* Rendimiento bajo en la aplicación
* Aumentos considerables en la demanda que provocan la desaceleración
* Uso de CPU mayor del esperado para el nivel de rendimiento/carga
* Uso de memoria alto o creciente (fuga de memoria potencial)

## Adición de métricas de aplicación a una aplicación Go existente
{: #go-add-appmetrics-existing}

Para añadir una supervisión de rendimiento a la aplicación Go, puede utilizar la agregación completa de métricas que proporciona el paquete `promhttp`.

El paquete `promhttp` tiene varios puntos de extensión, que incluyen a [configuración Prometheus](https://github.com/prometheus/client_golang){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").

1. Por ejemplo, utilice la siguiente aplicación sencilla "Hello World" Go + Gin:
    ```go
    // imports above
    func main() {
        router := gin.Default()
        router.GET("/", func(c *gin.Context) {
            c.String(http.StatusOK, "Hello, World!")
        }
        router.Run(":3000")
    }
    ```
    {: codeblock}

2. Obtenga el paquete con el mandato siguiente:
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. Añada `promhttp` a las importaciones:
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  También se debe añadir una ruta de métrica adicional al direccionador.

  Ahora, el código revisado es similar al siguiente ejemplo:
  ```go
  // imports above
  func main() {
      router := gin.Default()
      router.GET("/", func(c *gin.Context) {
          c.String(http.StatusOK, "Hello, World!")
      }
      router.GET("/metrics", gin.WrapH(promhttp.Handler()))
      router.Run(":3000")
  }
  ```
  {: codeblock}

## Utilización de métricas de aplicación en kits de inicio
{: #go-starterkits-appmetrics}

Las aplicaciones Go del servidor que se crean a partir de kits de inicio se ofrecen automáticamente con el [punto final Prometheus](https://prometheus.io/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") bajo `http://<hostname>:<port>/metrics`. El código para este punto final se encuentra en `server.go`.

Para obtener más información, consulte el apartado sobre [Repositorio GitHub para Prometheus](https://github.com/prometheus/client_golang/){: new_window} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo").
