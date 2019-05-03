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

# Tutoriel de mise en route
{: #getting-started}

Ce tutoriel vous guide à travers les étapes de création et de déploiement d'une application Go à l'aide des outils {{site.data.keyword.cloud_notm}} fournis. Vous pouvez utiliser les outils [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli) sur la ligne de commande ou le service Web [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe"), comme illustré dans la procédure suivante du tutoriel. En utilisant l'une ou l'autre de ces méthodes, vous pouvez générer une application Go prête à la production en quelques minutes seulement.

## Etape 1. Création d'une application Go personnalisée dans le tableau de bord
{: #create-go-app}

1. Connectez-vous à un compte {{site.data.keyword.cloud_notm}} pour créer une application. Si vous ne disposez pas de compte, vous pouvez [vous enregistrer pour obtenir un compte gratuit](https://{DomainName}/registration){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
2. Sur la page des [kits de démarrage](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") de la console {{site.data.keyword.dev_console}}, effectuez une des actions suivantes :
 * Sélectionnez un kit de démarrage créé dans `Go` puis cliquez sur **Créer une application** sur la page **Détails du kit de démarrage**.
 * Sélectionnez l'application de démarrage vide puis cliquez sur **Créer une application**.
3. Indiquez un nom pour votre application ou utilisez le nom d'application générique fourni.
4. Vérifiez que **Go** est sélectionné en tant que plateforme puis cliquez sur **Créer**. Une fois que votre application est créée, vous pouvez ajouter des services puis la déployer à l'aide d'une chaîne d'outils ou vous pouvez continuer de générer et de déployer votre projet à partir de la [ligne de commande](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

## Etape 2. Ajout de {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}}
{: #add-resource-go}

Vous pouvez désormais ajouter des services {{site.data.keyword.watson}} à votre application Go. Pour ce tutoriel, ajoutez le service {{site.data.keyword.texttospeechshort}} à votre application Go, qui convertit des données vocales en texte en utilisant une API cloud.

1. Depuis la page **Détails de l'application**, cliquez sur **Ajout d'un service**.
2. Sélectionnez **IA** puis cliquez sur **Suivant**.
3. Sélectionnez **{{site.data.keyword.texttospeechshort}}** puis cliquez sur **Suivant**.
4. Cliquez sur **Créer**.

Une fois que ce service est ajouté à votre application, vous verrez que la dépendance de [logiciel SDK Watson Developer Cloud Go](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") est ajoutée au fichier `Gopkg.toml` et que le code d'instrumentation simple est disponible pour le service dans le répertoire `services/`. De plus, les informations de configuration pour l'accès aux données d'identification de service dans les environnements respectifs sont incluses.

Pour télécharger le code, cliquez sur **Télécharger le code** sur la page **Détails de l'application**. Le code téléchargé est fourni avec le [logiciel SDK Watson Developer Cloud Go](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") qui inclut également du code d'initialisation de base.

## Etape 3. Déploiement de votre application à partir de la console
{: #deploy-go}

1. Sur la page **Détails de l'application**, cliquez sur **Configurer la distribution continue**.
2. Configurez votre cible de déploiement en fonction des instructions s'appliquant à la méthode choisie :
  * **Déployer dans [IBM Kubernetes Service](/docs/apps/deploying?topic=creating-apps-containers-kube)**. Cette option crée un cluster d'hôtes, appelé noeuds worker, afin de déployer et de gérer des conteneurs d'application à haute disponibilité. Vous pouvez créer un cluster ou déployer sur un cluster existant.
  * **Déployer dans Cloud Foundry**. Cette option déploie votre application cloud native sans qu'il soit nécessaire de gérer l'infrastructure sous-jacente. Si votre compte a accès à {{site.data.keyword.cfee_full_notm}}, vous pouvez sélectionner un déployeur de type **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** ou **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)**, que vous pouvez utiliser pour créer et gérer des environnements isolés pour l'hébergement de vos applications Cloud Foundry exclusivement pour votre entreprise.
  * **Déployer sur un [serveur virtuel](/docs/apps?topic=creating-apps-vsi-deploy)**. Cette option met à disposition une instance de serveur virtuel, charge une image qui inclut votre application, crée une chaîne d'outils DevOps et initie pour vous le premier cycle de déploiement.

3. Une fois que vous avez configuré votre cible de déploiement, cliquez sur **Suivant**.
4. Sélectionnez vos options de configuration puis cliquez sur **Créer**. Une chaîne d'outils est créée et votre application est automatiquement déployée.

## Etape 4. Test et accès à votre application Go localement
{: #run_local-go}

Sur la page **Détails de l'application**, vous pouvez :
* afficher votre référentiel en cliquant sur **Afficher le référentiel**.
* afficher et modifier votre chaîne d'outils en cliquant sur **Afficher la chaîne d'outils**.

Vous pouvez utiliser {{site.data.keyword.dev_cli_long}} pour travailler avec votre application Go localement à l'aide des commandes `ibmcloud dev`. Ces outils vous permettent d'effectuer à nouveau les actions localement et de transmettre vos modifications au cloud.

1. Utilisez la commande `ibmcloud dev build` pour générer votre application.
2. Utilisez la commande `ibmcloud dev run` pour exécuter l'application en local. Votre application est exécutée dans les conteneurs Docker sur votre système local. Vous pouvez tester votre application dans un navigateur en accédant à `http://localhost:8080`.
3. Un noeud final du diagnostic d'intégrité est disponible à l'adresse `http://localhost:8080/health`.
4. Vous pouvez accéder aux métriques sous `http://localhost:3000/metrics`.

Pour en savoir plus sur {{site.data.keyword.dev_cli_notm}}, consultez la [documentation complète de l'interface de ligne de commande](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli).

Vous êtes désormais prêt à générer votre application avec le service {{site.data.keyword.texttospeechshort}} !

## Exploration de méthodes de déploiement alternatives
{: #alt-deploy-go notoc}

Découvrez comment déployer votre application dans l'[onglet Déploiements](/docs/go?topic=go-go-deploy-apps).

Pour développer votre application Go, consultez les rubriques du guide de programmation Go.
