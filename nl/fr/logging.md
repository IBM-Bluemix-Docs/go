---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-30"

keywords: how to log in go, go logging, debug go apps, go troubleshooting, logrus go, go stdout

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Journalisation de Go
{: #logging-golang}

Les messages de journal sont des chaînes qui contiennent des informations contextuelles sur l'état et l'activité du micro-service au moment où l'entrée de journal est créée. Les journaux sont nécessaires pour diagnostiquer comment et pourquoi les services échouent, et jouent un rôle de support pour les [métriques](/docs/go?topic=go-go-appmetrics) dans la surveillance de l'intégrité des applications.

Compte tenu de la nature transitoire des processus dans les environnements de cloud, les journaux doivent être collectés et envoyés ailleurs, généralement dans un emplacement centralisé pour analyse. Le moyen le plus cohérent de consigner des événements dans les environnements de cloud est d'envoyer les entrées de journal dans des flux d'erreur et de sortie standard, ce qui permet à l'infrastructure de traiter le reste.

## Ajout de la prise en charge de Logrus à une application Go
{: #add-logrus-go}

[Logrus](https://github.com/sirupsen/logrus){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") est une infrastructure de journalisation populaire pour Go qui offre de nombreux avantages natifs, tels que : 
 * Journalisation dans `stdout` ou `stderr`
 * Diverses options d'ajout
 * Agencement et modèles de message de journal configurables
 * Utilisation de niveaux de journalisation pour différentes catégories de journal

1. Pour utiliser Logrus, exécutez la commande suivante dans le répertoire racine de votre application, qui installe le package et l'ajoute à votre fichier `Gopkg.toml`.
  ```bash
  dep ensure -add https://github.com/sirupsen/logrus
  ```
  {: codeblock}

2. Pour l'utiliser dans votre application, ajoutez les lignes de code suivantes :
  ```go
  import log "github.com/sirupsen/logrus"
  log.SetFormatter(&log.JSONFormatter{})
  log.SetOutput(os.Stdout)
  log.Info("My message")
  ```
  {: codeblock}

  **Sortie Stdout **:
  ```
  {"level":"info","msg":"My message","time":"2018-08-03T11:05:02-05:00"}
  ```
  {: screen}

Pour plus d'informations sur la personnalisation des messages de journal avec des programmes d'ajout, des niveaux de journal et des détails de configuration, voir la [documentation Logrus](https://godoc.org/gopkg.in/Sirupsen/logrus.v0){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe") officielle.

## Etapes suivantes
{: #go-logging-next notoc}

En savoir plus sur l'affichage des journaux dans chacune de vos cibles de déploiement :
* [Journaux Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/logging/){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")
* [Journaux Cloud Foundry](/docs/cli/reference/bluemix_cli?topic=cloud-cli-ibmcloud_cli#ibmcloud_app_logs)
* [Cloud Foundry Enterprise Environment - Audit et journalisation](/docs/cloud-foundry?topic=cloud-foundry-auditing-logging#auditing-logging)
* [Journaux {{site.data.keyword.openwhisk}} et surveillance](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#openwhisk_logs)

Pour en savoir plus sur l'utilisation d'un regroupeur de journaux :
* [Analyse de journal {{site.data.keyword.cloud_notm}}](/docs/services/CloudLogAnalysis?topic=cloudloganalysis-log_analysis_ov#log_analysis_ov)
* [Pile ELK privée {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.2/manage_metrics/logging_elk.html){: new_window} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")
