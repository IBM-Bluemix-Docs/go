---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-19"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Tutorial de Introdução
{: #getting-started}

O tutorial a seguir o guia pelas etapas para criar e implementar um app Go usando as ferramentas fornecidas pelo {{site.data.keyword.cloud_notm}}. É possível usar o [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) na linha de comandos ou no [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") baseado na web, conforme mostrado nas etapas do tutorial a seguir. Usando qualquer um desses métodos, é possível gerar um aplicativo Go pronto para produção em poucos minutos.

## Etapa 1. Criando um aplicativo Go customizado no painel
{: #create-go-app}

1. Efetue login em uma conta do {{site.data.keyword.cloud_notm}} para criar um aplicativo. Se você não tiver uma conta, será possível [registrar-se para uma conta grátis](https://{DomainName}/registration){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").
2. Na página [Kits do iniciador](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") no {{site.data.keyword.dev_console}}, execute uma das opções a seguir:
 * Selecione um kit do iniciador que esteja gravado em `Go` e, em seguida, clique em **Criar aplicativo** na página **Detalhes do Kit do iniciador**.
 * Selecione o aplicativo iniciador em branco e clique em **Criar aplicativo**.
3. Forneça um nome para seu aplicativo ou use o nome genérico do aplicativo que é fornecido.
4. Assegure-se de que **Go** esteja selecionado como a plataforma e, em seguida, clique em **Criar**. Após a criação do aplicativo, é possível incluir serviços e, em seguida, implementá-los usando uma cadeia de ferramentas ou continuar construindo e implementando seu projeto usando a [linha de comandos](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Etapa 2. Incluindo o {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

Agora é possível incluir os serviços {{site.data.keyword.watson}} em seu aplicativo Go. Para este tutorial, inclua o serviço {{site.data.keyword.texttospeechshort}} em seu aplicativo Go, que receberá entradas verbais e as converterá em texto usando uma API de nuvem.

1. Na página **Detalhes do app**, clique em **Incluir serviço**.
2. Selecione **AI** e clique em **Avançar**.
3. Selecione **{{site.data.keyword.texttospeechshort}}** e clique em **Avançar**.
4. Clique em **Criar**.

Após esse serviço ser incluído no seu aplicativo, você verá que a dependência do [SDK Go do Watson Developer Cloud](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") será incluída no `Gopkg.toml` e o código de instrumentação simples estará disponível para o serviço no diretório `services/`. Além disso, as informações de configuração para acessar as credenciais de serviço em seus respectivos ambientes são incluídas.

Para fazer download do código, clique em **Fazer download do código** na página **Detalhes do aplicativo**. O código transferido por download é fornecido com o [SDK Go do Watson Developer Cloud](https://github.com/watson-developer-cloud/go-sdk){: new_window}![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") incluído, bem como algum código básico de inicialização.

## Etapa 3. Implementando seu aplicativo por meio do console
{: #deploy-go}

1. Na página **Detalhes do aplicativo**, clique em **Configurar entrega contínua**.
2. Configure o destino de implementação de acordo com as instruções para o método escolhido:
  * **Implementar no [IBM Kubernetes Service](/docs/apps/deploying?topic=creating-apps-containers-kube)**. Essa opção cria um cluster de hosts, chamados de nós do trabalhador, para implementar e gerenciar contêineres de aplicativo altamente disponíveis. É possível criar um cluster ou implementar em um cluster existente.
  * **Implemente no Cloud Foundry**. Essa opção implementa o seu app nativo de nuvem sem você precisar gerenciar a infraestrutura subjacente. Se a sua conta tiver acesso ao {{site.data.keyword.cfee_full_notm}}, será possível selecionar um tipo de implementador do **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** ou do **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**, que é possível usar para criar e gerenciar ambientes isolados para hospedar aplicativos do Cloud Foundry exclusivamente para a sua empresa.
  * **Implemente em um [Servidor virtual](/docs/apps?topic=creating-apps-vsi-deploy)**. Essa opção provisiona uma instância de servidor virtual, carrega uma imagem que inclui o seu app, cria uma cadeia de ferramentas do DevOps e inicia o primeiro ciclo de implementação para você.

3. Depois de configurar seu destino de implementação, clique em **Avançar**.
4. Selecione suas opções de configuração e, em seguida, clique em **Criar**. Uma cadeia de ferramentas é criada para você e seu aplicativo é implementado automaticamente.

## Etapa 4. Testando e acessando seu app Go localmente
{: #run_local-go}

Na página **Detalhes do aplicativo** é possível:
* Visualizar o repositório clicando em **Visualizar repositório**.
* Visualizar e modificar sua cadeia de ferramentas clicando em **Visualizar cadeia de ferramentas**.

É possível usar o {{site.data.keyword.dev_cli_long}} para trabalhar com seu app Go localmente usando os comandos `ibmcloud dev`. Essas ferramentas ajudam a iterar localmente de forma rápida e a enviar suas mudanças para a nuvem.

1. Use o comando `ibmcloud dev build` para construir seu aplicativo.
2. Use o comando `ibmcloud dev run` para executar o aplicativo localmente. Seu aplicativo é executado em contêineres do Docker no sistema local. É possível testar seu aplicativo em um navegador, acessando `http://localhost:8080`.
3. Um terminal de verificação de funcionamento está disponível em `http://localhost:8080/health`.
4. É possível acessar as métricas em `http://localhost:3000/metrics`.

Para saber mais sobre o {{site.data.keyword.dev_cli_notm}}, consulte a [documentação da CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) completa.

Agora você está pronto para construir seu aplicativo com o serviço {{site.data.keyword.texttospeechshort}}.

## Explorando métodos de implementação alternativos
{: #alt-deploy-go notoc}

Saiba como implementar seu aplicativo na [guia Implementações](/docs/go?topic=go-go-deploy-apps).

Para expandir seu aplicativo Go, continue verificando os tópicos no guia de programação do Go.
