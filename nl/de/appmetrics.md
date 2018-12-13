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

# Anwendungsmetriken mit Go-Apps verwenden
{: #metrics}

Anwendungsmetriken sind wichtig für die Überwachung der Leistung Ihrer Anwendung. Eine Liveansicht von Metriken wie CPU, Speicher, Latenzzeit und HTTP-Metriken ist unverzichtbar, um sicherzustellen, dass Ihre Anwendung im Zeitablauf effektiv ausgeführt wird. Zur Anpassung an die aktuelle Workload durch dynamische Skalierung von Instanzen können Sie einen Cloud-Service wie den [Auto-Scaling-Service](/docs/services/Auto-Scaling/index.html) von Cloud Foundry verwenden, der sich auf Metriken stützt. Durch die Verwendung von Anwendungsmetriken werden Sie genau informiert, wann Sie Instanzen durch Scale-up oder Scale-down vertikal nach oben bzw. nach unten skalieren oder nicht mehr benötigte Instanzen bereinigen müssen, um die Kosten niedrig zu halten.

Anwendungsmetriken werden als Zeitreihendaten erfasst. Das Aggregieren und Visualisieren erfasster Metriken kann dabei helfen, allgemeine Leistungsprobleme zu erkennen, wie zum Beispiel die Folgenden: 

* Schleppende HTTP-Antwortzeiten auf manchen oder allen Routen
* Geringer Durchsatz in der Anwendung
* Nachfragespitzen, durch die eine Verlangsamung verursacht wird
* Höhere CPU-Auslastung als für den Durchsatz bzw. die Arbeitslast erwartet
* Hohe oder zunehmende Speicherbelegung (potenzielles Speicherleck)

## Anwendungsmetriken zu Ihrer vorhandenen Go-Anwendung hinzufügen
{: #add-appmetrics-existing}

Zum Hinzufügen der Leistungsüberwachung zu Ihrer Go-Anwendung können Sie die umfassende Aggregation von Metriken nutzen, die das Paket `promhttp` bereitstellt.

Das Paket `promhttp` enthält zahlreiche Erweiterungspunkte, unter anderem auch [Prometheus-Konfiguration](https://github.com/prometheus/client_golang).

1. Sie können zum Beispiel die folgende einfache Go- und Gin-Anwendung vom Typ 'Hello World' verwenden:
  ```go
  // Obiges importieren
  func main() {
      router := gin.Default()
      router.GET("/", func(c *gin.Context) {
          c.String(http.StatusOK, "Hello, World!")
      }
      router.Run(":3000")
  }
  ```
  {: codeblock}

2. Rufen Sie das Paket mit dem folgenden Befehl ab:
  ```
  go get github.com/prometheus/client_golang/prometheus/promhttp
  ```
  {: codeblock}

3. Fügen Sie `promhttp` zu den Importen hinzu:
  ```go
  import "github.com/prometheus/client_golang/prometheus/promhttp"
  ```
  {: codeblock}

  Es ist notwendig, eine zusätzliche Route für Metriken zum Router hinzuzufügen.

  Der überarbeitete Code sieht nun wie im folgenden Beispiel aus:
  ```go
  // Obiges importieren
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

## Anwendungsmetriken in Starter-Kits verwenden

Die serverseitigen Go-Anwendungen, die aus Starter-Kits erstellt werden, sind automatisch mit dem [Prometheus-Endpunkt](https://prometheus.io/) unter `http://<hostname>:<port>/metrics` ausgestattet. Der Code für diesen Endpunkt befindet sich in `server.go`.

Weitere Informationen enthält das [GitHub-Repository für Prometheus](https://github.com/prometheus/client_golang/).
