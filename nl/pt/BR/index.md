---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-18"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutorial de Introdução
{: #getting-started}

O tutorial a seguir o guia pelas etapas para criar e implementar um app Go usando as ferramentas fornecidas pelo {{site.data.keyword.cloud_notm}}. É possível usar o [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) na linha de comandos ou o [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) baseado na web, conforme mostrado nas etapas do tutorial a seguir. Usando qualquer um desses métodos, é possível gerar um aplicativo Go pronto para produção em poucos minutos.

## Etapa 1. Criando um app Go no painel
{: #create-go-app}

1. Na página [Kits do iniciador](https://cloud.ibm.com/developer/appservice/starter-kits) no Serviço de aplicativo, selecione um Kit do iniciador que seja gravado em `Go`. Também é possível criar um aplicativo iniciador em branco clicando em **Criar app** e selecionando `Go` como a linguagem.

    Deve-se estar com login efetuado em uma conta do {{site.data.keyword.cloud_notm}} para criar um app. Se você não tiver uma conta, será possível [registrar-se para uma conta grátis](https://cloud.ibm.com/registration).
    {: tip}

3. Clique em **Criar app**.
4. **Nomeie** seu app ou use o nome do app genérico que é fornecido.
5. Insira um **nome do host exclusivo**. O nome do host é usado para acessar seu aplicativo, por exemplo: `server.mybluemix.net`.
6. Clique em **Criar**. Após a criação do app, é possível implementar usando uma cadeia de ferramentas ou continuar construindo e implementando seu projeto usando a [linha de comandos](/docs/cli/index.html#ibmcloud-cli).

## Etapa 2. Implementando com o painel
{: #deploy-go}

1. Na página Detalhes do app, clique em **Implementar na nuvem**.
2. Configure o método de implementação de acordo com as instruções para o método escolhido:
  * **Implemente no [Kubernetes](/docs/apps/deploying/containers.html#containers)**. Essa opção cria um cluster de hosts, chamados de nós do trabalhador, para implementar e gerenciar contêineres de aplicativo altamente disponíveis. É possível criar um cluster ou implementar em um cluster existente.
  * **Implemente no Cloud Foundry**. Essa opção implementa o seu app nativo de nuvem sem você precisar gerenciar a infraestrutura subjacente. Se a sua conta tiver acesso ao {{site.data.keyword.cfee_full_notm}}, será possível selecionar um tipo de implementador do **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** ou do **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)**, que é possível usar para criar e gerenciar ambientes isolados para hospedar aplicativos do Cloud Foundry exclusivamente para a sua empresa.
  * **Implemente em um [Servidor virtual](/docs/apps/vsi-deploy.html#vsi-deploy)**. Essa opção provisiona uma instância de servidor virtual, carrega uma imagem que inclui o seu app, cria uma cadeia de ferramentas do DevOps e inicia o primeiro ciclo de implementação para você.

3. Selecione suas opções e, em seguida, clique em **Criar**. Uma cadeia de ferramentas é criada para você e implementa seu app automaticamente.

## Etapa 3. Incluindo um recurso (opcional)
{: #add-resource-go}

É possível incluir serviços rapidamente como segurança ou armazenamento em seu app Go de dentro do painel.

1. Retorne ao app Go no [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard).
2. Clique em **Incluir recurso**, selecione a categoria do serviço que você deseja incluir, clique em **Avançar** e, em seguida, escolha seu serviço. O {{site.data.keyword.cloud_notm}} App Service cria o serviço para você com base no plano selecionado. Se você criou anteriormente o serviço que planeja usar, escolha a categoria **Existente**.
3. Depois que o serviço for criado, clique em **Fazer download do código** para gerar novamente o projeto com o SDK que se conecta ao seu serviço.

## Etapa 4. Testando e acessando seu app Go localmente
{: #run_local-go}

É possível usar o [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) para trabalhar com seu app Go localmente usando os comandos `ibmcloud dev`.

1. Use o comando `ibmcloud dev build` para construir seu aplicativo.
2. Use o comando `ibmcloud dev run` para executar o aplicativo localmente. Seu aplicativo é executado em contêineres do Docker no sistema local. É possível testar seu aplicativo em um navegador, acessando `http://localhost:8080`.
3. Um terminal de verificação de funcionamento está disponível em `http://localhost:8080/health`.
4. É possível acessar as métricas em `http://localhost:3000/metrics`.

Para saber mais sobre o {{site.data.keyword.dev_cli_long}}, consulte a [documentação da CLI](/docs/cli/index.html#ibmcloud-cli) completa.

## Explorando métodos de implementação alternativos
{: #deploy_cloud-go notoc}

Aprenda a implementar seu aplicativo na guia Implementações [aqui](/docs/go/deploying_apps.html). 

Para expandir seu aplicativo Go, continue verificando os tópicos no guia de programação do Go.
