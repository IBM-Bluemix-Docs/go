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

# Go 環境の構成
{: #configuration}

一貫性のある移植性を維持できるようになる Go アプリケーションを開発する際に従うべき、標準化されたガイドラインがあります。考慮事項としては、資格情報の管理、データの保存、データの保管、およびクラウドへのコンテンツの公開があります。クラウド・ネイティブのプラクティスに従うことによって、Go アプリケーションを環境間で簡単に移行できます。例えば、コードを変更したり、テストされていないコード・パスを使用したりすることなく、テスト環境から実稼働環境に移行できます。

既存の Go アプリケーションにクラウド・サポートを追加する必要があるか、スターター・キットを使用して Go アプリを作成するかにかかわらず、目標はどのような開発プラットフォームに対しても移植性を確保することです。

## 既存の Go アプリケーションへのクラウド・サポートの追加
{: #add-ibm-cloud-env-golang}

[`ibm-cloud-env-golang`](https://github.com/ibm-developer/ibm-cloud-env-golang) モジュールはさまざまなクラウド・プロバイダー (CloudFoundry や Kubernetes など) から環境変数を集約するため、アプリケーションは環境から独立しています。

### `ibm-cloud-env-golang` モジュールのインストール
1. 次のコマンドを使用して `ibm-cloud-env-golang` モジュールをインストールします。
  ```bash
  go get github.com/ibm-developer/ibm-cloud-env-golang
  ```
  {: codeblock}

2. `mappings.json` ファイルを参照して、コード内でこのモジュールを初期化します。
  ```golang
  import "github.com/ibm-developer/ibm-cloud-env-golang"
  IBMCloudEnv.Initialize("/path/to/the/mappings/file/relative/to/project/root")
  ```
  {: codeblock}

  Go はデフォルト・パラメーターをサポートしないため、デフォルトの `mappings.json` ファイルは提供されません。マッピング・ファイル・パスが `IBMCloudEnv.Initialize ()` で指定されていない場合は、エラーがログに記録されます。
  {: tip}

  `mappings.json` ファイルの例:
  ```javascript
  {
      "service1-credentials": {
          "searchPatterns": [
              "user-provided:my-service1-instance-name:service1-credentials",
              "cloudfoundry:my-service1-instance-name", 
              "env:my-service1-credentials", 
              "file:/localdev/my-service1-credentials.json" 
          ]
      },
    "service2-username": {
         "searchPatterns":[
              "user-provided:my-service2-instance-name:username",
              "cloudfoundry:$.service2[@.name=='my-service2-instance-name'].credentials.username",
              "env:my-service2-credentials:$.username",
              "file:/localdev/my-service1-credentials.json:$.username"
         ]
      }
  }
  ```
  {: codeblock}

### Go アプリでの値の使用
以下のコマンドを使用して、アプリケーション内で値を取得します。

1. 変数 `service1credentials` を取得します。
  ```golang
  service1credentials := IBMCloudEnv.GetDictionary("service1-credentials"); 
  ```
  {: codeblock}

2. 変数 `service2username` を取得します。
  ```golang
  service2username := IBMCloudEnv.GetString("service2-username");
  ```
  {: codeblock}

これで、さまざまなクラウド計算プロバイダーから生じる相違点を抽象化することによって、任意のランタイム環境にアプリケーションを実装できるようになります。

### タグおよびラベルの値のフィルタリング
以下の例に示すように、モジュールによって生成された資格情報を、サービス・タグとサービス・ラベルに基づいてフィルターに掛けることができます。
```golang
filtered_credentials := IBMCloudEnv.GetCredentialsForService(tag, label, credentials)); // returns a Json with credentials for specified service tag and label
```
{: codeblock}

## スターター・キット・アプリからの Go 構成マネージャーの使用
[スターター・キット](https://console.bluemix.net/developer/appservice/starter-kits/)を使用して作成された Go アプリには、多くのクラウド・デプロイメント環境 (CF、K8s、および Functions) での実行に必要な資格情報と構成が自動的に付属します。

### サービス資格情報について

サービスに関するアプリケーション構成情報は `/server/config` ディレクトリーの `localdev-config.json` ファイルに保管されます。 このファイルは、Git に機密情報が保管されるのを防ぐために、`.gitignore` ディレクトリーにあります。 ローカルで実行される構成済みサービス用の接続情報 (ユーザー名、パスワード、およびホスト名など) がこのファイルに保管されます。

アプリケーションは、構成マネージャーを使用して、環境およびこのファイルから接続情報および構成情報を読み取ります。 また、`server/config` ディレクトリーにあるカスタムビルトの `mappings.json` を使用して、各サービスの資格情報がある場所を伝達します。

ローカルで実行されているアプリケーションは、`mappings.json` ファイルから読み取ったアンバインドされた資格情報を使用して、{{site.data.keyword.cloud_notm}} サービスに接続できます。ただし、アンバインドされた資格情報を作成する必要がある場合は、{{site.data.keyword.cloud_notm}} Web コンソール (例) から行うか、[CloudFoundry CLI](https://docs.cloudfoundry.org/cf-cli/) `cf create-service-key` コマンドを使用して行うことができます。

アプリケーションを {{site.data.keyword.cloud_notm}} にプッシュすると、これらの値は使用されなくなります。 代わりに、アプリケーションは環境変数を使用して、バインドされたサービスに自動的に接続します。 

* **Cloud Foundry**: サービス資格情報は、`VCAP_SERVICES` 環境変数から取得されます。

* **Kubernetes**: サービス資格情報は、サービスごとに個別の環境変数から取得されます。


## 次のステップ
{: #next_steps notoc}

`ibm-cloud-env-golang` では、値の検索に使用できる検索パターン・タイプとして、`user-provided`、`cloudfoundry`、`env`、および `file` の 4 つがサポートされています。サポートされる他の検索パターンおよび検索パターン例を確認したい場合は、[Usage](https://github.com/ibm-developer/ibm-cloud-env-golang#usage) セクションを参照してください。
