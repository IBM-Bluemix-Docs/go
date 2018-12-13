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

# Utilisation des métriques d'application avec les applications Go
{: #metrics}

Les métriques d'application sont importantes pour la surveillance des performances de votre application. Il est essentiel d'avoir une vue en temps réel des métriques telles que l'unité centrale, la mémoire, la latence et des métriques HTTP pour garantir le bon fonctionnement de votre application dans le temps. Vous pouvez utiliser un service cloud comme Cloud Foundry [Auto-Scaling](/docs/services/Auto-Scaling/index.html), qui repose sur des métriques pour adapter dynamiquement les instances aux charges de travail actuelles. En utilisant des métriques d'application, vous êtes informé précisément lorsqu'il faut augmenter ou réduire des instances, ou les nettoyer lorsqu'elles ne sont plus nécessaires, le tout pour limiter les coûts.

Les métriques d'application sont capturées sous forme de données de série temporelles. L'agrégation et la visualisation des métriques capturées peut permettre d'identifier des problèmes de performance courants :

* Faibles temps de réponse HTTP sur tout ou partie des routes
* Débit médiocre dans l'application 
* Pics de demandes entraînant des ralentissements
* Utilisation de l'unité centrale plus élevée que prévu pour le niveau de débit/charge
* Utilisation de la mémoire élevée ou en augmentation (possible fuite de mémoire)

## Ajout de métriques d'application à votre application Go existante
{: #add-appmetrics-existing}

Pour ajouter une surveillance des performances à votre application Go, vous pouvez utiliser l'agrégation complète des métriques fournies par le package `promhttp`.

Le package `promhttp` possède de nombreux points d'extension, notamment la [configuration de Prometheus](https://github.com/prometheus/client_golang).

1. Par exemple, utilisez l'application simple Go + Gin “Hello World” suivante :
  ```go
  // imports above
  func main() {
      router := gin.Default()
      router.GET("/", func(c *gin.Context) {
          c.String(http.StatusOK, "Hello, World!")
      }
      router.Run(":3000")
  }
  ```
  {: codeblock}

2. Obtenez le package avec la commande suivante :
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. Ajoutez `promhttp` aux importations :
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  Une route de métriques supplémentaire doit également être ajoutée au routeur.

  Le code révisé ressemble à présent à l'exemple suivant :
  ```go
  // imports above
  func main() {
      router := gin.Default()
      router.GET("/", func(c *gin.Context) {
          c.String(http.StatusOK, "Hello, World!")
      }
      router.GET("/metrics", gin.WrapH(promhttp.Handler()))
      router.Run(":3000")
  }
  ```
  {: codeblock}

## Utilisation de métriques d'application dans les kits de démarrage

Les applications Go côté serveur qui sont créées à partir des kits de démarrage possèdent automatiquement le [noeud final Prometheus](https://prometheus.io/) sous `http://<hostname>:<port>/metrics`. Le code de ce noeud final se trouve dans `server.go`.

Pour plus d'informations, voir le [référentiel GitHub pour Prometheus](https://github.com/prometheus/client_golang/).
