---

copyright:
  years: 2018
lastupdated: "2018-10-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurando a tolerância a falhas em apps Go
{: #fault-tolerance}

A tolerância a falhas permite que um aplicativo continue em execução se um componente falhar ou se tornar não responsivo. É possível incluir tolerância a falhas em um aplicativo Go existente ou ativar esses recursos por um Aplicativo Go gerado. Este tutorial foca no uso do [pacote Hystrix](https://godoc.org/github.com/afex/hystrix-go/hystrix) para incluir suporte de tolerância a falhas em um aplicativo Go.

## Incluindo tolerância a falhas em um app Go existente
{: #add-fault-tolerance}

No mesmo local que o arquivo `Gopkg.toml` do aplicativo Go, insira os comandos a seguir para incluir os pacotes necessários em sua lista de dependências:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

Para usar o hystrix, uma função de middleware deve ser incluída:
```go
func HystrixHandler(command string) gin.HandlerFunc {
  return func(c *gin.Context) {
    hystrix.Do(command, func() error {
      c.Next()
      return nil
    }, func(err error) error {
      //Handle failures
      return err
    })
  }
}
``` 
{: codeblock}

As falhas de manipulação podem mudar dependendo do tipo de aplicativo Go que está sendo usado. Para aplicativos da web, uma falha redireciona para uma página 500, enquanto os microsserviços retornam um objeto JSON que especifica o erro.

Esse middleware utiliza uma sequência que é um comando configurado. A configuração de um comando deve ser feita na função `main` da seguinte forma:
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Após um comando Hystrix ser configurado, o middleware pode ser incluído no roteador com a instrução a seguir:
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Expondo métricas do Hystrix no Prometheus (opcional)

Antes de incluir o Hystrix no Prometheus, seu app deve ser configurado com as métricas do aplicativo. É possível seguir as etapas no tópico [Usando as métricas do aplicativo com apps Go](/docs/go/appmetrics.html) para incluir o suporte ao appmetrics.

O Hystrix fornece aos usuários a capacidade de obter dados de métricas e expô-los em um coletor de métricas. Para expor Hystrix no Prometheus, o pacote metric_collector também deve ser incluído:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

Além do `metric_collector`, um arquivo adicional, `prometheus_collector.go`, deve ser incluído em seu aplicativo Go. Esse arquivo pode ser localizado [aqui](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go). Esse arquivo deve ser incluído no pacote `plugins`.

São necessárias duas importações adicionais:
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

Na função `main`, inclua as instruções a seguir:
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

Executar o projeto e ir para a rota `/metrics` mostra as métricas que são fornecidas pelo Hystrix.
