---

copyright:
  years: 2018
lastupdated: "2018-10-17"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using a health check in your Go app
{: #healthcheck}

Health checks provide a simple mechanism to determine whether a server-side application is behaving properly. Cloud environments like [Kubernetes](https://www.ibm.com/cloud/container-service) and [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry), can be configured to poll health endpoints periodically to determine whether an instance of your service is ready to accept traffic.

## Health check overview
{: #overview}

Health checks provide a simple mechanism to determine whether a server-side application is behaving properly. They're typically accessed over HTTP and use standard return codes to indicate UP or DOWN status. The return value of a health check is variable, but a minimal JSON response, like `{"status": "UP"}`, is typical.

Kubernetes has a nuanced notion of process health. It defines two probes:

- A _**readiness**_ probe is used to indicate whether the process can handle requests (is routable).

  Kubernetes doesn't route work to a container with a failing readiness probe. A readiness probe can fail if a service isn't finished initializing, or is otherwise busy, overloaded, or unable to process requests.

- A _**liveness**_ probe is used to indicate whether the process is to be restarted.

  Kubernetes stops and restarts a container with a failing liveness probe. Use liveness probes to clean up processes in an unrecoverable state, for example, if memory is exhausted, or if the container didn't stop properly after an internal process failed.

As a note for comparison, Cloud Foundry uses one health endpoint. If this check fails, the process is restarted, but if it succeeds, requests are routed to it. In this environment, the endpoint minimally succeeds when the process is live. An initial delay is configured to defer health checking until the service is finished initializing to avoid restart cycles.

The following table provides guidance on the responses that readiness, liveness, and singular health endpoints are to provide.

| State    | Readiness                   | Liveness                   | Health                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | Non-OK causes no load       | Non-OK causes restart      | Non-OK causes restart     |
| Starting | 503 - Unavailable           | 200 - OK                   | Use delay to avoid test   |
| Up       | 200 - OK                    | 200 - OK                   | 200 - OK                  |
| Stopping | 503 - Unavailable           | 200 - OK                   | 503 - Unavailable         |
| Down     | 503 - Unavailable           | 503 - Unavailable          | 503 - Unavailable         |
| Errored  | 500 - Server Error          | 500 - Server Error         | 500 - Server Error        |

## Adding a health check to an existing Go app
{: #add-healthcheck-existing}

You can add a minimal health or liveness check to an existing `Gin-Gonic` application by introducing a new route as shown in the following example:
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

Check the status of the app with a browser by accessing the `/health` endpoint.

There are more extensive libraries available, like [`http-healthcheck`](https://github.com/robzienert/http-healthcheck), which allow definition of extensible health checks with support for caching checks that are run against backing services. In this case, you would want to separate the simple liveness test in the example from the more robust, detailed readiness check that is built from the health check package.

## Accessing the health check endpoint in Go Starter Kit apps
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

## Recommendations for readiness and liveness probes
{: #recommendations}

Readiness probes should include the viability of connections to downstream services in their result when there isn’t an acceptable fallback if the downstream service is unavailable. This doesn't mean calling the health check that is provided by the downstream service directly, as infrastructure checks that for you. Instead, consider verifying the health of the existing references your application has to downstream services: this might be a JMS connection to WebSphere MQ, or an initialized Kafka consumer or producer. If you do check the viability of internal references to downstream services, cache the result to minimize the impact health checking has on your application.

A liveness probe, by contrast, is deliberate about what is checked, as a failure results in immediate termination of the process. A simple HTTP endpoint that always returns `{"status": "UP"}` with status code `200` is a reasonable choice.

## Configuring readiness and liveness probes in Kubernetes
{: #config_probes}

Declare liveness and readiness probes alongside your Kubernetes deployment. Both probes use the same configuration parameters:

* The kubelet waits for `initialDelaySeconds` before the first probe.

* The kubelet probes the service every `periodSeconds` seconds. The default is 1.

* The probe times out after `timeoutSeconds` seconds. The default and minimum value is 1.

* The probe is successful if it succeeds `successThreshold` times after a failure. The default and minimum value is 1. The value must be 1 for liveness probes.

* When a pod starts and the probe fails, Kubernetes tries `failureThreshold` times to restart the pod and then gives up. The minimum value is 1 and the default value is 3.
    - For a liveness probe, "giving up" means restarting the pod.
    - For a readiness probe, "giving up" means marking the pod `Unready`.

To avoid restart cycles, set `livenessProbe.initialDelaySeconds` to be safely longer than it takes your service to initialize. You can then use a shorter value for `readinessProbe.initialDelaySeconds` to route requests to the service as soon as it's ready.

An example configuration might look something like this:
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

For more information, see how to [Configure liveness and readiness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).
