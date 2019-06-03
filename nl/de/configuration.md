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

# Go-Umgebung konfigurieren
{: #configure-go-env}

Es stehen standardisierte Richtlinien zur Verfügung, deren Einhaltung bei der Entwicklung von Go-Anwendungen dabei hilft, die Portierbarkeit durchgängig sicherzustellen. Zu den Aspekten, die berücksichtigt werden müssen, zählen unter anderem die Verwaltung von Berechtigungsnachweisen, die Speicherung von Daten und die Publikation von Inhalten in der Cloud. Die Einhaltung von Cloud Native-Verfahren ermöglicht, dass eine Go-Anwendung problemlos von einer Umgebung zu einer anderen wechseln kann. So ist zum Beispiel der Wechsel von einer Test- zu einer Produktionsumgebung ohne Änderungen am Code und ohne Beanspruchung anderweitig nicht getesteter Codepfade möglich.

Unabhängig davon, ob Sie Cloudunterstützung zu vorhandenen Go-Anwendungen hinzufügen oder Go-Apps mit Starter-Kits erstellen müssen, besteht das Ziel darin, Portierbarkeit für die Verwendung auf einer beliebigen Entwicklungsplattform bereitzustellen.

## Cloudunterstützung zu vorhandenen Go-Anwendungen hinzufügen
{: #go-add-cloud-support}

Das Modul [`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") fasst Umgebungsvariablen von verschiedenen Cloud-Providern wie Cloud Foundry und Kubernetes zusammen, sodass die Anwendung von der Umgebung unabhängig ist.

### Modul `ibm-cloud-env-golang` installieren
{: #go-install-env-module}

1. Installieren Sie das Modul `ibm-cloud-env-golang` mit dem folgenden Befehl:
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. Initialisieren Sie das Modul in Ihrem Code, indem Sie auf die Datei `mappings.json` verweisen:
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Da Go keine Standardparameter unterstützt, wird keine Standardversion der Datei `mappings.json` bereitgestellt. Wenn der Pfad der Zuordnungsdatei nicht in `IBMCloudEnv.Initialize()` angegeben ist, wird ein Fehler protokolliert. 
  {: tip}

  Beispielversion der Datei `mappings.json`:
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

### Serviceberechtigungsnachweise abrufen
{: #go-get-creds}

Rufen Sie die Werte in Ihrer Anwendung mit den nachfolgend aufgeführten Befehlen ab.

1. Variable `service1credentials` abrufen:
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. Variable `service2username` abrufen:
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

Jetzt kann Ihre Anwendung in jeder beliebigen Laufzeitumgebung implementiert werden, indem Sie die Unterschiede, die sich durch die einzelnen Cloud-Computing-Provider ergeben, entsprechend abstrahieren.

### Werte nach Tags und Bezeichnungen filtern
{: #go-filter-creds}

Sie können die vom Modul generierten Berechtigungsnachweise anhand von Servicetags und Servicebezeichnungen wie im folgenden Beispiel dargestellt filtern:
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // Rückgabe einer JSON-Datei mit Berechtigungsnachweisen für angegebenen Service-Tag und angegebene Bezeichnung
```
{: codeblock}

## Go-Konfigurationsmanager von Starter-Kit-Apps verwenden
{: #go-config-manager}

Mit [Starter-Kits](https://cloud.ibm.com/developer/appservice/starter-kits){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") erstellte Go-Apps werden automatisch mit Berechtigungsnachweisen und Konfigurationen ausgestattet, die für die Ausführung in vielen Cloudbereitstellungszielen wie Cloud Foundry, Kubernetes, VSI und Functions erforderlich sind.

### Wissenswertes zu Serviceberechtigungsnachweisen
{: #go-credentials-config}

Ihre Anwendungskonfigurationsdaten für Services werden in der Datei `localdev-config.json` im Verzeichnis `/server/config` gespeichert. Die Datei befindet sich im Verzeichnis `.gitignore`, um zu verhindern, dass sensible Informationen in Git gespeichert werden. In dieser Datei werden die Verbindungsinformationen (wie Benutzername, Kennwort und Hostname) für jeden konfigurierten Service gespeichert, der lokal ausgeführt wird.

Zum Lesen der Verbindungs- und Konfigurationsinformationen aus der Umgebung und dieser Datei verwendet die Anwendung den Konfigurationsmanager. Mittels einer kundenspezifischen Datei `mappings.json`, die sich im Verzeichnis `server/config` befindet, kommuniziert sie, wo sich die Berechtigungsnachweise für die einzelnen Services befinden.

Lokal ausgeführte Anwendungen können mithilfe von nicht gebundenen Berechtigungsnachweisen, die aus der Datei `mappings.json` gelesen werden, eine Verbindung zu {{site.data.keyword.cloud_notm}}-Services herstellen. Wenn Sie nicht gebundene Berechtigungsnachweise erstellen müssen, können Sie dazu die {{site.data.keyword.cloud_notm}}-Webkonsole (Beispiel) oder den Befehl `cf create-service-key` der [Cloud Foundry-CLI](https://docs.cloudfoundry.org/cf-cli/){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") verwenden.

Wenn Sie Ihre Anwendung mit einer Push-Operation an {{site.data.keyword.cloud_notm}} übertragen, werden diese Werte nicht mehr verwendet. Stattdessen stellt die Anwendung mithilfe von Umgebungsvariablen automatisch eine Verbindung zu gebundenen Services her. 

* **Cloud Foundry**: Die Serviceberechtigungsnachweise werden aus der Umgebungsvariablen `VCAP_SERVICES` abgerufen.

* **Kubernetes**: Die Serviceberechtigungsnachweise werden aus einzelnen Umgebungsvariablen pro Service abgerufen.

* **{{site.data.keyword.cloud_notm}} Container Service**: Die Serviceberechtigungsnachweise werden aus virtuellen Serverinstanzen oder {{site.data.keyword.openwhisk}} (Openwhisk) abgerufen.

## Nächste Schritte
{: #go-next-steps-config notoc}

Das Modul `ibm-cloud-env-golang` unterstützt die Suche nach Werten unter Verwendung von vier Suchmustertypen: `user-provided`, `cloudfoundry`, `env` und `file`. Wenn Sie sich weitere unterstützte Suchmuster und Suchmusterbeispiele ansehen möchten, lesen Sie den Abschnitt zur [Syntax](https://github.com/ibm-developer/ibm-cloud-env-golang#usage){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link").
