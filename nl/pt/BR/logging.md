---

copyright:
  years: 2018
lastupdated: "2018-10-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Criação de log no Go
{: #logging_golang}

As mensagens de log são sequências com informações contextuais nelas sobre o estado e a atividade do microsserviço no momento em que a entrada de log é feita. Os logs são necessários para diagnosticar como e por que os serviços falham e desempenham uma função de suporte para [métricas](appmetrics.html) no monitoramento do funcionamento do aplicativo.

Dada a natureza temporária de processos em ambientes de nuvem, os logs devem ser coletados e enviados em outro lugar, geralmente para um local centralizado para análise. A maneira mais consistente de efetuar login em ambientes de nuvem é enviar entradas de log para fluxos de saída e erro padrão, deixando a infraestrutura manipular o restante.

## Incluindo suporte ao Logrus no app Go
{: #add_logrus}

[Logrus](https://github.com/sirupsen/logrus) é uma estrutura de criação de log popular para Go e fornece muitos benefícios nativos, que incluem: 
 * Criação de log em `stdout` ou `stderr`
 * Várias opções de anexação
 * Layout e padrões configuráveis de mensagem de log
 * Usando Níveis de log para diferentes categorias de log

1. Para usar o Logrus, execute o comando a seguir no diretório-raiz de seu aplicativo, que instala o pacote e o inclui em seu arquivo `Gopkg.toml`.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. Para usá-lo em seu aplicativo, inclua as linhas de código a seguir:
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  ** Saída Stdout **:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

Para obter mais informações sobre a customização das mensagens de log com anexadores, níveis de log e detalhes de configuração, consulte a [Documentação do Logrus](https://godoc.org/gopkg.in/Sirupsen/logrus.v0) oficial.

## Próximas Etapas
{: #next_steps}

Saiba mais sobre como visualizar os logs em cada um dos nossos ambientes de implementação:
* [Logs do Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
* [Logs do Cloud Foundry](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#ibmcloud_app_logs)
* [Logs e monitoramento do {{site.data.keyword.openwhisk}}](https://console.bluemix.net/docs/openwhisk/openwhisk_logs.html#openwhisk_logs)

Saiba mais sobre como usar um agregador de log:
* [Análise do log do {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov)
* [Pilha ELK do {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html)