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

# Lernprogramm 'Einführung'
{: #getting-started}

Das folgende Lernprogramm führt Sie durch die einzelnen Schritte zum Erstellen und Bereitstellen einer Go-App unter Verwendung der von {{site.data.keyword.cloud_notm}} bereitgestellten Tools. Sie können die [{{site.data.keyword.dev_cli_long}}](/docs/cli/index.html#ibmcloud-cli) in der Befehlszeile oder die webbasierte [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://cloud.ibm.com/developer/appservice/dashboard) verwenden, wie in den folgenden Lernprogrammschritten beschrieben. Mit beiden dieser Methoden können Sie in nur wenigen Minuten eine für den Produktionsbetrieb bereite Go-Anwendung erstellen.

## Schritt 1. Go-App im Dashboard erstellen
{: #create-go-app}

1. Wählen Sie im App-Service auf der Seite für [Starter-Kits](https://cloud.ibm.com/developer/appservice/starter-kits) ein Starter-Kit aus, das in `Go` geschrieben ist. Sie können auch eine leere Starter-App erstellen, indem Sie auf **App erstellen** klicken und `Go` als Sprache auswählen.

    Sie müssen bei einem {{site.data.keyword.cloud_notm}}-Konto angemeldet sein, um eine App zu erstellen. Wenn Sie (noch) kein Konto besitzen, können Sie sich [für ein kostenloses Konto registrieren](https://cloud.ibm.com/registration).
    {: tip}

3. Klicken Sie auf **App erstellen**.
4. Geben Sie Ihrer App mit **Name** einen Namen oder verwenden Sie den bereitgestellten generischen Namen.
5. Geben Sie einen **eindeutigen Hostnamen** ein. Der Hostname wird zum Zugreifen auf Ihre Anwendung verwendet, zum Beispiel `server.mybluemix.net`.
6. Klicken Sie auf **Erstellen**. Nach der Erstellung Ihrer App können Sie sie mithilfe einer Toolchain bereitstellen oder weiterentwickeln und dann über die [Befehlszeile](/docs/cli/index.html#ibmcloud-cli) bereitstellen. 

## Schritt 2. Mit dem Dashboard bereitstellen
{: #deploy-go}

1. Klicken Sie auf der Seite für App-Details auf **In Cloud bereitstellen**.
2. Richten Sie die Bereitstellungsmethode ein, indem Sie die Anweisungen für die von Ihnen ausgewählte Methode ausführen: 
  * **Bereitstellung in [Kubernetes](/docs/apps/deploying/containers.html#containers)**. Mit dieser Option wird ein Cluster mit Hosts erstellen, die als Workerknoten bezeichnet werden, um hoch verfügbare Anwendungscontainer bereitzustellen und zu verwalten. Sie können einen Cluster erstellen oder die Bereitstellung in einem vorhandenen Cluster vornehmen.
  * **Bereitstellung in Cloud Foundry**. Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen. Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public/about-cf.html#about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public/cfee.html#cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Anwendungen für Ihr Unternehmen verwendet werden kann. 
  * **Bereitstellung auf einem [virtuellen Server](/docs/apps/vsi-deploy.html#vsi-deploy)**. Mit dieser Option wird eine virtuelle Serverinstanz eingerichtet, ein Image mit Ihrer App geladen, eine DevOps-Toolchain erstellt und der erste Bereitstellungszyklus initiiert. 

3. Wählen Sie Ihre Optionen aus und klicken Sie dann auf **Erstellen**. Es wird eine Toolchain für Sie erstellt, die Ihre App automatisch bereitstellt.

## Schritt 3. Ressource hinzufügen (optional)
{: #add-resource-go}

Vom Dashboard aus können Sie zügig Services wie Sicherheit oder Speicher zu Ihrer Go-App hinzufügen.

1. Kehren Sie zu Ihrer Go-App im [IBM Cloud App Service](https://cloud.ibm.com/developer/appservice/dashboard) zurück.
2. Klicken Sie auf **Ressource hinzufügen**, wählen Sie die Kategorie von Service aus, die Sie hinzufügen wollen, klicken Sie auf **Weiter** und wählen Sie Ihren Service aus. Auf der Grundlage des ausgewählten Plans erstellt {{site.data.keyword.cloud_notm}} App Service den Service für Sie. Wenn Sie den Service, den Sie künftig verwenden wollen, zuvor erstellt haben, wählen Sie die Kategorie **Vorhanden** aus.
3. Nachdem der Service erstellt worden ist, klicken Sie auf **Code herunterladen**, um das Projekt mit dem SDK, das eine Verbindung zu Ihrem Service erstellt, erneut zu generieren.

## Schritt 4. Go-App testen und lokal auf sie zugreifen
{: #run_local-go}

Mittels `ibmcloud dev`-Befehlen können Sie die [{{site.data.keyword.dev_cli_short}}](/docs/cli/index.html#ibmcloud-cli) für die lokale Zusammenarbeit mit Ihrer Go-App verwenden.

1. Verwenden Sie den Befehl `ibmcloud dev build`, um Ihre Anwendung zu erstellen.
2. Verwenden Sie den Befehl `ibmcloud dev run`, um die Anwendung lokal auszuführen. Ihre Anwendung wird in den Docker-Containern auf Ihrem lokalen System ausgeführt. Durch Zugreifen auf `http://localhost:8080` können Sie Ihre Anwendung in einem Browser testen.
3. Ein Endpunkt für die Statusprüfung steht unter `http://localhost:8080/health` zur Verfügung.
4. Unter `http://localhost:3000/metrics` können Sie auf Metriken zugreifen.

In der vollständigen [CLI-Dokumentation](/docs/cli/index.html#ibmcloud-cli) finden Sie weitere Informationen zur {{site.data.keyword.dev_cli_long}}.

## Alternative Bereitstellungsmethoden erkunden
{: #deploy_cloud-go notoc}

Wie Sie Ihre Anwendung unter der Registerkarte 'Bereitstellungen' bereitstellen, erfahren Sie [hier](/docs/go/deploying_apps.html).  

Wenn Sie Ihre Go-Anwendung erweitern möchten, prüfen Sie den Inhalt der Abschnitte im Programmierhandbuch für Go.
