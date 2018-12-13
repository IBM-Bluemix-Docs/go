---

copyright:
  years: 2018
lastupdated: "2018-10-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuration de la tolérance aux pannes dans les applications Go
{: #fault-tolerance}

La tolérance aux pannes permet à une application de continuer de s'exécuter si un composant tombe en panne ou ne répond plus. Vous pouvez ajouter une tolérance aux pannes à une application Go existante ou activer ces fonctions à partir d'une application Go générée. Ce tutoriel se explique comment utiliser le [package Hystrix](https://godoc.org/github.com/afex/hystrix-go/hystrix) pour ajouter une prise en charge de la tolérance aux pannes à une application Go.

## Ajout de la tolérance aux pannes à une application Go existante
{: #add-fault-tolerance}

Dans l'emplacement où se trouve le fichier `Gopkg.toml` de votre application Go, entrez les commandes suivantes pour ajouter les packages requis à votre liste de dépendances :
```
dep ensure -add "github.com/afex/hystrix-go/hystrix"
```
{: codeblock}

Pour utiliser Hystrix, une fonction de middleware doit être ajoutée :
```go
func HystrixHandler(command string) gin.HandlerFunc {
  return func(c *gin.Context) {
    hystrix.Do(command, func() error {
      c.Next()
      return nil
    }, func(err error) error {
      //Handle failures
      return err
    })
  }
}
``` 
{: codeblock}

Le traitement des pannes peut changer en fonction du type d'application Go utilisée. Pour les applications Web, une panne renvoie à une page 500, alors que les micro-services renvoient un objet JSON qui spécifie l'erreur.

Ce middleware accepte une chaîne de caractères qui est une commande configurée. La configuration d'une commande doit s'effectuer dans la fonction `main` comme suit :
```go
hystrix.ConfigureCommand("mycommand", hystrix.CommandConfig{
  Timeout:               1000,
  MaxConcurrentRequests: 100,
  ErrorPercentThreshold: 25,
})
```
{: codeblock}

Une fois qu'une commande Hystrix a été configurée, le middleware peut être ajouté au routeur avec l'instruction suivante :
```go
router.Use(HystrixHandler("mycommand"))
```
{: codeblock}

## Exposition des métriques Hystrix à Prometheus (facultatif)

Avant d'ajouter Hystrix à Prometheus, votre application doit être configurée avec des métriques d'application. Pour ajouter la prise en charge des métriques d'application, suivez les étapes décrites dans la rubrique [Utilisation des métriques d'application avec les applications Go](/docs/go/appmetrics.html).

Hystrix permet aux utilisateurs de prendre des données de métriques et de les exposer à un collecteur de métriques. Pour pouvoir exposer Hystrix à Prometheus, vous devez également ajouter le package metric_collector :
```
dep ensure -add "github.com/afex/hystrix-go/hystrix/metric_collector"
```
{: codeblock}

En plus de `metric_collector`, un fichier additionnel, `prometheus_collector.go`, doit être ajouté à votre application Go. Ce fichier se trouve [ici](https://github.com/ibm-developer/generator-ibm-core-golang-gin/blob/develop/generators/app/templates/plugins/prometheus_collector.go). Ce fichier doit être ajouté au package `plugins`.

Deux importations additionnelles sont requises :
```go
import(
  "github.com/afex/hystrix-go/hystrix/metric_collector"
  "<app name>/plugins"
)
```
{: codeblock}

Dans la fonction `main`, ajoutez les instructions suivantes :
```go
collector := plugins.InitializePrometheusCollector(plugins.PrometheusCollectorConfig{
  Namespace: "<app name>",
})
metricCollector.Registry.Register(collector.NewPrometheusCollector)
```
{: codeblock}

Pour afficher les métriques fournies par Hystrix, exécutez le projet et accédez à `/metrics`.
