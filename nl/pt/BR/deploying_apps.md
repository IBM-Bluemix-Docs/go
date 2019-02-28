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

# Implementando e integrando apps Go
{: #deploy_apps}

É possível implementar seus apps Go com uma cadeia de ferramentas ou usando a interface da linha de comandos. Uma cadeia de ferramentas é um conjunto de integrações de ferramentas para ajudar a construir e automatizar a implementação do aplicativo. Para entender mais sobre as cadeias de ferramentas e como elas funcionam, consulte [Entrega contínua](/docs/services/ContinuousDelivery/index.html). Esse tópico ajuda a localizar informações úteis sobre diferentes metodologias de implementação para aplicativos Go como CLI, Kubernetes, contêineres, VSI e nuvem privada.

## Implementando em um cluster do Kubernetes
{: #deploy_kube}

É possível aprender como usar o {{site.data.keyword.cloud_notm}} Kubernetes Service para implementar um app Go conteinerizado. Consulte as [etapas do tutorial](https://console.bluemix.net/docs/containers/cs_cluster.html#cs_cluster) para obter mais informações sobre a configuração de um cluster do Kubernetes no {{site.data.keyword.cloud_notm}}.

Blogs relacionados que usam a CLI:
* [Implementando no Kubernetes com a CLI do IBM Cloud Developer Tools](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Implementando no IBM Cloud Private com a CLI do IBM Cloud Developer Tools](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## Implementando em serviços de contêiner com a CLI
{: #cf-deploy}

Use o comando `ibmcloud dev deploy` para implementar no {{site.data.keyword.cloud_notm}}. 

Para implementar no IBM Container Services no {{site.data.keyword.cloud_notm}}, use o comando a seguir:
```
ibmcloud dev deploy -target container 
```
{: codeblock}

Para obter mais informações sobre os comandos `ibmcloud dev`, consulte [Desenvolvendo e implementando seus apps](/docs/cli/idt/index.html)

## Implementando em um servidor virtual
{: #vsi-deploy}

Implemente os apps {{site.data.keyword.cloud}} App Service nas instâncias do servidor virtual para permitir que suas atividades de desenvolvedor de plataforma e infraestrutura funcionem juntas e aumentem o controle e a flexibilidade do app.

Uma instância de servidor virtual oferece melhor transparência, previsibilidade e automação para todos os tipos de carga de trabalho quando comparada com outras configurações. Combine-a com um servidor bare metal para criar combinações de carga de trabalho exclusivas. Por exemplo, é possível criar lógica de banco de dados de alto desempenho ou aprendizado de máquina com configurações bare metal e GPU que executam um sistema operacional baseado em Debian Linux.

Para obter mais informações, consulte [Implementando em um servidor virtual](/docs/apps/vsi-deploy.html).


