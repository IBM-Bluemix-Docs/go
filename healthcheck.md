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

# Using a health check in your Go app
{: #healthcheck}

Health checks provide a simple mechanism to determine whether a server-side application is behaving properly. Many deployment environments, such as [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) and [Kubernetes](https://www.ibm.com/cloud/container-service), can be configured to poll health endpoints periodically to determine whether an instance of your service is ready to accept traffic.

## Health check overview
{: #overview}

Health checks are typically accessed over `HTTP`, and use standard return codes for indicating `UP` or `DOWN` status. Examples include returning `200` for `UP`, and `5xx` for `DOWN`. For example, a `503` return code is used when the application can’t handle requests, and a `500` is used when the server experiences an error condition. The return value of a health check is variable, but a minimal JSON response, like `{“status”: “UP”}` provides consistency.

Kubernetes defines both [liveness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) and [readiness](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) probes:

* The liveness probe allows an application to indicate whether the process can be restarted. A subset of considerations applies: a `liveness` check might fail if a memory threshold is reached, for example. If your app is running on Kubernetes, consider adding a `liveness` endpoint to ensure that your process is restarted when necessary.

* The readiness probe is used for automatic routing decisions. The success or failure indicates whether the application can receive new work. This endpoint can include considerations for required downstream services. Consider caching the result of downstream service checks to minimize overall load on the system (for example, test database connectivity at most once per second).

Cloud Foundry uses only one health endpoint. If this check fails, Cloud Foundry restarts the process. But if it succeeds, Cloud Foundry assumes that the process can handle new work. This health check makes the single endpoint something of a combination between liveness and readiness.

## Adding a health check to an existing Go app
{: #add-healthcheck-existing}

Add Health check to an existing app by introducing a new route as shown in the following example:
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

Check the status of the app with a browser by accessing the `/health` endpoint.

Health check is extensible, as you can add time stamps or other meaningful criteria.

## Accessing Health check from Go Starter Kit apps
{: #healthcheck-starterkit}

By default, when you generate a Go app by using a Starter Kit,
a basic (unauthorized) health check endpoint is available at `/health` to check the status of the app (UP/DOWN).

The health check endpoint code is provided by the following `/routers/health.go` file:
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


