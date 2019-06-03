---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-30"

keywords: deploy go apps, deploy go kubernetes, deploy go cluster, deploy go cli, deploy go cloud foundry, go deploy virtual

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Implementando e integrando apps Go
{: #go-deploy-apps}

É possível implementar seus apps Go com uma cadeia de ferramentas ou usando a interface da linha de comandos. Uma cadeia de ferramentas é um conjunto de integrações de ferramentas para ajudar a construir e automatizar a implementação do aplicativo. Para entender mais sobre as cadeias de ferramentas e como elas funcionam, consulte [Entrega contínua](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-getting-started). Esse tópico ajuda a localizar informações úteis sobre diferentes metodologias de implementação para aplicativos Go como CLI, Kubernetes, contêineres, VSI e nuvem privada.

## Implementando em um cluster do Kubernetes
{: #deploy_kube-go}

É possível aprender como usar o {{site.data.keyword.cloud_notm}} Kubernetes Service para implementar um app Go conteinerizado. Consulte as [etapas do tutorial](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial) para obter mais informações sobre a configuração de um cluster do Kubernetes no {{site.data.keyword.cloud_notm}}.

Blogs relacionados que usam a CLI:
* [Implementando no Kubernetes com a CLI do {{site.data.keyword.dev_cli_long}}](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").
* [Implementando no IBM Cloud Private com a CLI do {{site.data.keyword.dev_cli_short}}](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo").

## Implementando em serviços de contêiner com a CLI
{: #go-deploy-container}

Use o comando `ibmcloud dev deploy` para implementar no {{site.data.keyword.cloud_notm}}. 

Para implementar no IBM Container Services no {{site.data.keyword.cloud_notm}}, use o comando a seguir:
```
ibmcloud dev deploy -target container 
```
{: codeblock}

Para obter mais informações sobre os comandos `ibmcloud dev`, consulte [Desenvolvendo e implementando seus apps](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Implementando no Cloud Foundry
{: #go-deploy-cf}

Essa opção implementa o seu app nativo de nuvem sem você precisar gerenciar a infraestrutura subjacente.

Se você planejar implementar o seu app no [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry?topic=cloud-foundry-about#about), deverá [preparar a sua conta do {{site.data.keyword.cloud_notm}}](/docs/cloud-foundry?topic=cloud-foundry-prepare#prepare).

Se a sua conta tiver acesso ao {{site.data.keyword.cfee_full_notm}}, será possível selecionar um tipo de implementador do **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf#about-cf)** ou do **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee#cfee)**, que é possível usar para criar e gerenciar ambientes isolados para hospedar aplicativos do Cloud Foundry exclusivamente para a sua empresa.

## Implementando em um servidor virtual
{: #go-vsi-deploy}

Implemente os apps {{site.data.keyword.cloud}} App Service nas instâncias do servidor virtual para permitir que suas atividades de desenvolvedor de plataforma e infraestrutura funcionem juntas e aumentem o controle e a flexibilidade do app.

Uma instância de servidor virtual oferece melhor transparência, previsibilidade e automação para todos os tipos de carga de trabalho quando comparada com outras configurações. Combine-a com um servidor bare metal para criar combinações de carga de trabalho exclusivas. Por exemplo, é possível criar lógica de banco de dados de alto desempenho ou aprendizado de máquina com configurações bare metal e GPU que executam um sistema operacional baseado em Debian Linux.

Para obter mais informações, consulte [Implementando em um servidor virtual](/docs/apps?topic=creating-apps-vsi-deploy#vsi-deploy).

