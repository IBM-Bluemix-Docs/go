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

# Tutoriel de mise en route
{: #getting-started}

Ce tutoriel vous guide à travers les étapes de création et de déploiement d'une application Go à l'aide des outils {{site.data.keyword.cloud_notm}} fournis. Vous pouvez utiliser les outils [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) sur la ligne de commande le service Web [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) comme illustré dans la procédure suivante du tutoriel. En utilisant l'une ou l'autre de ces méthodes, vous pouvez générer une application Go prête à la production en quelques minutes seulement.

## Etape 1. Création d'une application Go dans le tableau de bord
{: #create-go-app}

1. Sur la page [Kits de démarrage](https://cloud.ibm.com/developer/appservice/starter-kits) du service d'application, sélectionnez un kit de démarrage écrit dans `Go`. Vous pouvez également créer une application de démarrage vide en cliquant sur **Créer une application** et en sélectionnant `Go` comme langage.

    Vous devez être connecté à un compte {{site.data.keyword.cloud_notm}} pour créer une application. Si vous ne disposez pas d'un compte, vous pouvez [vous enregistrer pour obtenir un compte gratuit](https://cloud.ibm.com/registration).
    {: tip}

3. Cliquez sur **Créer une application**.
4. **Nommez** votre application ou utilisez le nom d'application générique fourni.
5. Entrez un **nom d'hôte unique**. Le nom d'hôte est utilisé pour accéder à votre application, par exemple : `server.mybluemix.net`.
6. Cliquez sur **Créer**. Une fois votre application créée, vous pouvez déployer à l'aide d'une chaîne d'outils ou vous pouvez continuer de générer et de déployer votre projet à partir de la [ligne de commande](/docs/cli/index.html#ibmcloud-cli).

## Etape 2. Déploiement avec le tableau de bord
{: #deploy-go}

1. Dans la page Détails de l'application, cliquez sur **Déployer dans le cloud**.
2. Configurez votre méthode de déploiement en fonction des instructions s'appliquant à la méthode choisie.
  * **Déployer dans [Kubernetes](/docs/apps/deploying/containers.html#containers)**. Cette option crée un cluster d'hôtes, appelé noeuds worker, afin de déployer et de gérer des conteneurs d'application à haute disponibilité. Vous pouvez créer un cluster ou déployer sur un cluster existant.
  * **Déployer dans Cloud Foundry**. Cette option déploie votre application cloud native sans qu'il soit nécessaire de gérer l'infrastructure sous-jacente. Si votre compte a accès à {{site.data.keyword.cfee_full_notm}}, vous pouvez sélectionner un déployeur de type **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** ou **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)**, que vous pouvez utiliser pour créer et gérer des environnements isolés pour l'hébergement de vos applications Cloud Foundry exclusivement pour votre entreprise.
  * **Déployer sur un [serveur virtuel](/docs/apps/vsi-deploy.html#vsi-deploy)**. Cette option met à disposition une instance de serveur virtuel, charge une image qui inclut votre application, crée une chaîne d'outils DevOps et initie pour vous le premier cycle de déploiement.

3. Sélectionnez vos options, puis cliquez sur **Créer**. Une chaîne d'outils est créée qui déploie votre application automatiquement.

## Etape 3. Ajout d'une ressource (facultatif)
{: #add-resource-go}

Vous pouvez ajouter rapidement des services tels que la sécurité ou le stockage à votre application Go depuis le tableau de bord.

1. Retournez à votre application Go dans [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard).
2. Cliquez sur **Ajouter une ressource**, sélectionnez la catégorie du service que vous souhaitez ajouter, cliquez sur **Suivant**, puis choisissez votre service. {{site.data.keyword.cloud_notm}} App Service crée le service pour vous en fonction du plan sélectionné. Si vous avez créé au préalable le service que vous prévoyez d'utiliser, choisissez la catégorie **Existant**.
3. Une fois le service créé, cliquez sur **Télécharger le code** pour régénérer le projet avec le logiciel SDK connecté à votre service.

## Etape 4. Test et accès à votre application Go localement
{: #run_local-go}

Vous pouvez utiliser [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) pour travailler avec votre application Go localement à l'aide des commandes `ibmcloud dev`.

1. Utilisez la commande `ibmcloud dev build` pour générer votre application.
2. Utilisez la commande `ibmcloud dev run` pour exécuter l'application en local. Votre application est exécutée dans les conteneurs Docker sur votre système local. Vous pouvez tester votre application dans un navigateur en accédant à `http://localhost:8080`.
3. Un noeud final du diagnostic d'intégrité est disponible à l'adresse `http://localhost:8080/health`.
4. Vous pouvez accéder aux métriques sous `http://localhost:3000/metrics`.

Pour en savoir plus sur {{site.data.keyword.dev_cli_long}}, consultez la [documentation complète de l'interface de ligne de commande](/docs/cli/index.html#ibmcloud-cli).

## Exploration de méthodes de déploiement alternatives
{: #deploy_cloud-go notoc}

Découvrez comment déployer votre application sous l'onglet Déploiement [ici](/docs/go/deploying_apps.html). 

Pour développer votre application Go, consultez les rubriques du guide de programmation Go.
