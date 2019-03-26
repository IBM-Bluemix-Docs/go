---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Statusprüfung in Go-App verwenden
{: #healthcheck}

Statusprüfungen stellen einen einfachen Mechanismus bereit, mit dem festgestellt werden kann, ob sich eine serverseitige Anwendung korrekt verhält. Cloudumgebungen wie [Kubernetes](https://www.ibm.com/cloud/container-service) und [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) können so konfiguriert werden, dass sie die Statusendpunkte in regelmäßigen Intervallen abfragen und so bestimmen, ob eine Instanz Ihres Service für die Annahme von Datenverkehr bereit ist.

## Statusprüfungen im Überblick
{: #overview}

Statusprüfungen stellen einen einfachen Mechanismus bereit, mit dem festgestellt werden kann, ob sich eine serverseitige Anwendung korrekt verhält. Sie werden normalerweise über HTTP aufgerufen und geben den Status UP (AKTIV) oder DOWN (INAKTIV) mit Standardrückgabecodes an. Der Rückgabewert einer Statusprüfung ist variabel, aber eine minimale JSON-Antwort wie etwa `{"status": "UP"}` ist typisch.

Kubernetes hat ein differenziertes Verständnis vom Prozesszustand. Daher werden zwei Prüfungen definiert:

- Anhand einer Prüfung der _**Bereitschaft**_ (Readiness) wird festgestellt, ob der Prozess Anforderungen verarbeiten kann, d. h. ob er routingfähig ist.

  Kubernetes leitet Arbeit nicht an Container weiter, deren Bereitschaftsprüfung fehlgeschlagen ist. Eine Bereitschaftsprüfung kann fehlschlagen, wenn ein Service noch nicht vollständig initialisiert worden oder anderweitig ausgelastet oder überlastet ist oder aber Anforderungen nicht verarbeiten kann.

- Anhand einer Prüfung der _**Aktivität**_ (Liveness) wird festgestellt, ob der Prozess erneut gestartet werden muss.

  Kubernetes stoppt Container mit einer fehlgeschlagenen Prüfung der Aktivität (Liveness) und startet diese anschließend erneut. Die Verwendung von Aktivitätsprüfungen bietet sich an, um Prozesse in einem Zustand der Nichtwiederherstellbarkeit zu bereinigen, wenn z. B. der Speicher erschöpft ist oder der Container nach dem Fehlschlagen eines internen Prozesses nicht ordnungsgemäß gestoppt wurde.

Im Vergleich hierzu verwendet Cloud Foundry einen Statusendpunkt. Wenn diese Prüfung fehlschlägt, wird der Prozess erneut gestartet. Wird die Prüfung hingegen erfolgreich abgeschlossen, werden Anforderungen an ihn weitergeleitet. In dieser Umgebung ist der Endpunkt nur minimal erfolgreich, wenn der Prozess aktiv ('live') ist. Zur Vermeidung von Neustartzyklen wird die Überprüfung des Zustands durch eine entsprechend konfigurierte Anfangsverzögerung so lange hinausgezögert, bis der Service vollständig initialisiert worden ist.

Die folgende Tabelle stellt eine Orientierungshilfe für die Antworten dar, die Bereitschaft (Readiness), Aktivität (Liveness) und einzelne Zustandsendpunkte liefern müssten.

| Zustand    | Bereitschaft/Readiness                   | Aktivität/Liveness                   | Status                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Wenn nicht 'OK', kein Laden       | Wenn nicht 'OK', Neustart      | Wenn nicht 'OK', Neustart     |
| Wird gestartet | 503 - Nicht verfügbar           | 200 - OK                   | Test durch Verzögerung vermeiden   |
| Aktiv       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Wird gestoppt | 503 - Nicht verfügbar           | 200 - OK                   | 503 - Nicht verfügbar         |
| Inaktiv     | 503 - Nicht verfügbar           | 503 - Nicht verfügbar          | 503 - Nicht verfügbar         |
| Mit Fehler(n)  | 500 - Serverfehler          | 500 - Serverfehler         | 500 - Serverfehler        |

## Statusprüfung zu einer vorhandenen Go-App hinzufügen
{: #add-healthcheck-existing}

Zu einer vorhandenen `Gin-Gonic`-Anwendung können Sie eine minimale Prüfung des Status oder der Aktivität (Liveness) hinzufügen, indem Sie eine neue Route einführen, wie im folgenden Beispiel dargestellt:
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

Sie können den Status der App mit einem Browser überprüfen, indem Sie auf den Endpunkt `/health` zugreifen.

Es gibt umfangreichere Bibliotheken wie [`http-healthcheck`](https://github.com/robzienert/http-healthcheck), die das Definieren von erweiterbaren Statusprüfungen mit Unterstützung für Cacheprüfungen ermöglichen, die für Unterstützungsservices ausgeführt werden. In diesem Fall wäre es erstrebenswert, den einfachen Aktivitätstest im Beispiel von der robusteren, ausführlicheren Bereitschaftsprüfung zu trennen, die mit dem Paket für Statusprüfungen erstellt wird.

## Auf den Statusprüfungsendpunkt in Go-Starter-Kit-Apps zugreifen
{: #healthcheck-starterkit}

Wenn Sie eine Go-App mithilfe eines Starter-Kits generieren, ist standardmäßig unter `/health` ein grundlegender (berechtigungsloser) Endpunkt für Statusprüfungen verfügbar, über den der Status (UP/DOWN) der App geprüft werden kann.

Der Code für den Endpunkt für Statusprüfungen wird durch die folgende Datei `/routers/health.go` bereitgestellt:
```go
package routers

import (
  "github.com/gin-gonic/gin"
)

func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
}
```
{: codeblock}

## Empfehlungen für Prüfungen der Bereitschaft (Readiness) und Aktivität (Liveness)
{: #recommend-healthcheck}

Prüfungen der Bereitschaft (Readiness) sollten die Funktionsfähigkeit von Verbindungen zu nachgeschalteten Services in ihrem Ergebnis berücksichtigen, wenn es keine vertretbare Fallbacklösung für den Fall gibt, dass der nachgeschaltete Service nicht verfügbar ist. Dies bedeutet nicht, dass die durch den nachgeschalteten Service bereitgestellte Statusprüfung direkt aufgerufen wird, da die Infrastruktur dies für Sie prüft. Erwägen Sie stattdessen eine Überprüfung des Status der vorhandenen Referenzen, die Ihre Anwendung zu nachgeschalteten Services hat. Dabei könnte es sich um eine JMS-Verbindung zu WebSphere MQ oder um einen initialisierten Kafka-Konsumenten oder -Produzenten handeln. Wenn Sie die Funktionsfähigkeit interner Referenzen zu nachgeschalteten Services überprüfen, sollten Sie das Ergebnis zwischenspeichern, um die Auswirkungen der Statusüberprüfung auf Ihre Anwendung zu minimieren.

Im Gegensatz dazu wird bei einer Prüfung der Aktivität (Liveness) ganz bewusst entschieden, was geprüft wird, da ein Fehlschlagen der Prüfung die sofortige Beendigung des Prozesses zur Folge hat. Ein einfacher HTTP-Endpunkt, der stets `{"status": "UP"}` mit dem Statuscode `200` zurückgibt, ist eine angemessene Wahl.

## Prüfungen der Bereitschaft (Readiness) und Aktivität (Liveness) in Kubernetes konfigurieren
{: #config_probes-healthcheck}

Deklarieren Sie gemeinsam mit Ihrer Kubernetes-Bereitstellung Prüfungen der Bereitschaft (Readiness) und der Aktivität (Liveness). Beide Prüfungen verwenden dieselben Konfigurationsparameter:

* Das Kubelet wartet bis zur Durchführung der ersten Prüfung einen Zeitraum von `initialDelaySeconds` ab.

* Das Kubelet prüft den Service in Zeitabständen von `periodSeconds` Sekunden. Der Standardwert beträgt 1.

* Die Prüfung überschreitet das Zeitlimit nach `timeoutSeconds` Sekunden. Der Standard- und Mindestwert beträgt 1.

* Die Prüfung ist erfolgreich, wenn sie nach einem Fehler `successThreshold` Mal erfolgreich war. Der Standard- und Mindestwert beträgt 1. Der Wert für Prüfungen der Aktivität muss 1 lauten.

* Wenn ein Pod gestartet wird und die Prüfung fehlschlägt, versucht Kubernetes `failureThreshold` Mal, den Pod erneut zu starten, und gibt dann auf. Der Mindestwert ist 1, der Standardwert beträgt 3.
    - Bei einer Prüfung der Aktivität (Liveness) bedeutet 'aufgeben', dass der Pod erneut gestartet wird.
    - Bei einer Prüfung der Bereitschaft (Readiness) bedeutet 'aufgeben', dass der Pod mit `Unready` als nicht bereit markiert wird.

Zur Vermeidung von zyklischen Neustarts sollten Sie für `livenessProbe.initialDelaySeconds` einen Wert festlegen, der sicherheitshalber einen längeren Zeitraum an den angibt, der eigentlich für die Initialisierung Ihres Service notwendig wäre. Für `readinessProbe.initialDelaySeconds` können Sie dann einen niedrigeren Wert verwenden, um Anforderungen an den Service weiterzuleiten, sobald dieser bereit ist.

Eine Beispielkonfiguration könnte wie folgt aussehen:
```yaml
spec:
  containers:
  - name: ...
    image: ...
    readinessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 120
      timeoutSeconds: 5
    livenessProbe:
      httpGet:
        path: /liveness
        port: 8080
      initialDelaySeconds: 130
      timeoutSeconds: 10
      failureThreshold: 10
```
{: codeblock}

Weitere Informationen finden Sie im Abschnitt zum Konfigurieren von Prüfungen auf Bereitschaft (Readiness) und Aktivität (Liveness) in [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).
