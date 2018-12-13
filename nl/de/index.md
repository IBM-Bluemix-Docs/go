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

# Lernprogramm 'Einführung'

Das folgende Lernprogramm führt Sie durch die einzelnen Schritte zum Erstellen und Bereitstellen einer Go-App unter Verwendung der von {{site.data.keyword.cloud_notm}} bereitgestellten Tools. Sie können die [{{site.data.keyword.dev_cli_long}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) in der Befehlszeile oder den webbasierten [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) verwenden, wie in den folgenden Schritten des Lernprogramms dargestellt. Mit beiden dieser Methoden können Sie in nur wenigen Minuten eine für den Produktionsbetrieb bereite Go-Anwendung erstellen.

## Schritt 1. Go-App im Dashboard erstellen
{: #create_project}

1. Wählen Sie im App-Service auf der Seite für [Starter-Kits](https://console.bluemix.net/developer/appservice/starter-kits) ein Starter-Kit aus, das in `Go` geschrieben ist. Sie können auch eine leere Starter-App erstellen, indem Sie auf **App erstellen** klicken und `Go` als Sprache auswählen.

    Um ein Projekt erstellen zu können, müssen Sie bei einem {{site.data.keyword.cloud_notm}}-Konto angemeldet sein. Wenn Sie (noch) kein Konto besitzen, können Sie sich [für ein kostenloses Konto registrieren](https://console.bluemix.net/registration).
    {: tip}

3. Klicken Sie auf **App erstellen**.
4. Geben Sie Ihrer App mit **Name** einen Namen oder verwenden Sie den bereitgestellten generischen Namen.
5. Geben Sie einen **eindeutigen Hostnamen** ein. Der Hostname wird zum Zugreifen auf Ihre Anwendung verwendet, zum Beispiel `server.mybluemix.net`.
6. Klicken Sie auf **Erstellen**. Nachdem Ihr Projekt erstellt worden ist, können Sie es mithilfe einer Toolchain bereitstellen oder weiterentwickeln und dann über die [Befehlszeile](/docs/cli/idt/index.html) bereitstellen.

## Schritt 2. Mit dem Dashboard bereitstellen
{: #deploy_dashboard}

1. Klicken Sie auf der Seite für App-Details auf **In Cloud bereitstellen**.
2. Wählen Sie als Nächstes eine der folgenden Bereitstellungsmethoden aus:
    * **In Kubernetes bereitstellen** - Sie müssen eine Gruppe von Workerknoten erstellen. Sie können zum Beispiel VMs verwenden, um hoch verfügbare Anwendungscontainer bereitzustellen und zu verwalten. Sie können einen Cluster erstellen oder die Bereitstellung in einem vorhandenen Cluster vornehmen.
    * **In Cloud Foundry bereitstellen** - Sie müssen die zugrunde liegende Infrastruktur nicht verwalten.
    * **Auf einem virtuellen Server implementieren (Beta)** - Für den Zugriff auf virtuelle Server ist ein [nutzungsabhängiges Konto](https://console.bluemix.net/dashboard/ibm-iaas-g1) erforderlich.
3. Wählen Sie Ihre Optionen aus und klicken Sie dann auf **Erstellen**. Es wird eine Toolchain für Sie erstellt, die Ihre App automatisch bereitstellt.

## Schritt 3. Service hinzufügen (optional)
{: #add_service}

Vom Dashboard aus können Sie zügig Services wie Sicherheit oder Speicher zu Ihrer Go-App hinzufügen.

1. Kehren Sie zu Ihrer Go-App im [IBM Cloud App Service](https://console.bluemix.net/developer/appservice/dashboard) zurück.
2. Klicken Sie auf **Ressource hinzufügen**, wählen Sie die Kategorie von Service aus, die Sie hinzufügen wollen, klicken Sie auf **Weiter** und wählen Sie Ihren Service aus. Auf der Grundlage des ausgewählten Plans erstellt {{site.data.keyword.cloud_notm}} App Service den Service für Sie. Wenn Sie den Service, den Sie künftig verwenden wollen, zuvor erstellt haben, wählen Sie die Kategorie **Vorhanden** aus.
3. Nachdem der Service erstellt worden ist, klicken Sie auf **Code herunterladen**, um das Projekt mit dem SDK, das eine Verbindung zu Ihrem Service erstellt, erneut zu generieren.

## Schritt 4. Go-App testen und lokal auf sie zugreifen
{: #run_local}

Mittels `ibmcloud dev`-Befehlen können Sie die [{{site.data.keyword.dev_cli_short}}](https://console.bluemix.net/docs/cloudnative/dev_cli.html#add-cli) für die lokale Zusammenarbeit mit Ihrer Go-App verwenden.

1. Verwenden Sie den Befehl `ibmcloud dev build`, um Ihre Anwendung zu erstellen.
2. Verwenden Sie den Befehl `ibmcloud dev run`, um die Anwendung lokal auszuführen. Ihre Anwendung wird in den Docker-Containern auf Ihrem lokalen System ausgeführt. Durch Zugreifen auf `http://localhost:8080` können Sie Ihre Anwendung in einem Browser testen.
3. Ein Endpunkt für die Statusprüfung steht unter `http://localhost:8080/health` zur Verfügung.
4. Unter `http://localhost:3000/metrics` können Sie auf Metriken zugreifen.

In der vollständigen [CLI-Dokumentation](/docs/cli/idt/index.html) finden Sie weitere Informationen zur {{site.data.keyword.dev_cli_long}}.

## Alternative Bereitstellungsmethoden erkunden
{: #deploy_cloud notoc}

Wie Sie Ihre Anwendung in Cloud Foundry oder Kubernetes unter der Registerkarte 'Bereitstellungen' bereitstellen, erfahren Sie [hier](/docs/go/deploying_apps.html). 

Wenn Sie Ihre Go-Anwendung erweitern möchten, prüfen Sie den Inhalt der Abschnitte im Programmierhandbuch für Go.
