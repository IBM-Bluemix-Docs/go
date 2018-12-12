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

# Go アプリでのヘルス・チェックの使用
{: #healthcheck}

ヘルス・チェックには、サーバー・サイド・アプリケーションが正常に動作しているかどうかを判別するための単純なメカニズムが備わっています。 [Kubernetes](https://www.ibm.com/cloud/container-service) や [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) のようなクラウド環境を構成することによって、ヘルス・エンドポイントを定期的にポーリングして、サービスのインスタンスがトラフィックを受け入れる準備ができているかどうかを判別できます。

## ヘルス・チェックの概要
{: #overview}

ヘルス・チェックには、サーバー・サイド・アプリケーションが正常に動作しているかどうかを判別するための単純なメカニズムが備わっています。 通常は HTTP を介してアクセスされ、標準的な戻りコードで UP または DOWN 状況が示されます。ヘルス・チェックの戻り値は変数ですが、`{"status": "UP"}` のような最小の JSON 応答が標準です。

Kubernetes には、プロセスの正常性を詳細に示す概念があります。次の 2 つのプローブが定義されています。

- _**readiness**_ プローブは、プロセスが要求を扱えるかどうか (ルーティング可能かどうか) を示すために使用されます。

  Kubernetes は、readiness プローブが失敗したコンテナーには作業をルーティングしません。サービスが初期化を完了していない場合、あるいはそれ以外でビジー状態である場合、過負荷である場合、または要求を処理できない場合、readiness プローブは失敗する可能性があります。

- _**liveness**_ プローブは、プロセスを再始動する必要があるかどうかを示すために使用されます。

  Kubernetes は、liveness プローブが失敗したコンテナーを停止して再始動します。例えば、メモリーが使い尽くされた場合や、内部プロセスが失敗した後にコンテナーが適切に停止しなかった場合に、liveness プローブを使用して、リカバリー不能状態のプロセスをクリーンアップします。

なお、比較として、Cloud Foundry では 1 つの health エンドポイントを使用します。
この検査が失敗するとプロセスは再始動されますが、成功した場合、要求はプロセスにルーティングされます。この環境では、プロセスがライブ状態であればエンドポイントは最低限成功します。再始動サイクルを避けるために、サービスの初期化が完了するまでヘルス・チェックが延期されるように初期遅延が構成されます。

次の表は、readiness エンドポイント、liveness エンドポイント、および 1 つだけのヘルス・エンドポイントが提供する応答に関するガイダンスです。

| 状態     | readiness                   | liveness                   | health                       |
|----------|-----------------------------|----------------------------|------------------------------|
|          | OK 以外はロードなし         | OK 以外は再始動            | OK 以外は再始動              |
| Starting | 503 - 使用不可              | 200 - OK                   | テスト回避のために遅延を使用 |
| Up       | 200 - OK                    | 200 - OK                   | 200 - OK                     |
| Stopping | 503 - 使用不可              | 200 - OK                   | 503 - 使用不可               |
| Down     | 503 - 使用不可              | 503 - 使用不可             | 503 - 使用不可               |
| Errored  | 500 - サーバー・エラー      | 500 - サーバー・エラー     | 500 - サーバー・エラー       |

## 既存の Go アプリへのヘルス・チェックの追加
{: #add-healthcheck-existing}

次の例に示すように、新しいルートを導入することによって、既存の `Gin-Gonic` アプリケーションに最低限のヘルス・チェックまたは活性チェックを追加できます。
```go
func HealthGET(c *gin.Context) {
  c.JSON(200, gin.H{
    "status": "UP",
  })
```
{: codeblock}

ブラウザーで `/health` エンドポイントにアクセスして、アプリの状況を検査します。

[`http-healthcheck`](https://github.com/robzienert/http-healthcheck) など、より広範囲のライブラリーが用意されています。これらのライブラリーでは、バッキング・サービスに対して実行される検査のキャッシングのサポートを含む拡張可能なヘルス・チェックを定義できます。この場合は、この例の単純な liveness テストを、ヘルス・チェック・パッケージから作成される、より堅固で詳細な readiness チェックから分離することが必要になります。

## Go スターター・キット・アプリでのヘルス・チェック・エンドポイントへのアクセス
{: #healthcheck-starterkit}

デフォルトでは、スターター・キットを使用して Go アプリを生成すると、アプリの状況 (UP/DOWN) を検査するための基本的な (無許可) ヘルス・チェック・エンドポイントが `/health` で使用可能になります。

ヘルス・チェック・エンドポイントのコードは、以下の `/routers/health.go` ファイルによって提供されます。
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

## readiness プローブと liveness プローブに関する推奨事項
{: #recommendations}

ダウンストリーム・サービスが使用不可の場合、使用できるフォールバックがなければ、ダウンストリーム・サービスへの接続の実行可能性を readiness プローブの結果に含める必要があります。これは、ダウンストリーム・サービスが備えているヘルス・チェックを直接呼び出すということではありません。それはインフラストラクチャーが検査することになります。代わりに、アプリケーションにおけるダウンストリーム・サービスへの既存の参照の正常性を検査することを検討してください。これに該当するものとして、WebSphere MQ への JMS 接続、あるいは初期化された Kafka コンシューマーまたはプロデューサーが考えられます。ダウンストリーム・サービスへの内部参照の実行可能性を実際に検査する場合は、ヘルス・チェックがアプリケーションに与える影響を最小限に抑えるために、結果をキャッシュしてください。

一方、liveness プローブは、検査対象について注意が必要です。失敗は結果としてプロセスの即時終了につながるからです。状況コードが `200` の `{"status": "UP"}` を常に返す単純な HTTP エンドポイントが適切な選択です。

## Kubernetes の readiness プローブと liveness プローブの構成
{: #config_probes}

Kubernetes デプロイメントと併せて liveness プローブと readiness プローブを宣言します。両方のプローブが同じ構成パラメーターを使用します。

* kubelet は最初のプローブの前に `initialDelaySeconds` 秒間待機します。

* `periodSeconds` 秒おきに kubelet がサービスをプローブします。デフォルトは、1 です。

* `timeoutSeconds` 秒経過するとプローブがタイムアウトになります。デフォルト値および最小値は 1 です。

* 失敗後 `successThreshold` 回成功するとプローブは成功です。デフォルト値および最小値は 1 です。liveness プローブの場合、値は 1 でなければなりません。

* ポッドが開始され、プローブが失敗した場合、Kubernetes はポッドの再始動を `failureThreshold` 回試行した後、試行を中止します。最小値は 1、デフォルト値は 3 です。
    - liveness プローブの場合、「中止する」はポッドを再始動することを意味します。
    - readiness プローブの場合、「中止する」はポッドに `Unready` のマークを付けることを意味します。

再始動サイクルを避けるために、安全を見てサービスの初期化時間よりも十分に長い時間を `livenessProbe.initialDelaySeconds` の値として設定してください。`readinessProbe.initialDelaySeconds` にそれよりも短い値を使用することで、サービスの準備ができ次第、要求をサービスにルーティングできるようにします。

構成例は以下のようになります。
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

詳しくは、[Configure liveness and readiness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) でその方法を参照してください。
