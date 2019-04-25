---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-08"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configurando o ambiente do Go
{: #configure-go-env}

Diretrizes padronizadas estão disponíveis para seguir para o desenvolvimento de aplicativos Go que ajudam a manter a portabilidade consistente. As considerações a serem feitas incluem gerenciamento de credencial, salvamento de dados, armazenamento de dados e publicação de conteúdo na nuvem. Seguindo as práticas de Nuvem Nativa, um aplicativo Go pode mover de um ambiente para outro facilmente. Por exemplo, do teste para a produção, sem mudar o código, ou exercitando de outra forma caminhos de código não testados.

Se for necessário incluir suporte de nuvem em aplicativos Go existentes ou criar aplicativos Go com os Kits do iniciador, o objetivo será fornecer portabilidade para uso com qualquer plataforma de desenvolvimento.

## Incluindo suporte de Nuvem em aplicativos Go existentes
{: #go-add-cloud-support}

O módulo [`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") agrega variáveis de ambiente de vários provedores em nuvem, tais como o Cloud Foundry e o Kubernetes, portanto, o aplicativo é independente do ambiente.

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

Recupere os valores em seu aplicativo usando os comandos a seguir.

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

Agora seu aplicativo pode ser implementado em qualquer ambiente de tempo de execução, abstraindo as diferenças introduzidas por meio de diferentes provedores de cálculo em nuvem.

### Filtrando os valores para tags e rótulos
{: #go-filter-creds}

É possível filtrar credenciais que são geradas pelo módulo com base nas tags de serviço e nos rótulos de serviço, conforme mostrado no exemplo a seguir:
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## Usando o gerenciador de configuração Go dos apps do Kit do Iniciador
{: #go-config-manager}

Os aplicativos Go que são criados com os [Kits do iniciador](https://cloud.ibm.com/developer/appservice/starter-kits/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") vêm automaticamente com credenciais e configurações necessárias para execução em muitos destinos de implementação na nuvem, tais como o Cloud Foundry, o Kubernetes, o VSI e o Functions.

### Entendendo credenciais de serviço
{: #go-credentials-config}

Suas informações de configuração de aplicativo para serviços são armazenadas no arquivo `localdev-config.json` no diretório `/server/config`. O arquivo fica no diretório `.gitignore` para evitar que informações confidenciais sejam armazenadas no Git. As informações de conexão de qualquer serviço configurado que é executado localmente, como nome do usuário, senha e nome do host, são armazenadas nesse arquivo.

O aplicativo usa o gerenciador de configuração para ler as informações de conexão e de configuração do ambiente e desse arquivo. Ele usa um `mappings.json` customizado, que está localizado no diretório `server/config`, para comunicar onde as credenciais podem ser localizadas para cada serviço.

Se o aplicativo estiver em execução localmente, ele poderá se conectar a serviços do {{site.data.keyword.cloud_notm}} usando credenciais desvinculadas que são lidas do arquivo `mappings.json`. Mas, se for necessário criar credenciais desvinculadas, será possível fazer isso no console da web do {{site.data.keyword.cloud_notm}} (exemplo) ou usando o comando `cf create-service-key` da [CLI do CloudFoundry](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

Quando você enviar seu aplicativo por push para o {{site.data.keyword.cloud_notm}}, esses valores não serão mais usados. Em vez disso, o aplicativo se conecta automaticamente aos serviços ligados usando variáveis de ambiente. 

* **Cloud Foundry**: as credenciais de serviço são obtidas da variável de ambiente `VCAP_SERVICES`.

* **Kubernetes**: as credenciais de serviço são obtidas de variáveis de ambiente individuais por serviço.

* **{{site.data.keyword.cloud_notm}} Container Service**: credenciais de serviço são obtidas de instâncias do servidor virtual ou do {{site.data.keyword.openwhisk}} (Openwhisk).

## Próximas Etapas
{: #go-next-steps-config notoc}

O `ibm-cloud-env-golang` suporta a procura de valores usando quatro tipos de padrão de procura: `user-provided`, `cloudfoundry`, `env` e `file`. Se você quiser verificar outros padrões de procura suportados e exemplos de padrões de procura, verifique a seção [Uso](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").
