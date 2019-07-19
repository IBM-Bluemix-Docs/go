---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: create go app, ibmcloud dev go, cli go, create go app locally, deploy go app, go starter kit

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Lernprogramm 'Einführung'
{: #getting-started}

Das folgende Lernprogramm führt Sie durch die Schritte zum Erstellen und Bereitstellen einer Go-Anwendung mithilfe der von {{site.data.keyword.cloud_notm}} bereitgestellten Tools. Sie können die [{{site.data.keyword.dev_cli_long}}](/docs/cli?topic=cloud-cli-getting-started) über die Befehlszeile oder den webbasierten [{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}](https://{DomainName}/developer/appservice/dashboard){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") verwenden, wie in den folgenden Schritten des Lernprogramms beschrieben. Mit beiden dieser Methoden können Sie in nur wenigen Minuten eine einsatzbereite Go-App erstellen. 

## Schritt 1. Angepasste Go-App im Dashboard erstellen
{: #create-go-app}

1. Melden Sie sich bei einem {{site.data.keyword.cloud_notm}}-Konto an, um eine App zu erstellen. Wenn Sie nicht über ein Konto verfügen, können Sie sich [für ein kostenloses Konto registrieren](https://{DomainName}/registration){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link").
2. Führen Sie auf der Seite mit den [Starter-Kits](https://{DomainName}/developer/appservice/starter-kits){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") im {{site.data.keyword.dev_console}} eine der folgenden Aktionen aus:
 * Wählen Sie ein Starter-Kit aus, das in `Go` geschrieben ist, und klicken Sie dann auf der Seite mit den Details zum Starter-Kit**** auf **App erstellen**.
 * Wählen Sie die leere Starter-App aus und klicken Sie auf **App erstellen**.
3. Geben Sie einen Namen für Ihre App an oder verwenden Sie den bereitgestellten generischen App-Namen.
4. Stellen Sie sicher, dass **Go** als Plattform ausgewählt ist, und klicken Sie dann auf **Erstellen**. Nach der Erstellung Ihrer App können Sie Services hinzufügen und die App anschließend mithilfe einer Toolchain bereitstellen. Sie können Ihr Projekt jedoch auch über die [Befehlszeile](/docs/cli?topic=cloud-cli-getting-started) weiterentwickeln und bereitstellen.

## Schritt 2. {{site.data.keyword.watson}} {{site.data.keyword.texttospeechshort}} hinzufügen
{: #add-resource-go}

Sie können Ihrer Go-App jetzt {{site.data.keyword.watson}}-Services hinzufügen. Fügen Sie Ihrer Go-App für dieses Lernprogramm den {{site.data.keyword.texttospeechshort}}-Service hinzu. Dieser Service konvertiert verbale Eingabe mithilfe einer Cloud-API in Text. 

1. Auf der Seite **App-Details** klicken Sie auf **Service hinzufügen**.
2. Wählen Sie **Künstliche Intelligenz** aus und klicken Sie auf **Weiter**.
3. Wählen Sie **{{site.data.keyword.texttospeechshort}}** aus und klicken Sie auf **Weiter**.
4. Klicken Sie auf **Erstellen**.

Sie sehen, dass die Abhängigkeit [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") der Datei `Gopkg.toml` hinzugefügt wurde und einfacher Instrumentierungscode für den Service im Verzeichnis `services/` verfügbar ist. Zudem sind Konfigurationsinformationen für den Zugriff auf die Serviceberechtigungsnachweise in der jeweiligen Umgebung enthalten.

Zum Herunterladen des Codes klicken Sie auf **Code herunterladen** auf der Seite **App-Details**. In dem heruntergeladenen Code ist auch das [Watson Developer Cloud Go SDK](https://github.com/watson-developer-cloud/go-sdk){: new_window} ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link") sowie grundlegender Initialisierungscode enthalten.

## Schritt 3. App über die Konsole bereitstellen
{: #deploy-go}

1. Auf der Seite **App-Details** klicken Sie auf **Continuous Delivery konfigurieren**.
2. Richten Sie Ihr Bereitstellungsziel ein, indem Sie die Anweisungen für die von Ihnen ausgewählte Methode ausführen:
  * **Bereitstellung in [IBM Kubernetes Service](/docs/containers?topic=containers-app)**. Mit dieser Option wird ein Cluster mit Hosts erstellt, die als Workerknoten bezeichnet werden, um hoch verfügbare App-Container bereitzustellen und zu verwalten. Sie können einen Cluster erstellen oder die Bereitstellung in einem vorhandenen Cluster vornehmen.
  * **Bereitstellung in Cloud Foundry**. Mit dieser Option wird die cloudnative App bereitgestellt, ohne dass Sie die zugrunde liegende Infrastruktur verwalten müssen. Wenn Ihr Konto über Zugriff auf {{site.data.keyword.cfee_full_notm}} verfügt, können Sie als Bereitstellertyp entweder **[Public Cloud](/docs/cloud-foundry-public?topic=cloud-foundry-public-about-cf)** oder **[Enterprise Environment](/docs/cloud-foundry-public?topic=cloud-foundry-public-cfee)** auswählen, das zum Erstellen und Verwalten isolierter Umgebungen für das exklusive Hosting von Cloud Foundry-Apps für Ihr Unternehmen verwendet werden kann. 
  * **Bereitstellung auf einem [virtuellen Server](/docs/vsi?topic=virtual-servers-deploying-to-a-virtual-server)**. Mit dieser Option wird eine virtuelle Serverinstanz bereitgestellt, ein Image mit Ihrer App geladen, eine DevOps-Toolchain erstellt und der Bereitstellungszyklus für Sie initiiert. 

3. Nach der Einrichtung Ihres Bereitstellungsziels klicken Sie auf **Weiter**.
4. Wählen Sie die Konfigurationsoptionen aus und klicken Sie dann auf **Erstellen**. Es wird eine Toolchain für Sie erstellt und Ihre App wird automatisch bereitgestellt.

## Schritt 4. Go-App testen und lokal auf sie zugreifen
{: #run_local-go}

Auf der Seite **App-Details** können Sie folgende Aktionen ausführen:
* Das Repository anzeigen, indem Sie auf **Repository anzeigen** klicken.
* Die Toolchain anzeigen und ändern, indem Sie auf **Toolchain anzeigen** klicken.

Mittels `ibmcloud dev`-Befehlen können Sie die {{site.data.keyword.dev_cli_long}} für die lokale Zusammenarbeit mit Ihrer Go-App verwenden. Mit diesen Tools können Sie schnell lokal iterieren und Ihre Änderungen mit Push-Operationen in die Cloud übertragen.

1. Verwenden Sie den Befehl `ibmcloud dev build`, um Ihre App zu erstellen. 
2. Verwenden Sie den Befehl `ibmcloud dev run`, um die App lokal auszuführen. Ihre App wird in den Docker-Containern auf Ihrem lokalen System ausgeführt. Durch Zugreifen auf `http://localhost:8080` können Sie Ihre App in einem Browser testen. 
3. Ein Endpunkt für die Statusprüfung steht unter `http://localhost:8080/health` zur Verfügung.
4. Unter `http://localhost:3000/metrics` können Sie auf Metriken zugreifen.

In der vollständigen [CLI-Dokumentation](/docs/cli?topic=cloud-cli-getting-started) finden Sie weitere Informationen zur {{site.data.keyword.dev_cli_notm}}.

Sie können jetzt Ihre App mit dem {{site.data.keyword.texttospeechshort}}-Service weiterentwickeln! 

## Alternative Bereitstellungsmethoden erkunden
{: #alt-deploy-go notoc}

Lernen Sie, wie Sie Ihre App unter der [Registerkarte für Bereitstellungen](/docs/go?topic=go-go-deploy-apps) bereitstellen. 

Wenn Sie Ihre Go-App erweitern möchten, sehen Sie sich als Nächstes die Themen im Programmierungshandbuch zu Go an. 
