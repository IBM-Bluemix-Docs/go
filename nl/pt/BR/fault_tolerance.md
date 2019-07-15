---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: fault tolerance go, hystrix go, add fault tolerance, prometheus go, debug go apps

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurando a tolerância a falhas em apps Go
{: #fault-tolerance}

A tolerância a falhas permite que um aplicativo continue em execução se um componente falhar ou se tornar não responsivo. É possível incluir tolerância a falhas em um app Go existente ou ativar esses recursos por meio de um app Go gerado. Este tutorial se concentra em usar o [Pacote do Hystrix](https://godoc.org/github.com/afex/hystrix-go/hystrix){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") para incluir suporte de tolerância a falhas em um app Go.

## Incluindo tolerância a falhas em um app Go existente
{: #add-fault-tolerance}

No mesmo local que o seu arquivo `Gopkg.toml` de apps Go, insira os comandos a seguir para incluir os pacotes necessários em sua lista de dependências:
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

As falhas de manipulação poderão mudar dependendo de qual tipo de app Go estiver sendo usado. Para aplicativos da web, uma falha redireciona para uma página 500, enquanto os microsserviços retornam um objeto JSON que especifica o erro.

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
{: #hystrix-optional}

Antes de incluir Hystrix em Prometheus, o seu app deve ser configurado com as métricas do app. É possível seguir as etapas no tópico [Usando métricas do aplicativo com apps Go](/docs/go?topic=go-go-appmetrics) para incluir o suporte de métricas do app.

O Hystrix fornece aos usuários a capacidade de obter dados de métricas e expô-los em um coletor de métricas. Para expor o Hystrix ao Prometheus, o pacote metric_collector também deve ser incluído:
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

Além do `metric_collector`, o arquivo `prometheus_collector.go` deve ser incluído em seu app Go. Esse arquivo pode ser localizado [aqui](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go). Inclua esse arquivo no pacote `plugins`.

São necessárias duas importações:
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
