---

copyright:
  years:  2018, 2019
lastupdated: "2019-03-06"

keywords: Sysdig, IBM Cloud, monitoring,  predefined dashboards

subcollection: Sysdig

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}


# 事前定義ダッシュボード
{: #default_dashboards}
以下の任意の事前定義ダッシュボードを選択して、インフラストラクチャーをモニターできます。
{:shortdesc}


## アプリケーション
{: #default_dashboards_applications}

以下の表は、アプリケーションおよびインフラストラクチャーのコンポーネントをモニターするために選択できる事前定義ダッシュボードを示しています。

| ダッシュボード名        | 説明                                                            | 使用法             |
|-----------------------|------------------------------------------------------------------------|-------------------|
| HTTP 概要 (HTTP Overview)         | Web サーバーの正常性をモニターするには、このダッシュボードを使用します。 このダッシュボードでは、サーバーに対する負荷およびタイムリーに要求に対応するサーバーの機能が示されます。 | 「要求の数 (Number of Requests)」パネルおよび「要求の最大時間と平均時間 (Avg/Max Request Time)」パネルを使用して、サーバーの全体的な負荷を測定します。 </br>URL 間の相関を見つけて、パフォーマンスを高める機会を探ります。 </br>状況コード・メトリックを使用して、4xx エラー・コードと 5xx エラー・コードをモニターします。 |  
| HTTP 上位要求 (HTTP Top Requests)  |  このダッシュボードを使用して、最も要求されている Web サーバーの URL について、要求の合計数、要求に対応する平均時間と最大時間、要求および応答に含まれるトラフィックの量などをモニターします。 |   |
| MongoDB  | このダッシュボードを使用して、MongoDB サービスの頻度、最も需要の多いコレクション、およびパフォーマンスが最も遅いコレクションをモニターします。  |  照会および索引のパフォーマンス調整の利点を活用できるコレクションを見つけます。 |
| MySQL/PostgreSQL  | このダッシュボードを使用して、SQL データベース・トランザクションの全体的な負荷とパフォーマンスの状況をモニターします。 要求の数と要求がどれだけ迅速に処理されたかを確認します。  | パフォーマンスを改善します。 例えば、最も遅い照会の応答時間をモニターし、表の照会または索引に加える変更を特定します。  |
| MySQL/PostgreSQL 上位 (MySQL/PostgreSQL Top)  | 上位の SQL 照会をモニターするには、このダッシュボードを使用します。 受信した照会の数およびその照会のために送受信されるトラフィックの量を確認します。   | 最も遅い照会、最も多く実行される照会、高トラフィックの照会を見つけます。  |
| Nginx  | ホスト・リソース、http 接続、最上位および最も遅い URL、およびホスト応答コードをモニターするには、このダッシュボードを使用します。 | 書き込み、読み取り、待機している接続、マシンによる CPU 負荷、ネットワーク・バイト・アクティビティー、要求数/秒のメトリックを使用してモニターし、ロード・バランシングの調整を特定します。 </br>最も遅い URL メトリックを確認して、最適化できるページを探します。 </br>応答コードを確認して、問題を見つけます。  |
{: caption="表 1. 事前定義ダッシュボードのアプリケーションおよびインフラストラクチャーのコンポーネントのリスト" caption-side="top"} 


## ホストおよびコンテナーのダッシュボード
{: #default_dashboards_host_container}

以下の表は、ホストおよびコンテナーでのリソース使用状況およびシステム・アクティビティーをモニターするために使用できる事前定義ダッシュボードを示しています。


| ダッシュボード名        | 説明                                                            | 使用法             |
|-----------------------|------------------------------------------------------------------------|-------------------|
| *コンテナー制限 (Container Limits)*    | 各コンテナーに割り当てられているホスト・リソースに関連する CPU およびメモリーのメトリックを表示するには、このダッシュボードを使用します。 | |
| *ファイル・システム (File System)*         | ディレクトリー・マウント・ポイント、ファイル・システム・デバイス、およびインスタンスにマウントされているファイル・システムの容量と使用量の情報を表示するには、このダッシュボードを使用します。  </br>デフォルトでは、リモートでマウントされているファイル・システムはリストされません。 このようなファイル・システムのデータを表示するには、各インスタンスの */opt/draios/bin/dragent.properties* ファイルにエントリー **remotefs.enabled = true** を追加します。 </br>グループが選択される場合、メトリックは、類似したファイル・システム・マウント・ポイントの平均です。| いっぱいになっているファイル・システムとあまり使用されていないファイル・システムを見つけます。 *fs.bytes.used* によってソートします。|
| *コンテナー別の概要 (Overview by Container)* | 選択されたグループまたはインスタンスで実行されているコンテナーの CPU 使用率、メモリー使用率、ネットワークの合計バイト数、ファイルの合計バイト数などのリソース使用統計を表示するには、このダッシュボードを使用します。  </br>このビューの*「比較 (Compare)」*機能とともに*「移動時間 (Time Travel)」*機能を使用して、履歴情報を表示します。 このデータに基づいて、プロセスが予測どおり実行されているか、間違った処理を行っているか、あるいはさらにリソースが必要かどうかを確認します。 | 多くのリソースを使用しているコンテナーを見つけます。  </br>この情報を使用して、アプリケーションを別のホストに移動する必要があるかどうかを判断します。 |
| *コンテナー・イメージ別の概要 (Overview by Container Image)* | コンテナー・イメージで定義されているサービスのサイズ、パフォーマンス、および制限の概要を取得するには、このダッシュボードを使用します。 | |
| ホスト別の概要 (Overview by Host) | ホストの CPU 使用率、メモリー使用率、ネットワークの合計バイト数、ネットワーク接続の数、ファイルの合計バイト数、ディスク使用量などの一般リソースの使用統計を表示するには、このダッシュボードを使用します。 </br>このダッシュボードを使用して、インスタンスのグループをモニターすることもできます。 </br>このビューの*「比較 (Compare)」*機能とともに*「移動時間 (Time Travel)」*機能を使用して、履歴情報を表示します。 このデータに基づいて、プロセスが予測どおり実行されているか、間違った処理を行っているか、あるいはさらにリソースが必要かどうかを確認します。 | 類似したジョブ機能のホストのグループ内で使用されていないホストや過度に使用されているホストを見つけます。 | 
| プロセス別の概要 (Overview by Process) | ホスト最上位プロセスの CPU 使用率、メモリー使用率、ネットワークの合計バイト数、ネットワーク接続の数、ファイルの合計バイト数、ディスク使用量などの一般リソースの使用統計を表示するには、このダッシュボードを使用します。 </br>このダッシュボードを使用して、インスタンスのグループをモニターすることもできます。 </br>このビューの*「比較 (Compare)」*機能とともに*「移動時間 (Time Travel)」*機能を使用して、履歴情報を表示します。 このデータに基づいて、プロセスが予測どおり実行されているか、間違った処理を行っているか、あるいはさらにリソースが必要かどうかを確認します。 | 多くのリソースを使用しているプロセスを見つけます。  </br>この情報を使用して、アプリケーションを別のホストに移動する必要があるかどうかを判断します。 |
| Sysdig エージェントの要約 (Sysdig Agent Summary) | ご使用の環境にデプロイされている Sysdig エージェントの数とそのバージョンを表示するには、このダッシュボードを使用します。 |  | 
| 上位ファイル (Top Files) | 上位ファイルのリストを表示するには、このダッシュボードを使用します。 ファイル名、ファイルの合計バイト数、ファイルの入出力にかかった時間、およびファイルのエラー件数を確認できます。 | サイズ別にソートして、上位のディスク・コンシューマーを見つけます。 </br>入出力別にソートして、最も使用されているファイルを見つけます。 </br>*「ファイル・エラー数 (File Error Count)」*列のデータをモニターして、可能性のあるエラーを特定します。 |
| 最上位プロセス (Top Processes) | ホストまたはホストのグループで実行されている最上位プロセスのリストを表示するには、このダッシュボードを使用します。 プロセス名、そのパス、およびすべての引数を確認できます。 </br>フィルター機能を使用して、リストされるプロセスを制限します。 | 同じプロセスが複数回作成される環境で最上位コンシューマーであるプロセスを見つけます。 |
| 最上位サーバー・プロセス (Top Server Processes) | サーバー指向 (httpd、java、ntpd など) であると識別されたプロセスのみのリソース消費を表示するには、このダッシュボードを使用します。. | サーバー・プロセスのリソース使用量を確認します。 |
{: caption="表 2. ホストおよびコンテナーの事前定義ダッシュボードのリスト" caption-side="top"} 

## ネットワーク
{: #default_dashboards_network}

以下の表は、ネットワークの接続およびアクティビティーをモニターするために選択できる事前定義ダッシュボードを示しています。

| ダッシュボード名        | 説明                                                            | 使用法             |
|-----------------------|------------------------------------------------------------------------|-------------------|
| 「接続」表     | インスタンスおよびリモート・ノード間の接続を表示するには、このダッシュボードを使用します。 各接続のトラフィックをモニターします。 | ネットワークで最も多くトラフィックを消費する最上位送話者を見つけます。 |
| 概説              |  方向、アプリケーション、プロセス、およびホスト別に時間の経過に伴うネットワークの合計使用率を表示するには、このダッシュボードを使用します。| 応答パフォーマンスに最も影響を与えるリソースを特定します。 |
| 応答時間とリソース使用量 (Response Times vs Resource Usage) | ネットワーク要求の処理でのホストの応答時間と比較した、時間の経過に伴うホスト・リソース・メトリックを表示するには、このダッシュボードを使用します。 |  |
| 上位のポート (Top Ports)             |  ポートごとの着信バイト数、発信バイト数、および合計バイト数と各ポート番号への接続数を表示するには、このダッシュボードを使用します。 |  |
{: caption="表 3. ネットワークの事前定義ダッシュボードのリスト" caption-side="top"} 



## サービス
{: #default_dashboards_service}


以下の表は、サービスが組織的なコンテナーにデプロイされている場合でもサービスのパフォーマンスをモニターするために使用できる事前定義ダッシュボードを示しています。

| ダッシュボード名        | 説明                                                            | 
|-----------------------|------------------------------------------------------------------------|
| サービス別の概要 (Overview by Service)   | 同じコンテナー・イメージで実行されているサービスのサイズ、パフォーマンス、および制限を表示するには、このダッシュボードを使用します。 | 
| サービスの概要 (Service Overview)      | 単一のサービスのリソース使用量および応答時間を表示するには、このダッシュボードを使用します。 | 
{: caption="表 4. サービスの事前定義ダッシュボードのリスト" caption-side="top"} 



## トポロジー
{: #default_dashboards_topology}

以下の表は、アプリケーション層の論理依存性をモニターするために選択できる事前定義ダッシュボードを示しています。

| ダッシュボード名        | 説明                                                            | 
|-----------------------|------------------------------------------------------------------------|
| CPU 使用率             | ホストと残りのインフラストラクチャーとの間の相互作用をモニターするには、このダッシュボードを使用します。  | 
| ネットワーク・トラフィック (Network Traffic)       | ローカル・プロセスとリモート・ノードのプロセスの間の選択したインスタンスによる帯域幅使用量をモニターするには、このダッシュボードを使用します。 |
| 応答時間        | 選択したホストでのすべてのプロセスの間の通信とネットワーク応答のメトリックをモニターするには、このダッシュボードを使用します。 </br>応答の数と時間が、1 秒の細分性までの平均で示されます。  |
{: caption="表 5. トポロジーの事前定義ダッシュボードのリスト" caption-side="top"} 

**注:** これらのダッシュボードでは、Sysdig エージェントがインストールされておらず、通信が検出されたインスタンスに対して、破線およびグレーのボックスが表示されます。














