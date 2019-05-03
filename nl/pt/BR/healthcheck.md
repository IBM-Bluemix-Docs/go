---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Usando uma verificação de funcionamento em seu app Go
{: #healthcheck}

As verificações de funcionamento fornecem um mecanismo simples para determinar se um aplicativo do lado do servidor está se comportando adequadamente. Ambientes de nuvem como o [Kubernetes](https://www.ibm.com/cloud/container-service) e o [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) podem ser configurados para consultar terminais de funcionamento periodicamente para determinar se uma instância de seu serviço está pronta para aceitar o tráfego.

## Visão geral da verificação de
{: #overview}

As verificações de funcionamento fornecem um mecanismo simples para determinar se um aplicativo do lado do servidor está se comportando adequadamente. Elas geralmente são acessadas por meio de HTTP e usam códigos de retorno padrão para indicar o status UP ou DOWN. O valor de retorno de uma verificação de funcionamento é variável, mas uma resposta JSON mínima, como `{"status": "UP"}`, é típica.

O Kubernetes tem uma noção matizada do funcionamento do processo. Ele define duas análises:

- Uma análise de _**prontidão**_ é usada para indicar se o processo pode manipular solicitações (é roteável).

  O Kubernetes não roteia trabalho para um contêiner com uma análise de prontidão com falha. Uma análise de prontidão poderá falhar se um serviço não terminar de inicializar ou se estiver de outra forma ocupado, sobrecarregado ou incapaz de processar solicitações.

- Uma análise de _**atividade**_ é usada para indicar se o processo deve ser reiniciado.

  O Kubernetes para e reinicia um contêiner com uma análise de atividade com falha. Use as análises de atividade para limpar processos em um estado irrecuperável, por exemplo, se a memória estiver esgotada, ou se o contêiner não foi parado corretamente após um processo interno ter falhado.

Como uma nota para comparação, o Cloud Foundry usa um terminal de funcionamento. Se essa verificação falhar, o processo será reiniciado, mas se ele for bem-sucedido, as solicitações serão roteadas para ele. Nesse ambiente, o terminal é minimamente bem-sucedido quando o processo é em tempo real. Um atraso inicial é configurado para adiar a verificação de funcionamento até que o serviço conclua a inicialização para evitar ciclos de reinicialização.

A tabela a seguir fornece orientação sobre as respostas que os terminais de prontidão, de atividade e de funcionamento singular devem fornecer.

| Estado    | Prontidão                   | Atividade                   | Funcionamento                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Não OK não causa carregamento       | Não OK causa reinicialização      | Não OK causa reinicialização     |
| Iniciando | 503 - Indisponível           | 200 - OK                   | Usar atraso para evitar teste   |
| Ativado       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Parando | 503 - Indisponível           | 200 - OK                   | 503 - Indisponível         |
| Inativo     | 503 - Indisponível           | 503 - Indisponível          | 503 - Indisponível         |
| Com erro  | 500 - Erro do servidor          | 500 - Erro do servidor         | 500 - Erro do servidor        |

## Incluindo uma verificação de funcionamento em um app Go existente
{: #add-healthcheck-existing}

É possível incluir uma verificação mínima de funcionamento ou de atividade em um aplicativo `Gin-Gonic` existente introduzindo uma nova rota, conforme mostrado no exemplo a seguir:
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

Verifique o status do app com um navegador, acessando o terminal `/health`.

Há bibliotecas mais extensivas disponíveis, como [`http-healthcheck`](https://github.com/robzienert/http-healthcheck), que permitem a definição de verificações de funcionamento extensíveis com suporte para verificações de armazenamento em cache que são executadas nos serviços auxiliares. Nesse caso, você desejaria separar o teste de atividade simples no exemplo da verificação de prontidão detalhada mais robusta que é construída pelo pacote de verificação de funcionamento.

## Acessando o terminal de verificação de funcionamento em apps do Go Starter Kit
{: #healthcheck-starterkit}

Por padrão, quando você gera um app Go usando um Kit do Iniciador,
um terminal de verificação de funcionamento básico (não autorizado) está disponível em `/health` para verificar o status do app (UP/DOWN).

O código do terminal de verificação de funcionamento é fornecido pelo arquivo `/routers/health.go` a seguir:
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

## Recomendações para análises de prontidão e de atividade
{: #recommend-healthcheck}

Análises de prontidão deverão incluir a viabilidade das conexões com serviços de recebimento de dados em seu resultado quando não houver um fallback aceitável, caso o serviço de recebimento de dados esteja indisponível. Isso não significa chamar a verificação de funcionamento que é fornecida pelo serviço de recebimento de dados diretamente, pois a infraestrutura verifica isso para você. Em vez disso, considere verificar o funcionamento das referências existentes que seu aplicativo possui para os serviços de recebimento de dados: essa pode ser uma conexão JMS com o WebSphere MQ ou um consumidor ou produtor Kafka inicializado. Se você verificar a viabilidade das referências internas com relação aos serviços de recebimento de dados, armazene em cache o resultado para minimizar o impacto que a verificação de funcionamento tem em seu aplicativo.

Em contrapartida, uma análise de atividade é deliberada sobre o que é verificado, uma vez que uma falha resulta em finalização imediata do processo. Um terminal HTTP simples que sempre retorna `{"status": "UP"}` com código de status `200` é uma opção razoável.

## Configurando análises de prontidão e de atividade no Kubernetes
{: #config_probes-healthcheck}

Declare análises de atividade e prontidão juntamente com a implementação do Kubernetes. Ambas as análises usam os mesmos parâmetros de configuração:

* O kubelet aguarda por `initialDelaySeconds` antes da primeira análise.

* O kubelet analisa o serviço a cada `periodSeconds` segundos. O padrão é 1.

* A análise atinge o tempo limite após `timeoutSeconds` segundos. O valor padrão e mínimo é 1.

* A análise será bem-sucedida se ela for bem-sucedida `successThreshold` vezes após uma falha. O valor padrão e mínimo é 1. O valor deve ser 1 para análises de atividade.

* Quando um pod é iniciado e a análise falha, o Kubernetes tenta reiniciar o pod `failureThreshold` vezes e, em seguida, desiste. O valor mínimo é 1 e o valor padrão é 3.
    - Para uma análise de atividade, "desistir" significa reiniciar o pod.
    - Para uma análise de prontidão, "desistir" significa marcar o pod como `Unready`.

Para evitar ciclos de reinicialização, configure `livenessProbe.initialDelaySeconds` para ser seguramente mais longo do que demora seu serviço para inicializar. É possível, então, usar um valor mais curto para `readinessProbe.initialDelaySeconds` para rotear solicitações para o serviço assim que ele estiver pronto.

Uma configuração de exemplo pode ser semelhante a esta:
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

Para obter mais informações, consulte como [Configurar análises de atividade e de prontidão](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).
