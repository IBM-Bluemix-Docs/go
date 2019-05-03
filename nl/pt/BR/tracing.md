---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: how to trace go apps, tracing go, jaeger go, opentracing go, jaeger packages, debug go app, troubleshoot go, go app help

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurando o rastreio em apps Go
{: #go-e2e-tracing}

O tutorial a seguir foca nos pacotes Opentracing e Jaeger para rastrear aplicativos Go. Para obter mais informações sobre como usar o Jaeger, consulte o [Portal de documentação do Jaeger](https://www.jaegertracing.io/docs/1.11/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

Nas etapas a seguir, dois aplicativos pequenos (um front-end, um back-end) são usados para rastrear entre dois terminais usando o módulo Jaeger. É possível iniciar do zero ou aplicar os princípios que estão descritos aqui em seus aplicativos Go existentes.

## Etapa 1. Instalando e ativando os pacotes Opentracing e Jaeger
{: #install-go-opentracing}

1. No mesmo local que o arquivo `Gopkg.toml` do aplicativo Go, insira os comandos a seguir para incluir os pacotes necessários em sua lista de dependências:
  ```go
  dep ensure -add "github.com/opentracing/opentracing-go"
  dep ensure -add "github.com/uber/jaeger-client-go"
  dep ensure -add "github.com/uber/jaeger-lib/metrics/prometheus"
  ```
  {: codeblock}

2. Inclua as linhas a seguir em seu código do servidor do Go:
  ```go
  import(
  "github.com/opentracing/opentracing-go"
	"github.com/opentracing/opentracing-go/ext"
	"github.com/uber/jaeger-client-go"
	jaegerprom "github.com/uber/jaeger-lib/metrics/prometheus"
  )
  ```
  {: codeblock}

### Incluindo rastreio no aplicativo do servidor
{: #add-tracing-go}

Algumas instruções são necessárias para incluir o rastreio em seu aplicativo do servidor. Primeiro, deve-se criar um rastreador.

Para criar um rastreador, forneça o seguinte:
 * Transportador
 * Ferramenta de Relatório
 * Exportador de métricas opcional
 
Neste tutorial, o Jaeger exporta métricas de estilo Prometheus. Um objeto de métricas permite que o Jaeger relate as métricas ao Prometheus.

1. Para criar um objeto de métricas, inclua as instruções a seguir na função principal:
  ```go
  factory := jaegerprom.New()
  metrics := jaeger.NewMetrics(factory, map[string]string{"lib": "jaeger"})
  ```
  {: codeblock}

2. Forneça a instrução a seguir para um transportador, que informa ao Jaeger para onde enviar seus rastreios. Para o desenvolvimento local, o destino é `localhost` na porta `5775`. Embora o nome do host possa mudar, a porta na maioria dos casos é `5775`.
  ```go
  transport, err := jaeger.NewUDPTransport("<hostname>:<port>", 0)
  if err != nil {
	log.Errorln(err.Error())
  }
  ```
  {: codeblock}

3. Um relator informa ao Jaeger como ele relata seus dados de rastreio. São usados três tipos de relatores:
  * Criação de log - imprime dados de span como um log
  * Remoto - envia dados de span para um agente do Jaeger
  * Composto - combina mais de um relator

  Como os kits do iniciador do Go usam Logrus para registrar dados, relatar spans do Jaeger não é possível por padrão. No entanto, o Jaeger suporta uma interface de criação de log que pode ser usada incluindo um adaptador de criação de log com a instrução a seguir:
  ```go
  type LogrusAdapter struct{}

  func (l LogrusAdapter) Error(msg string) {
	log.Errorf(msg)
  }

  func (l LogrusAdapter) Infof(msg string, args ...interface{}) {
	log.Infof(msg, args)
  }
  ```
  {: codeblock}

4. Para criar um relator que registra e relata spans, inclua as instruções a seguir:
  ```go
  logAdapt := LogrusAdapter{}
  reporter := jaeger.NewCompositeReporter(
	jaeger.NewLoggingReporter(logAdapt),
	jaeger.NewRemoteReporter(transport,
		jaeger.ReporterOptions.Metrics(metrics),
		jaeger.ReporterOptions.Logger(logAdapt),
	),
  )
  defer reporter.Close()
  ```
  {: codeblock}

5. Um objeto de amostra determina em quais situações ou com que frequência os spans são relatados. Para propósitos de desenvolvimento, um aplicativo deve relatar todos os spans que ele recebe. No entanto, para produção, relatar todos os spans pode não ser viável. Para relatar todos os spans, é possível usar o objeto ConstSampler:
  ```go
  sampler := jaeger.NewConstSampler(true)
  ```
  {: codeblock}

6. Agora que você criou os objetos relator, sampler e metrics, será possível criar um rastreador.
  ```go
  tracer, closer := jaeger.NewTracer("<application name>",
	sampler,
	reporter,
	jaeger.TracerOptions.Metrics(metrics),
  )
  defer closer.Close()

  opentracing.SetGlobalTracer(tracer)
  ```
  {: codeblock}

7. Inclua uma função de middleware que extraia dados de span de uma solicitação do cliente ou crie um novo objeto de span raiz:
  ```go
  func OpenTracing() gin.HandlerFunc {
	return func(c *gin.Context) {
		wireCtx, _ := opentracing.GlobalTracer().Extract(
			opentracing.HTTPHeaders,
			opentracing.HTTPHeadersCarrier(c.Request.Header))

		serverSpan := opentracing.StartSpan(c.Request.URL.Path,
			ext.RPCServerOption(wireCtx))
		defer serverSpan.Finish()
		c.Request = c.Request.WithContext(opentracing.ContextWithSpan(c.Request.Context(), serverSpan))
		c.Next()
	}
  }
  ```
  {: codeblock}

8. Inclua o middleware no roteador. **DEVE-SE** incluir esta instrução antes que os terminais sejam criados:
  ```go
  router.Use(OpenTracing)
  ...
  router.GET("/health", routes.HealthGET)
  ```
  {: codeblock}

  Essa instrução configura um aplicativo que pode extrair um span e, em seguida, relatar novos dados de span. Por padrão, todos os kits do iniciador do Go implementam completamente o OpenTracing do lado do servidor.

9. Para enviar os dados de span de um serviço para outro, deve-se incluir mais algumas instruções. Consulte a solicitação de amostra a seguir:
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)
  client.Do(req)
  ```
  {: codeblock}

10. Para enviar dados de span juntamente com essa solicitação, inclua duas instruções depois que a solicitação for criada:
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)

  span := opentracing.SpanFromContext(c.Request.Context())
  opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

  client.Do(req)
  ```
  {: codeblock}

## Etapa 2. Configurando um servidor Jaeger local
{: #setup-jaeger-server}

Os servidores Jaeger são compostos por três serviços separados:
 * Agentes
 * Coletores
 * Consultas
 
Os serviços do Go conectam-se ao agente usando o objeto `UDPTransport`. Os agentes, então, relatam dados para coletores, que armazenam dados de span em um banco de dados. O serviço de consulta, então, permite que o usuário recupere spans no banco de dados. Os serviços são separados por flexibilidade porque todos os serviços devem ter seus próprios agentes aos quais se conectar, enquanto o número de serviços de coletor e de consulta pode ser escalado com base na necessidade.

No entanto, para o desenvolvimento local, o Jaeger fornece um serviço tudo em um, que vem com os serviços de agente, coletor, banco de dados e consulta empacotados juntos. Essa configuração é útil para desenvolvimento local, mas não a use na produção.

Antes de implementar seu app em qualquer nuvem, é possível testar o rastreio localmente.

Para obter mais informações sobre a implementação do Jaeger em um contêiner usando Kubernetes, consulte [estas etapas](#jaeger-kube).

Para executar o serviço tudo em um localmente, execute o comando a seguir:
```bash
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 9411:9411 \
  jaegertracing/all-in-one:latest
 ```
{: codeblock}

O agente pode ser conectado na porta `5775`, enquanto a consulta pode ser conectada à porta `16686`.

### Configurando um servidor Jaeger implementado para Kubernetes
{: #jaeger-kube}

Tal como o desenvolvimento local, o Jaeger fornece um serviço tudo em um para o desenvolvimento do Kubernetes. Use o serviço tudo em um somente para desenvolvimento, não para código de produção. Para saber mais sobre como implementar no Kubernetes para produção, consulte o [Guia de modelos do Kubernetes do Jaeger](https://github.com/jaegertracing/jaeger-kubernetes#production-setup){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

Para implementar o servidor Jaeger, conclua estas etapas:
1. Certifique-se de que o cluster esteja configurado executando `ibmcloud cs cluster-config <cluster name>` e siga as instruções.
2. Execute o comando a seguir:
  ```go
  kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
  ```
  {: codeblock}

  Esse comando implementa um agente, coletor e consulta do Jaeger no cluster do Kubernetes.
3. Antes de implementar o aplicativo Go no Kubernetes, deve-se atualizar o Transporte UDP para apontar corretamente para o agente Jaeger. O comando `kubectl` na etapa anterior cria um terminal interno denominado `"jaeger-agent:5775"`. Atualize o transporte com esse novo terminal.
  ```go
  transport, err := jaeger.NewUDPTransport("jaeger-agent:5775", 0)
  ```
  {: codeblock}

4. Depois que o aplicativo é implementado, é possível visualizar os dados de rastreio, acessando <*public cluster ip*>:<*port*>. É possível localizar o endereço IP do cluster público executando `bx cs workers <*cluster name*>`.
É possível localizar a porta executando `kubectl get service jaeger-query`.

## Etapa 3. Testando um Cenário de Exemplo
{: #test-go-tracing}

Ao seguir as etapas anteriores, é simples criar dois aplicativos Go separados que suportam rastreio. É possível incluir uma rota em um dos projetos com o código a seguir:
```go
client := http.Client{}
req, _ := http.NewRequest("GET", "http://localhost:<other port>", nil)

span := opentracing.SpanFromContext(c.Request.Context())
opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

client.Do(req)
```
{: codeblock}

Essa rota envia uma solicitação `GET` de um aplicativo para outro.

Para visualizar spans, acesse `http://localhost:16686`. É possível procurar rastreios por serviço, operação e tags e, em seguida, clicar em **Localizar rastreios**.

![IU do Jaeger](images/JaegerUI.png)

Clique em um rastreio específico para visualizar mais informações sobre ele:
![Exemplo de rastreio](images/TraceExample.png)
