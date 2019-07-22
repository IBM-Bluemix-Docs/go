---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: healthcheck go, add healthcheck, healthcheck endpoint, readiness go, liveness go, endpoint go, probes go

subcollection: go

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 在 Go 应用程序中使用运行状况检查
{: #go-healthcheck}

运行状况检查提供了一种简单的机制，用于确定服务器端应用程序是否在正常运行。您可以将云环境（如 [Kubernetes](https://www.ibm.com/cloud/container-service){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标") 和 [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")）配置为定期轮询运行状况端点，以确定您的服务实例是否已准备好接受流量。


## 运行状况检查概述
{: #go-healtcheck-overview}

运行状况检查提供了一种简单的机制，用于确定服务器端应用程序是否在正常运行。运行状况检查通常通过 HTTP 进行访问，并使用标准返回码来指示 UP 或 DOWN 状态。运行状况检查的返回值是可变的，但极简 JSON 响应（如 `{"status": "UP"}`）则采用典型值。

Kubernetes 对于进程运行状况的概念有细微差别。它定义了两个探测器：

- _**就绪性**_探测器用于指示该进程是否可以处理请求（可路由）。

  Kubernetes 不会将工作路由到未通过就绪性探测器测试的容器。如果服务未完成初始化，或者服务因其他原因而繁忙、超负荷或无法处理请求，那么不会通过就绪性探测器测试。

- _**活性**_探测器用于指示该进程是否要重新启动。

  Kubernetes 会停止并重新启动活跃度探测器失败的容器。例如，如果内存耗尽，或内部进程发生故障后容器未适当地停止，那么使用活性探测器可清除处于不可恢复状态的进程。

作为比较，请注意，Cloud Foundry 使用一个运行状况端点。如果此次检查失败，将重新启动该进程，但是如果成功，会将请求路由到该进程。在此环境中，端点会在该进程处于活动状态时实现最低限度的成功。配置了初始延迟，以将运行状况检查延迟到服务完成初始化之后，从而避免重新启动循环。

下表提供了就绪性、活性以及单个运行状况端点可提供的响应的指南。

| 状态     |就绪性                   | 活性                   | 运行状况                    |
|----------|-----------------------------|----------------------------|---------------------------|
|          | 非正常，不装入              | 非正常，重新启动            | 非正常，重新启动            |
| 正在启动 | 503 - 不可用                | 200 - 正常                  | 使用延迟来避免测试          |
| 启动     | 200 - 正常                  | 200 - 正常                  | 200 - 正常                  |
| 正在停止 | 503 - 不可用                | 200 - 正常                  | 503 - 不可用                |
| 关闭     | 503 - 不可用                | 503 - 不可用                | 503 - 不可用                |
| 出错     | 500 - 服务器错误            | 500 - 服务器错误            | 500 - 服务器错误            |
{: caption="表 1. HTTP 状态码" caption-side="bottom"}

## 向现有 Go 应用程序添加运行状况检查
{: #go-add-healthcheck-existing}

您可以通过引入新路径向现有 `Gin-Gonic` 应用程序添加极简的运行状况或活性检查，如以下示例中所示：
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

通过访问 `/health` 端点，使用浏览器检查应用程序的状态。

提供有更广泛的库，如 [`http-healthcheck`](https://github.com/robzienert/http-healthcheck){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")，这允许定义可扩展运行状况检查，其中支持高速缓存针对后备服务运行的检查。在此情况下，您需要将示例中的简单活性测试与从运行状况检查包构建的更稳健的详细就绪性检查分隔开。

## 在 Go 入门模板工具包应用程序中访问运行状况检查端点
{: #go-healthcheck-starterkit}

缺省情况下，使用入门模板工具包来生成 Go 应用程序时，`/health` 处提供了基本（未授权）运行状况检查端点，可用于检查应用程序的状态 (UP/DOWN)。

运行状况检查端点代码通过以下 `/routers/health.go` 文件提供：
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

## 针对就绪性和活性探测器的建议
{: #go-recommend-healthcheck}

如果下游服务不可用，那么在没有可接受的回退时，就绪性探测器必须在其结果中包含连接到下游服务的可行性。不必调用下游服务直接提供的运行状况检查，因为基础架构会为您执行该项检查。请考虑验证应用程序对下游服务的现有引用的运行状况。例如，这些引用可能是与 WebSphere MQ 的 JMS 连接，也可能是已初始化的 Kafka 使用者或生产者。如果您确实检查了下游服务的内部引用的可行性，请将结果高速缓存以尽可能减小运行状况检查对应用程序的影响。

相比之下，活性探测器会注重检查的内容，因为故障会导致该进程立即终止。一个始终使用状态码 `200` 返回 `{"status": "UP"}` 的简单 HTTP 端点是比较合理的选项。

## 在 Kubernetes 中配置就绪性和活性探测器
{: #go-config-probes-healthcheck}

随 Kubernetes 部署一起声明活性和就绪性探测器。这两个探测器使用相同的配置参数：

* 在进行首次探测之前，kubelet 会等待 `initialDelaySeconds`。

* kubelet 每 `periodSeconds` 秒探测一次服务。缺省值为 1。

* 探测器在 `timeoutSeconds` 秒后超时。缺省值和最小值为 1。

* 如果探测器在一次失败后成功 `successThreshold` 次，那么该探测器是成功的。缺省值和最小值为 1。活性探测器的值必须为 1。

* 在 pod 启动后，如果探测器失败，Kubernetes 会尝试 `failureThreshold` 次重新启动该 pod，然后放弃。最小值为 1 并且缺省值为 3。
    - 对于活性探测器，“放弃”意味着重新启动该 pod。
    - 对于就绪性探测器，“放弃”意味着将该 pod 标记为 `Unready`。

要避免重新启动循环，请将 `livenessProbe.initialDelaySeconds` 设置为比服务初始化更长且安全的时间。然后您可以对 `readinessProbe.initialDelaySeconds` 使用更短的值，以在服务就绪后尽快将请求路由到服务。

请参阅以下示例配置：
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

有关更多信息，请参阅如何[配置活性和就绪性探测器](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/){: new_window} ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")。
