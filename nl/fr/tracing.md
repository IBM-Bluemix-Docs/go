---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: how to trace go apps, tracing go, jaeger go, opentracing go, jaeger packages, debug go app, troubleshoot go, go app help

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuration du traçage dans les applications Go
{: #go-e2e-tracing}

Ce tutoriel est consacré aux packages Opentracing et Jaeger pour le traçage des applications Go. Pour plus d'informations sur l'utilisation de Jaeger, accédez au [portail de documentation Jaeger](https://www.jaegertracing.io/docs/1.11/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").

Dans les étapes suivantes, deux petites applications (une frontale et une dorsale) sont utilisées pour effectuer un traçage entre deux points finaux à l'aide du module Jaeger. Vous pouvez démarrer à partir de zéro ou appliquer les principes décrits ici à vos applications Go existantes.

## Etape 1. Installation et activation des packages Opentracing et Jaeger
{: #install-go-opentracing}

1. Dans l'emplacement où se trouve le fichier `Gopkg.toml` de votre application Go, entrez les commandes suivantes pour ajouter les packages requis à votre liste de dépendances :
  ```go
  dep ensure -add "github.com/opentracing/opentracing-go"
  dep ensure -add "github.com/uber/jaeger-client-go"
  dep ensure -add "github.com/uber/jaeger-lib/metrics/prometheus"
  ```
  {: codeblock}

2. Ajoutez les lignes suivantes à votre code de serveur Go :
  ```go
  import(
  "github.com/opentracing/opentracing-go"
	"github.com/opentracing/opentracing-go/ext"
	"github.com/uber/jaeger-client-go"
	jaegerprom "github.com/uber/jaeger-lib/metrics/prometheus"
  )
  ```
  {: codeblock}

### Ajout du traçage à votre application serveur
{: #add-tracing-go}

Quelques instructions sont nécessaires pour ajouter un traçage à votre application serveur. Vous devez commencer par créer un traceur.

Pour créer un traceur, fournissez les informations suivantes :
 * Transporteur
 * Générateur de rapport
 * Exporteur de métriques facultatif
 
Dans ce tutoriel, Jaeger exporte des métriques de style Prometheus. Un objet metrics permet à Jaeger de signaler des métriques dans Prometheus.

1. Pour créer un objet metrics, ajoutez les instructions suivantes à la fonction principale :
  ```go
  factory := jaegerprom.New()
  metrics := jaeger.NewMetrics(factory, map[string]string{"lib": "jaeger"})
  ```
  {: codeblock}

2. Fournissez l'instruction suivante pour un transporteur, qui indique à Jaeger où envoyer ses traces. Dans un développement local, la destination est `localhost` sur le port `5775`. Bien que le nom d'hôte puisse changer, le port est `5775` dans la plupart des cas.
  ```go
  transport, err := jaeger.NewUDPTransport("<hostname>:<port>", 0)
  if err != nil {
	log.Errorln(err.Error())
  }
  ```
  {: codeblock}

3. Un générateur de rapport indique à Jaeger comment générer des rapports sur ses données de traçage. Trois types de générateurs de rapport sont utilisés :
  * Journalisation - imprime les données d'intervalle sous forme de journal
  * Distant - envoie les données d'intervalle à un agent Jaeger
  * Composite - combine plusieurs générateurs de rapports

  Comme les kits de démarrage Go utilisent Logrus pour consigner les données, la génération de rapports sur les intervalles Jaeger n'est pas possible par défaut. Cependant, Jaeger prend en charge une interface de journalisation qui peut être utilisée en ajoutant un adaptateur de journalisation avec l'instruction suivante :
  ```go
  type LogrusAdapter struct{}

  func (l LogrusAdapter) Error(msg string) {
	log.Errorf(msg)
  }

  func (l LogrusAdapter) Infof(msg string, args ...interface{}) {
	log.Infof(msg, args)
  }
  ```
  {: codeblock}

4. Pour créer un générateur de rapport consignant et générant des rapports sur les intervalles, ajoutez l'instruction suivante :
  ```go
  logAdapt := LogrusAdapter{}
  reporter := jaeger.NewCompositeReporter(
	jaeger.NewLoggingReporter(logAdapt),
	jaeger.NewRemoteReporter(transport,
		jaeger.ReporterOptions.Metrics(metrics),
		jaeger.ReporterOptions.Logger(logAdapt),
	),
  )
  defer reporter.Close()
  ```
  {: codeblock}

5. Un objet sampler détermine dans quelles situations ou avec quelle fréquence les intervalles sont signalés. Pour le développement, une application doit signaler tous les intervalles qu'il reçoit. Par contre, pour la production, il se peut que le signalement de tous les intervalles ne soit pas possible. Pour signaler tous les intervalles, vous pouvez utiliser l'objet ConstSampler :
  ```go
  sampler := jaeger.NewConstSampler(true)
  ```
  {: codeblock}

6. Maintenant que vous avez créé les objets reporter, sampler et metrics, vous pouvez créer un traceur.
  ```go
  tracer, closer := jaeger.NewTracer("<application name>",
	sampler,
	reporter,
	jaeger.TracerOptions.Metrics(metrics),
  )
  defer closer.Close()

  opentracing.SetGlobalTracer(tracer)
  ```
  {: codeblock}

7. Ajoutez une fonction middleware qui extrait les données d'intervalle d'une demande du client ou crée un nouvel objet d'intervalle racine :
  ```go
  func OpenTracing() gin.HandlerFunc {
	return func(c *gin.Context) {
		wireCtx, _ := opentracing.GlobalTracer().Extract(
			opentracing.HTTPHeaders,
			opentracing.HTTPHeadersCarrier(c.Request.Header))

		serverSpan := opentracing.StartSpan(c.Request.URL.Path,
			ext.RPCServerOption(wireCtx))
		defer serverSpan.Finish()
		c.Request = c.Request.WithContext(opentracing.ContextWithSpan(c.Request.Context(), serverSpan))
		c.Next()
	}
  }
  ```
  {: codeblock}

8. Ajoutez le middleware au routeur. Vous **DEVEZ** ajouter cette instruction avant que les noeuds finaux soient créés :
  ```go
  router.Use(OpenTracing)
  ...
  router.GET("/health", routes.HealthGET)
  ```
  {: codeblock}

  Cette instruction configure une application qui peut extraire un intervalle et signaler ensuite de nouvelles données d'intervalle. Par défaut, tous les kits de démarrage Go implémentent entièrement OpenTracing côté serveur.

9. Pour envoyer les données d'intervalle d'un service à un autre, vous devez ajouter quelques instructions supplémentaires. Voir l'exemple de demande suivant :
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)
  client.Do(req)
  ```
  {: codeblock}

10. Pour envoyer des données d'intervalle avec cette demande, ajoutez deux instructions après avoir créé la demande :
  ```go
  client := http.Client{}
  req, _ := http.NewRequest("GET", "http://localhost:3000", nil)

  span := opentracing.SpanFromContext(c.Request.Context())
  opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

  client.Do(req)
  ```
  {: codeblock}

## Etape 2. Configuration d'un serveur Jaeger local
{: #setup-jaeger-server}

Les serveurs Jaeger comptent trois services différents :
 * Agents
 * Collecteurs
 * Requêtes
 
Les services Go se connectent à l'agent à l'aide de l'objet `UDPTransport`. Les agents transmettent ensuite les données aux collecteurs qui stockent les données d'intervalle dans une base de données. Le service de requête permet ensuite à l'utilisateur de récupérer les intervalles dans la base de données. Les services sont séparées pour des raisons de flexibilité car tous les services doivent avoir leurs propres agents auxquels se connecter, tandis que le nombre de services de collecteurs et de requêtes peut être adapté en fonction des besoins.

Cependant, pour le développement local, Jaeger fournit un service tout-en-un, qui est fourni avec les services agent, collecteur, base de données et requête regroupés. Cette configuration est utile pour le développement local, mais vous ne devez pas l'utiliser en production.

Avant de déployer votre application dans un cloud, vous pouvez tester le traçage localement.

Pour en savoir plus sur le déploiement de Jaeger dans un conteneur à l'aide de Kubernetes, voir [ces étapes](#jaeger-kube).

Pour exécuter le service tout-en-un localement, exécutez la commande suivante :
```bash
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14268:14268 \
  -p 9411:9411 \
  jaegertracing/all-in-one:latest
 ```
{: codeblock}

L'agent peut être connecté sur le port `5775`, tandis que la requête peut être connectée sur le port `16686`.

### Configuration d'un serveur Jaeger déployé dans Kubernetes
{: #jaeger-kube}

Comme pour le développement local, Jaeger fournit un service tout-en-un pour le développement dans Kubernetes. Utilisez le service tout-en-un uniquement pour le développement, et non pour le code de production. Pour plus d'informations sur le déploiement dans Kubernetes pour la production, consultez le [guide Jaeger Kubernetes Templates](https://github.com/jaegertracing/jaeger-kubernetes#production-setup){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe").

Pour déployer le serveur Jaeger, procédez comme suit :
1. Vérifiez que votre cluster est configuré en exécutant `ibmcloud cs cluster-config <cluster name>` et suivez les instructions.
2. Exécutez les commandes suivantes :
  ```go
  kubectl create -f https://raw.githubusercontent.com/jaegertracing/jaeger-kubernetes/master/all-in-one/jaeger-all-in-one-template.yml
  ```
  {: codeblock}

  Cette commande déploie un agent, un collecteur et une requête Jaeger dans votre cluster Kubernetes.
3. Avant de déployer l'application Go dans Kubernetes, vous devez mettre à jour le transport UDP pour pointer correctement vers l'agent Jaeger. La commande `kubectl` de l'étape précédente crée un noeud final interne appelé `"jaeger-agent:5775"`. Mettez à jour le transport avec ce nouveau point final.
  ```go
  transport, err := jaeger.NewUDPTransport("jaeger-agent:5775", 0)
  ```
  {: codeblock}

4. Une fois que l'application a été déployée, vous pouvez afficher les données de traçage en accédant à <*public cluster ip*>:<*port*>. Pour obtenir l'adresse IP du cluster public, exécutez `bx cs workers <*cluster name*>`.
Pour obtenir le port, exécutez `kubectl get service jaeger-query`.

## Etape 3. Test d'un exemple de scénario
{: #test-go-tracing}

Lorsque vous suivez les étapes précédentes, il est simple de créer deux applications Go séparées qui prennent en charge le traçage. Vous pouvez ajouter une route vers l'un des projets avec le code suivant :
```go
client := http.Client{}
req, _ := http.NewRequest("GET", "http://localhost:<other port>", nil)

span := opentracing.SpanFromContext(c.Request.Context())
opentracing.GlobalTracer().Inject(span.Context(), opentracing.HTTPHeaders, opentracing.HTTPHeadersCarrier(req.Header))

client.Do(req)
```
{: codeblock}

Cette route envoie une demande `GET` d'une application à une autre.

Pour afficher l'intervalle, accédez à `http://localhost:16686`. Recherchez des traces par service, opération et balises, puis cliquez sur **Find Traces**.

![Interface utilisateur Jaeger](images/JaegerUI.png)

Cliquez sur une trace spécifique pour afficher des informations supplémentaires à son sujet :
![Exemple de trace](images/TraceExample.png)
