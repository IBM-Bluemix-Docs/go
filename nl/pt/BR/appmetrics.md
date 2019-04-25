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

# Usando as métricas do aplicativo com apps Go
{: #go-appmetrics}

Métricas de aplicativo são importantes para monitorar o desempenho de seu aplicativo. Ter uma visualização em tempo real de métricas como métricas de CPU, Memória, Latência e HTTP é essencial para assegurar que seu aplicativo esteja em execução efetivamente ao longo do tempo. É possível usar um serviço de nuvem como o [autoscaling](/docs/services/Auto-Scaling?topic=services/Auto-Scaling-get-started#get-started) do Cloud Foundry que depende de métricas para escalar dinamicamente instâncias para corresponder à carga de trabalho atual. Ao usar métricas do aplicativo, você é informado com precisão quando aumentar a escala, reduzir a escala ou limpar instâncias que não são mais necessárias para manter os custos baixos.

As métricas do aplicativo são capturadas como dados de série temporal. A agregação e a visualização de métricas capturadas podem ajudar a identificar problemas comuns de desempenho, como:

* Tempos de resposta lentos de HTTP em algumas ou em todas as rotas
* Rendimento de rendimento no aplicativo
* Espiões na demanda que causam desaceleração
* Uso de CPU maior que o esperado para o nível de rendimento/carregamento
* Uso de memória alto ou crescente (potencial fuga de memória)

## Incluindo as métricas do aplicativo em seu aplicativo Go existente
{: #go-add-appmetrics-existing}

Para incluir monitoramento de desempenho em seu aplicativo Go, é possível usar a agregação abrangente de métricas que são fornecidas pelo pacote `promhttp`.

O pacote `promhttp` tem muitos pontos de extensão, incluindo a [Configuração do
Prometheus](https://github.com/prometheus/client_golang){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

1. Por exemplo, use o aplicativo simples “Hello World” Go + Gin a seguir:
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

2. Obtenha o pacote com o comando a seguir:
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. Inclua `promhttp` nas importações:
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  Uma rota de métricas extra também deve ser incluída no roteador.

  O código revisado agora é semelhante ao seguinte exemplo:
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

## Usando as métricas do aplicativo em kits do iniciador
{: #go-starterkits-appmetrics}

Os aplicativos Go do lado do servidor que são criados usando os Kits do iniciador vêm automaticamente com o [Terminal do Prometheus](https://prometheus.io/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") em `http://<hostname>:<port>/metrics`. O código para esse terminal está em `server.go`.

Para obter mais informações, consulte o [Repositório GitHub para o Prometheus](https://github.com/prometheus/client_golang/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").
