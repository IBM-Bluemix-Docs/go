---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-19"

keywords: configure go environment, go environment

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuration de l'environnement Go
{: #configure-go-env}

Des directives normalisées sont disponibles pour le développement d'applications Go qui aident à maintenir une portabilité constante. Les considérations à prendre en compte incluent la gestion des données d'identification, l'enregistrement des données, le stockage des données et la publication du contenu dans le cloud. En suivant les pratiques de Cloud Native, une application Go peut passer facilement d'un environnement à un autre. Par exemple, du test à la production, sans changer de code, ou en utilisant des chemins de code autrement non testés.

Que vous ayez besoin d'ajouter la prise en charge du cloud aux applications Go existantes ou de créer des applications Go avec des kits de démarrage, l'objectif est de fournir une portabilité pour permettre une utilisation avec n'importe quelle plateforme de développement.

## Ajout de la prise en charge du cloud aux applications Go existantes
{: #go-add-cloud-support}

Le module [`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") regroupe les variables d'environnement de différents fournisseurs de cloud, tels que Cloud Foundry et Kubernetes, de sorte que l'application est indépendante de l'environnement.

### Installation du module `ibm-cloud-env-golang`
{: #go-install-env-module}

1. Installez le module `ibm-cloud-env-golang` avec la commande suivante :
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. Initialisez le module dans votre code en référençant le fichier `mappings.json` :
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Comme Go ne prend pas en charge les paramètres par défaut, aucun fichier `mappings.json` par défaut n'est fourni. Si le chemin d'accès au fichier de mappage n'est pas spécifié dans `IBMCloudEnv.Initialize()`, une erreur est consignée. 
  {: tip}

  Exemple de fichier `mappings.json` :
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

### Extraction des données d'identification de service
{: #go-get-creds}

Récupérez les valeurs de votre application à l'aide des commandes suivantes.

1. Pour extraire la variable `service1credentials` :
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. Pour extraire la variable `service2username` :
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

A présent, votre application peut être implémentée dans un environnement d'exécution en faisant abstraction des différences introduites par des fournisseurs de traitement cloud différents.

### Filtrage des valeurs pour les balises et libellés
{: #go-filter-creds}

Vous pouvez filtrer les données d'identification générées par le module en fonction des balises et libellés de service, comme illustré dans l'exemple suivant :
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## Utilisation du gestionnaire de configuration Go depuis les applications du kit de démarrage
{: #go-config-manager}

Les applications Go créées avec les [kits de démarrage](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") incluent automatiquement des données d'identification et des configurations qui sont requises pour une exécution dans de nombreuses cibles de déploiement de cloud, telles Cloud Foundry, Kubernetes, VSI et Functions.

### Présentation des données d'identification de service
{: #go-credentials-config}

Vos informations de configuration d'application pour les services sont stockées dans le fichier `localdev-config.json` du répertoire `/server/config`. Le fichier se trouve dans le répertoire `.gitignore` afin d'empêcher que les informations sensibles ne soient stockées dans Git. Les informations de connexion de tout service configuré qui s'exécute en local, comme le nom d'utilisateur, le mot de passe et le nom d'hôte, sont stockées dans ce fichier.

L'application utilise le gestionnaire de configuration pour lire les informations de connexion et de configuration depuis l'environnement et ce fichier. Elle utilise un fichier `mappings.json` fait sur mesure, situé dans le répertoire `server/config`, pour communiquer l'emplacement des données d'identification pour chaque service.

Si l'application est exécutée localement, elle peut se connecter aux services {{site.data.keyword.cloud_notm}} à l'aide des données d'identification non liées qui sont lues dans le fichier `mappings.json`. Cependant, si vous avez besoin de créer des données d'identification non liées, vous pouvez le faire à partir de la console Web {{site.data.keyword.cloud_notm}} (exemple) ou en utilisant la commande `cf create-service-key` de l'[interface de ligne de commande CloudFoundry](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").

Lorsque vous envoyez par commande push votre application à {{site.data.keyword.cloud_notm}}, ces valeurs ne sont plus utilisées. A la place, l'application se connecte automatiquement aux services liés à l'aide de variables d'environnement. 

* **Cloud Foundry** : Les données d'identification du service sont récupérées à partir de la variable d'environnement `VCAP_SERVICES`.

* **Kubernetes** : Les données d'identification du service sont récupérées par service, à partir de variables d'environnement individuelles.

* **{{site.data.keyword.cloud_notm}} Container Service** : Les données d'identification du service sont récupérées des instances de serveurs virtuels ou d'{{site.data.keyword.openwhisk}} (Openwhisk).

## Etapes suivantes
{: #go-next-steps-config notoc}

Le module `ibm-cloud-env-golang` permet de rechercher des valeurs à l'aide de quatre types de modèles de recherche : `user-provided`, `cloudfoundry`, `env` et `file`. Si vous souhaitez consulter d'autres modèles de recherche/exemples de modèles de recherche pris en charge, reportez-vous à la section [Usage](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
