---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-13"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Configurando o ambiente do Go
{: #configure-go-env}

Diretrizes padronizadas estão disponíveis para seguir para o desenvolvimento de aplicativos Go que ajudam a manter a portabilidade consistente. As considerações a serem feitas incluem gerenciamento de credencial, salvamento de dados, armazenamento de dados e publicação de conteúdo na nuvem. Seguindo as práticas nativas de nuvem, um app Go pode se mover de um ambiente para outro facilmente. Por exemplo, do teste para a produção, sem mudar o código, ou exercitando de outra forma caminhos de código não testados.

Caso você precise incluir o suporte de nuvem em apps Go existentes ou criar apps Go com Kits do iniciador, o objetivo será fornecer portabilidade para uso com qualquer plataforma de desenvolvimento.

## Incluindo suporte de Nuvem em apps Go existentes
{: #go-add-cloud-support}

O módulo [`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") agrega variáveis de ambiente de vários provedores em Nuvem, como o CloudFoundry e o Kubernetes, de modo que o app é independente do ambiente.

### Instalando o módulo `ibm-cloud-env-golang`
{: #go-install-env-module}

1. Instale o módulo `ibm-cloud-env-golang` com o comando a seguir:
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. Inicialize o módulo em seu código, referenciando o arquivo `mappings.json`:
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Como o Go não suporta parâmetros padrão, um arquivo padrão `mappings.json` não é fornecido. Se o caminho de arquivo de mapeamentos não estiver especificado em `IBMCloudEnv.Initialize()`, um erro será registrado. 
  {: tip}

  Arquivo `mappings.json` de exemplo:
  ```javascript
  {
      "service1-credentials": {
          "searchPatterns": [
              "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name",
              "env:my-service1-credentials",
              "file:/localdev/my-service1-credentials.json"
          ]
      },
      "service2-username": {
         "searchPatterns":[
              "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### Recuperando credenciais de serviço
{: #go-get-creds}

Recupere os valores em seu app usando os comandos a seguir.

1. Recuperar a variável `service1credentials`:
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. Recuperar a variável `service2username`:
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

Agora, o seu app pode ser implementado em qualquer ambiente de tempo de execução, abstraindo as diferenças que são introduzidas por meio de diferentes Provedores de cálculo de nuvem.

### Filtrando os valores para tags e rótulos
{: #go-filter-creds}

É possível filtrar credenciais que são geradas pelo módulo com base nas tags de serviço e nos rótulos de serviço, conforme mostrado no exemplo a seguir:
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## Usando o Go Configuration Manager por meio de apps do kit do iniciador
{: #go-config-manager}

Os apps Go que são criados com [kits do iniciador](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") são fornecidos automaticamente com credenciais e configurações necessárias para serem executados em muitos destinos de implementação de nuvem, como [Kubernetes](/docs/containers?topic=containers-getting-started), [Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf), [{{site.data.keyword.cfee_full_notm}}](/docs/cloud-foundry?topic=cloud-foundry-about), [Servidor virtual (VSI)](/docs/vsi?topic=virtual-servers-getting-started-tutorial) ou [{{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-getting_started).

  A implementação do VSI está disponível para alguns kits do iniciador. Para usar esse recurso, acesse o [painel do {{site.data.keyword.cloud_notm}}](https://{DomainName}) e clique em **Criar um app** no ladrilho **Apps**.
  {: note}

### Entendendo credenciais de serviço
{: #go-credentials-config}

As suas informações de configuração de app para serviços são armazenadas no arquivo `localdev-config.json` no diretório `/server/config`. O arquivo fica no diretório `.gitignore` para evitar que informações confidenciais sejam armazenadas no Git. As informações de conexão de qualquer serviço configurado que é executado localmente, como nome do usuário, senha e nome do host, são armazenadas nesse arquivo.

O app usa o Configuration Manager para ler as informações de conexão e de configuração do ambiente e desse arquivo. Ele usa um `mappings.json` customizado, que está localizado no diretório `server/config`, para comunicar onde as credenciais podem ser localizadas para cada serviço.

Se o app estiver sendo executado localmente, ele poderá se conectar aos serviços do {{site.data.keyword.cloud_notm}} usando as credenciais desvinculadas que são lidas do arquivo `mappings.json`. Mas, se for necessário criar credenciais desvinculadas, será possível fazer isso no console da web do {{site.data.keyword.cloud_notm}} (exemplo) ou usando o comando `cf create-service-key` da [CLI do CloudFoundry](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

Quando você enviar o seu app por push para o {{site.data.keyword.cloud_notm}}, esses valores não serão mais usados. Em vez disso, o app se conecta automaticamente aos serviços ligados usando variáveis de ambiente. 

* **Cloud Foundry**: as credenciais de serviço são obtidas da variável de ambiente `VCAP_SERVICES`.

* **Kubernetes**: as credenciais de serviço são obtidas de variáveis de ambiente individuais por serviço.

* **{{site.data.keyword.cloud_notm}} Container Service**: as credenciais de serviço são obtidas de instâncias do servidor virtual ou do {{site.data.keyword.openwhisk}}.

## Próximas etapas
{: #go-next-steps-config notoc}

O `ibm-cloud-env-golang` suporta a procura de valores usando quatro tipos de padrão de procura: `user-provided`, `cloudfoundry`, `env` e `file`. Se você quiser verificar outros padrões de procura suportados e exemplos de padrões de procura, verifique a seção [Uso](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").
