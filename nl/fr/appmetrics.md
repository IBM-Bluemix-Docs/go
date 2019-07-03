---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: prometheus go, application metrics go, view metrics go app, add metrics go, promhttp go, autoscaling go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilisation des métriques d'application avec les applications Go
{: #go-appmetrics}

Les métriques d'application sont importantes pour la surveillance des performances de votre application. Il est essentiel de disposer d'une vue en direct de métriques types UC, mémoire, latence et HTTP, pour garantir un fonctionnement efficace de votre application dans le temps. Vous pouvez utiliser un service cloud comme Cloud Foundry [Auto-Scaling](/docs/services/Auto-Scaling?topic=Auto-Scaling), qui repose sur des métriques pour adapter dynamiquement les instances aux charges de travail actuelles. En utilisant des métriques d'application, vous êtes informé précisément lorsqu'il faut augmenter ou réduire des instances, ou les nettoyer lorsqu'elles ne sont plus nécessaires, le tout pour limiter les coûts.

Les métriques d'application sont capturées sous forme de données de série temporelles. L'agrégation et la visualisation des métriques capturées peut permettre d'identifier des problèmes de performance courants :

* Faibles temps de réponse HTTP sur tout ou partie des routes
* Débit médiocre dans l'application
* Pics de demandes entraînant des ralentissements
* Utilisation de l'unité centrale plus élevée que prévu pour le niveau de débit ou de charge
* Utilisation de la mémoire élevée ou en augmentation (possible fuite de mémoire)

## Ajout de métriques d'application à votre application Go existante
{: #go-add-appmetrics-existing}

Pour ajouter une surveillance des performances à votre application Go, vous pouvez utiliser l'agrégation complète des métriques fournies par le package `promhttp`.

Le package `promhttp` inclut de nombreux points d'extension, notamment la [configuration de Prometheus](https://github.com/prometheus/client_golang){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").

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
{: #go-starterkits-appmetrics}

Les applications Go côté serveur créées à partir des kits de démarrage incluent automatiquement le [noeud final Prometheus](https://prometheus.io/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") sous `http://<hostname>:<port>/metrics`. Le code de ce noeud final se trouve dans `server.go`.

Pour plus d'informations, voir le [référentiel GitHub pour Prometheus](https://github.com/prometheus/client_golang/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").
