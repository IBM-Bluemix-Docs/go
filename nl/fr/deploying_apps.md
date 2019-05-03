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

# Déploiement et intégration des applications Go
{: #deploy_apps}

Vous pouvez déployer vos applications Go avec une chaîne d'outils ou en utilisant l'interface de ligne de commande. Une chaîne d'outils est un ensemble d'intégrations d'outils qui permettent de créer et d'automatiser un déploiement d'application. Pour mieux connaître les chaînes d'outils et comprendre comment elles fonctionnent, consultez la rubrique [Distribution continue](/docs/services/ContinuousDelivery/index.html#cd_getting_started). Cette rubrique vous donnera des renseignements utiles sur les différentes méthodologies de déploiement pour les applications Go, telles que l'interface de ligne de commandes, Kubernetes, les conteneurs, VSI et le cloud privé.

## Déploiement dans un cluster Kubernetes
{: #deploy_kube-go}

Apprenez à utiliser le service Kubernetes d'{{site.data.keyword.cloud_notm}} pour déployer une application Go conteneurisée. Consultez les [étapes du tutoriel](/docs/containers/cs_cluster.html#cs_cluster) pour en savoir plus sur la configuration d'un cluster Kubernetes dans {{site.data.keyword.cloud_notm}}.

Blogues associés utilisant l'interface de ligne de commande :
* [Déploiement dans Kubernetes avec l'interface de ligne de commande IBM Cloud Developer Tools](https://www.ibm.com/blogs/bluemix/2017/09/deploying-kubernetes-ibm-cloud-ibm-cloud-developer-tools-cli/).
* [Déploiement dans IBM Cloud Private avec l'interface de ligne de commande IBM Cloud Developer Tools](https://www.ibm.com/blogs/bluemix/2017/09/deploying-ibm-cloud-private-ibm-cloud-developer-tools-cli/).

## Déploiement dans des services de conteneur avec l'interface de ligne de commande
{: #go-deploy-container}

Utilisez la commande `ibmcloud dev deploy` pour le déploiement dans {{site.data.keyword.cloud_notm}}. 

Pour un déploiement sur IBM Container Services dans {{site.data.keyword.cloud_notm}}, utilisez la commande suivante :
```
ibmcloud dev deploy –target container 
```
{: codeblock}

Pour plus d'informations sur les commandes `ibmcloud dev`, voir [Développement et déploiement de vos applications](/docs/cli/index.html).

## Déploiement dans Cloud Foundry
{: #go-deploy-cf}

Cette option déploie votre application cloud native sans qu'il soit nécessaire de gérer l'infrastructure sous-jacente.

Si vous envisagez de déployer votre application dans [{{site.data.keyword.cfee_full}}](/docs/cloud-foundry/index.html), vous devez [préparer votre compte {{site.data.keyword.cloud_notm}}](/docs/cloud-foundry/prepare-account.html).

Si votre compte a accès à {{site.data.keyword.cfee_full_notm}}, vous pouvez sélectionner un déployeur de type **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** ou **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)**, que vous pouvez utiliser pour créer et gérer des environnements isolés pour l'hébergement de vos applications Cloud Foundry exclusivement pour votre entreprise.

## Déploiement sur un serveur virtuel
{: #go-vsi-deploy}

Déployez les applications d'{{site.data.keyword.cloud}} App Service dans des instances de serveur virtuel pour permettre à votre plateforme et à vos activités de développement d'infrastructure de travailler ensemble et d'augmenter le contrôle et la flexibilité des applications.

Par rapport aux autres configurations, une instance de serveur virtuel offre une meilleure transparence, un meilleur caractère prévisionnel et une meilleure automatisation pour tous les types de charge de travail. Associez-la à un serveur Bare metal pour créer des combinaisons de charge de travail uniques. Par exemple, vous pouvez créer une logique de base de données à hautes performances ou un système d'apprentissage automatique avec des configurations Bare metal ou GPU exécutant un système d'exploitation de type Debian Linux.

Pour en savoir plus, voir [Déploiement d'un serveur virtuel](/docs/apps/vsi-deploy.html#vsi-deploy).

