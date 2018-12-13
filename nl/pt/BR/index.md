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

# Tutorial de Introdução

O tutorial a seguir o guia pelas etapas para criar e implementar um app Go usando as ferramentas fornecidas pelo {{site.data.keyword.cloud_notm}}. É possível usar o [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) na linha de comandos ou no [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) baseado na web, conforme mostrado nas etapas do tutorial a seguir. Usando qualquer um desses métodos, é possível gerar um aplicativo Go pronto para produção em poucos minutos.

## Etapa 1. Criando um app Go no painel
{: #create_project}

1. Na página [Kits do iniciador](https://console.bluemix.net/developer/appservice/starter-kits) no Serviço de aplicativo, selecione um Kit do iniciador que seja gravado em `Go`. Também é possível criar um aplicativo iniciador em branco clicando em **Criar app** e selecionando `Go` como a linguagem.

    Deve-se estar conectado a uma conta do {{site.data.keyword.cloud_notm}} para criar um projeto. Se você não tiver uma conta, será possível [registrar-se para uma conta grátis](https://console.bluemix.net/registration).
    {: tip}

3. Clique em **Criar app**.
4. **Nomeie** seu app ou use o nome do app genérico que é fornecido.
5. Insira um **nome do host exclusivo**. O nome do host é usado para acessar seu aplicativo, por exemplo: `server.mybluemix.net`.
6. Clique em **Criar**. Após a criação do projeto, é possível implementar usando uma cadeia de ferramentas ou é possível continuar a construir e implementar seu projeto pela [linha de comandos](/docs/cli/idt/index.html).

## Etapa 2. Implementando com o painel
{: #deploy_dashboard}

1. Na página Detalhes do app, clique em **Implementar na nuvem**.
2. Em seguida, selecione um dos métodos de implementação a seguir:
    * **Implementar no Kubernetes** - Deve-se criar um conjunto de nós do trabalhador. Por exemplo, é possível usar VMs para implementar e gerenciar contêineres de aplicativos altamente disponíveis. É possível criar um cluster ou implementar em um cluster existente.
    * **Implementar no Cloud Foundry** - Não é necessário gerenciar a infraestrutura subjacente.
    * **Implementar em um Servidor Virtual (Beta)** - Uma [conta pré-paga](https://console.bluemix.net/dashboard/ibm-iaas-g1) é necessária para acessar os servidores virtuais.
3. Selecione suas opções e, em seguida, clique em **Criar**. Uma cadeia de ferramentas é criada para você e implementa seu app automaticamente.

## Etapa 3. Incluindo um serviço (opcional)
{: #add_service}

É possível incluir serviços rapidamente como segurança ou armazenamento em seu app Go de dentro do painel.

1. Retorne ao app Go no [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard).
2. Clique em **Incluir recurso**, selecione a categoria do serviço que você deseja incluir, clique em **Avançar** e, em seguida, escolha seu serviço. O {{site.data.keyword.cloud_notm}} App Service cria o serviço para você com base no plano selecionado. Se você criou anteriormente o serviço que planeja usar, escolha a categoria **Existente**.
3. Depois que o serviço for criado, clique em **Fazer download do código** para gerar novamente o projeto com o SDK que se conecta ao seu serviço.

## Etapa 4. Testando e acessando seu app Go localmente
{: #run_local}

É possível usar o [{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) para trabalhar com seu app Go localmente usando os comandos `ibmcloud dev`.

1. Use o comando `ibmcloud dev build` para construir seu aplicativo.
2. Use o comando `ibmcloud dev run` para executar o aplicativo localmente. Seu aplicativo é executado em contêineres do Docker no sistema local. É possível testar seu aplicativo em um navegador, acessando `http://localhost:8080`.
3. Um terminal de verificação de funcionamento está disponível em `http://localhost:8080/health`.
4. É possível acessar as métricas em `http://localhost:3000/metrics`.

Para saber mais sobre o {{site.data.keyword.dev_cli_long}}, consulte a [documentação da CLI](/docs/cli/idt/index.html) completa.

## Explorando métodos de implementação alternativos
{: #deploy_cloud notoc}

Aprenda como implementar seu aplicativo no Cloud Foundry ou Kubernetes sob a guia Implementações [aqui](/docs/go/deploying_apps.html). 

Para expandir seu aplicativo Go, continue verificando os tópicos no guia de programação do Go.
