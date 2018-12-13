---

copyright:
  years: 2018
lastupdated: "2018-09-25"

---

{:tip: .tip}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Chatbot hinzufügen
{: #assistant}

Mit dem {{site.data.keyword.conversationshort}}-Service können Sie Go-Anwendungen erstellen, die Eingaben in natürlicher Sprache verstehen und mit einer menschenähnlichen Unterhaltung auf Benutzer reagieren.

Funktionsweise der Integration:

* Benutzer interagieren mit der Front-End-Benutzerschnittstelle Ihrer App.
* Ihre App sendet Benutzereingaben unter Verwendung des {{site.data.keyword.ibmwatson}} Go-SDKs an den {{site.data.keyword.conversationshort}}.
* Das {{site.data.keyword.watson}} Go-SDK stellt eine Verbindung zu einem Arbeitsbereich her, der ein Container für Ihren Dialogablauf und die Trainingsdaten ist.
* Der Arbeitsbereich interpretiert die Benutzereingaben und steuert den Ablauf des Dialogs. Dabei wird eine Antwort an Ihre App gesendet.
* Das Front-End Ihrer App teilt die Audioantwort als abspielbare `MP3`-Datei mit dem Benutzer oder als herunterladbare Datei.

Sie können den virtuellen Assistenten zu einer neuen Go-Starter-Kit-App oder zu einer vorhandenen Go-App hinzufügen.

## Vorbereitende Schritte
{: #before-you-begin}

Installieren Sie das {{site.data.keyword.watson}} Go-SDK:
```bash
go get github.com/watson-developer-cloud/go-sdk
```
{: codeblock}

## Virtuellen Assistenten zu einer vorhandenen Go-App hinzufügen
{: #add-a-virtual-assistant-to-your-app}

1. Öffnen Sie nach dem Herunterladen des Codes Ihr Projekt.

2. Fügen Sie eine Importanweisung für {{site.data.keyword.conversationshort}} hinzu.

  ```golang
  package main

  import (
    "fmt"
    "go-sdk/assistantv1"
    "encoding/json"
  )

  // Dient zum Drucken des formatierten Ergebnisses
  func prettyPrint(result interface{}, resultName string) {
    output, err := json.MarshalIndent(result, "", "    ")

    if err == nil {
      fmt.Printf("%v:\n%+v\n\n", resultName, string(output))
    }
  }
  ```
  {: codeblock}

3. Erstellen Sie eine Instanz des Service. Das Beispiel verwendet einen Service namens `Assistant`.

  ```golang
  func main() {
    // Instanz von Watson Assistant-Service erstellen
    assistant, assistantErr := assistantv1.NewAssistantV1(&ServiceCredentials{
      ServiceURL: "YOUR SERVICE URL",
      Version: "2018-07-10",
      Username: "YOUR SERVICE USERNAME",
      Password: "YOUR SERVICE PASSWORD",
    })

    // Erfolgreiche Instanzerstellung prüfen
    if assistantErr != nil {
      fmt.Println(assistantErr)
      return
    }
  ```
  {: codeblock}

4. Sie können mit dem Service interagieren, indem Sie die Methoden 'GET' und 'LIST' für einen Arbeitsbereich verwenden.

  Arbeitsbereich auflisten:
  ```golang
  // Methode 'ListWorkspaces' für 'Assistant' aufrufen
  list, listErr := assistant.ListWorkspaces(assistantv1.NewListWorkspacesOptions())

  // Erfolgreichen Aufruf prüfen
  if listErr != nil {
    fmt.Println(listErr)
    return
  }

  // Ergebnis von 'list.Result' in den von 'ListWorkspaces' zurückgegebenen spezifischen Datentyp umsetzen
  // HINWEIS: Die meisten Methoden besitzen eine entsprechende Funktion Get<methodName>Result()
  listResult := assistantv1.GetListWorkspacesResult(list)

  // Erfolgreiche Umsetzung prüfen
  if listResult != nil {
    prettyPrint(listResult, "List Workspaces")
  }
  ```
  {: codeblock}

  Arbeitsbereich abrufen:
  ```golang
  // Methode 'GetWorkspace' des Assistenten abrufen
  getWorkspaceOptions := assistantv1.NewGetWorkspaceOptions(listResult.Workspaces[0].WorkspaceID)
  get, getErr := assistant.GetWorkspace(getWorkspaceOptions)

  // Erfolgreichen Aufruf prüfen
  if getErr != nil {
    fmt.Println(getErr)
    return
  }

  // Ergebnis umsetzen
  getResult := assistantv1.GetGetWorkspaceResult(get)

  // Erfolgreiche Umsetzung prüfen
  if getResult != nil {
    prettyPrint(getResult, "Get Workspace")
    }
  }
  ```
  {: codeblock}

## Chatbot unter Verwendung von Starter-Kits hinzufügen
{: #conversation_starterkits}

Mithilfe von Starter-Kits können Sie das Leistungsspektrum von {{site.data.keyword.cloud_notm}} rasch und ohne großen Aufwand nutzen. Mit einem Starter-Kit können Sie {{site.data.keyword.conversationshort}} zu jedem beliebigen serverseitigen Back-End-Programm hinzufügen. Das Starter-Kit 'Chatbot for iOS with Watson' veranschaulicht, wie Sie die Deep-Learning-Funktionalität von {{site.data.keyword.conversationshort}} nutzen können, indem Sie zu Ihrer Anwendung eine Schnittstelle für natürliche Sprache hinzufügen, die die Interaktionen mit den Benutzern automatisiert.

1. Wählen Sie das [Starter-Kit](https://console.bluemix.net/developer/appledevelopment/starter-kits){:new_window} aus, mit dem Sie arbeiten möchten.
2. Erstellen Sie das Projekt mit den Standardservices.
3. Klicken Sie auf **Ressourcen hinzufügen > Watson > {{site.data.keyword.conversationshort}}**.
4. Laden Sie das Projekt herunter, indem Sie auf **Code herunterladen** klicken. Die entsprechenden Serviceberechtigungsnachweise befinden sich in der Datei `config/local-dev.json`.

## Nächste Schritte
{: #assistant_next}

Hervorragende Arbeit! Sie haben einen AI-Assistenten zu Ihrer App hinzugefügt. Erhalten Sie die Dynamik aufrecht, indem Sie eine der folgenden Optionen ausprobieren:
* Informieren Sie sich über das [Go-SDK für {{site.data.keyword.watson}}](https://github.com/watson-developer-cloud/go-sdk){:new_window}.
* Nutzen Sie alle Features, die [{{site.data.keyword.conversationshort}}](/docs/services/conversation/index.html) bietet.
