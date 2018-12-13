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

# Tutoriel de mise en route

Ce tutoriel vous guide à travers les étapes de création et de déploiement d'une application Go à l'aide des outils {{site.data.keyword.cloud_notm}} fournis. Vous pouvez utiliser [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) sur la ligne de commande ou le service [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) basé sur le Web, comme indiqué dans les étapes suivantes du tutoriel. En utilisant l'une ou l'autre de ces méthodes, vous pouvez générer une application Go prête à la production en quelques minutes seulement.

## Etape 1. Création d'une application Go dans le tableau de bord
{: #create_project}

1. Sur la page [Kits de démarrage](https://console.bluemix.net/developer/appservice/starter-kits) du service d'application, sélectionnez un kit de démarrage écrit dans `Go`. Vous pouvez également créer une application de démarrage vide en cliquant sur **Créer une application** et en sélectionnant `Go` comme langage.

    Vous devez être connecté à un compte {{site.data.keyword.cloud_notm}} pour pouvoir créer un projet. Si vous ne disposez pas d'un compte, vous pouvez [vous enregistrer pour obtenir un compte gratuit](https://console.bluemix.net/registration).
    {: tip}

3. Cliquez sur **Créer une application**.
4. **Nommez** votre application ou utilisez le nom d'application générique fourni.
5. Entrez un **nom d'hôte unique**. Le nom d'hôte est utilisé pour accéder à votre application, par exemple : `server.mybluemix.net`.
6. Cliquez sur **Créer**. Une fois votre projet créé, vous pouvez déployer à l'aide d'une chaîne d'outils ou vous pouvez continuer de générer et de déployer votre projet à partir de la [ligne de commande](/docs/cli/idt/index.html).

## Etape 2. Déploiement avec le tableau de bord
{: #deploy_dashboard}

1. Dans la page Détails de l'application, cliquez sur **Déployer dans le cloud**.
2. Ensuite, sélectionnez l'une des méthodes de déploiement suivantes :
    * **Déploiement dans Kubernetes** - Vous devez créer un ensemble de noeuds worker. Vous pouvez, par exemple, utiliser des machines virtuelles pour déployer et gérer des conteneurs d'application hautement disponibles. Vous pouvez créer un cluster ou déployer sur un cluster existant.
    * **Déploiement dans Cloud Foundry** - Vous n'avez pas besoin de gérer l'infrastructure sous-jacente.
    * **Déploiement dans un serveur virtuel (bêta)** - Un [compte Paiement à la carte](https://console.bluemix.net/dashboard/ibm-iaas-g1) est requis pour accéder aux serveurs virtuels.
3. Sélectionnez vos options, puis cliquez sur **Créer**. Une chaîne d'outils est créée qui déploie votre application automatiquement.

## Etape 3. Ajout d'un service (facultatif)
{: #add_service}

Vous pouvez ajouter rapidement des services tels que la sécurité ou le stockage à votre application Go depuis le tableau de bord.

1. Retournez à votre application Go dans [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard).
2. Cliquez sur **Ajouter une ressource**, sélectionnez la catégorie du service que vous souhaitez ajouter, cliquez sur **Suivant**, puis choisissez votre service. {{site.data.keyword.cloud_notm}} App Service crée le service pour vous en fonction du plan sélectionné. Si vous avez créé au préalable le service que vous prévoyez d'utiliser, choisissez la catégorie **Existant**.
3. Une fois le service créé, cliquez sur **Télécharger le code** pour régénérer le projet avec le logiciel SDK connecté à votre service.

## Etape 4. Test et accès à votre application Go localement
{: #run_local}

Vous pouvez utiliser [{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) pour travailler avec votre application Go localement à l'aide des commandes `ibmcloud dev`.

1. Utilisez la commande `ibmcloud dev build` pour générer votre application.
2. Utilisez la commande `ibmcloud dev run` pour exécuter l'application en local. Votre application est exécutée dans les conteneurs Docker sur votre système local. Vous pouvez tester votre application dans un navigateur en accédant à `http://localhost:8080`.
3. Un noeud final du diagnostic d'intégrité est disponible à l'adresse `http://localhost:8080/health`.
4. Vous pouvez accéder aux métriques sous `http://localhost:3000/metrics`.

Pour en savoir plus sur {{site.data.keyword.dev_cli_long}}, consultez la [documentation complète de l'interface de ligne de commande](/docs/cli/idt/index.html).

## Exploration de méthodes de déploiement alternatives
{: #deploy_cloud notoc}

Découvrez comment déployer votre application dans Cloud Foundry ou Kubernetes sous l'onglet Déploiement [ici](/docs/go/deploying_apps.html). 

Pour développer votre application Go, consultez les rubriques du guide de programmation Go.
