---

copyright:
  years:  2018, 2019
lastupdated: "2019-03-06"

keywords: Sysdig, IBM Cloud, monitoring, panels

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


# パネルの処理
{: #panels}

パネルを使用して、メトリックまたはメトリックのグループをダッシュボードに表示します。 パネルに対して、コピー、有効範囲の変更、複製、削除、エクスポート、および探索を行うことができます。
{:shortdesc}

以下のいずれかのパネル・タイプを使用できます。

| タイプ | 説明 |
|------|-------------|
| 折れ線グラフ | このパネルを使用して、1 つ以上のメトリックの傾向を経時的に表示します。  |
| Area | このパネルを使用して、1 つ以上のメトリックの傾向を経時的に表示します。  |
| 上部リスト | このパネルを使用して、エンティティーのグループ間でメトリックを比較します。 棒グラフは降順でソートされます。  |
| ヒストグラム | このパネルを使用して、バケットでのメトリックの頻度分布を表示します。  |
| トポロジー | このパネルを使用して、インフラストラクチャーをトポロジー・マップとして視覚化し、マップ内のエンティティー間の関係を視覚化します。  |
| 番号 | このパネルを使用して、1 つ以上のエンティティーについて経時的に集約されたメトリックの値を表す単一の数字を表示します。  |
| テーブル | このパネルを使用して、インフラストラクチャーの数値データをメトリックおよびセグメントに基づいて表示します。  |
| テキスト | このパネルを使用して、テキストを追加します。 マークダウンを使用して、テキストを追加します。  |
{: caption="表 1. パネル・タイプ" caption-side="top"} 



## ダッシュボードへのパネルのコピー
{: #panels_copy}

パネルをコピーするには、以下のステップを実行します。

1. Web UI で*「ダッシュボード」** セクションにナビゲートします。 ダッシュボードを選択します。 次に、コピーするメトリックを表示するパネルを特定します。

2. *「その他のオプション (More options)」*アイコン ![3 つのドットのアイコン](images/actions.png) を選択し、**「パネルのコピー (Copy panel)」** ![「コピー」アイコン](images/actions.png) を選択します。

3. リストされているいずれかのダッシュボードを選択するか、または新しいダッシュボードの名前を入力します。 

4. **「コピーして開く (Copy and Open)」**をクリックします。



## パネルの有効範囲の変更
{: #panels_scope}

パネルの有効範囲を変更するには、以下のステップを実行します。

1. Web UI で*「ダッシュボード」** セクションにナビゲートします。 ダッシュボードを選択します。 次に、有効範囲を変更するメトリックを表示するパネルを特定します。

2. パネルで、**「有効範囲の編集 (Edit Scope)」**をクリックして、デフォルトの有効範囲を変更します。 

    デフォルトでは、**「すべて (Everywhere)」**が選択されています。
    
3. 有効範囲を選択します。 

4. オプションで、**「カスタム・パネルの有効範囲をオーバーライド (Override the custom panel scopes)」**をクリックして、カスタム有効範囲が現在定義されているすべてのパネルの有効範囲をオーバーライドします。 

    **注: このアクションを元に戻すことはできません。** 

    ダッシュボードの有効範囲をインフラストラクチャー全体となるようリセットするか、または既存のダッシュボードの有効範囲をインフラストラクチャー全体となるよう更新するには、**「すべて (Everywhere)」**を選択します。
    {: tip}

5. **「保存」**をクリックします。



## パネルの複製
{: #panels_duplicate}

現在のダッシュボードでパネルを複製するには、以下のステップを実行します。

1. Web UI で*「ダッシュボード」** セクションにナビゲートします。 ダッシュボードを選択します。 次に、コピーするメトリックを表示するパネルを特定します。

2. *「その他のオプション (More options)」*アイコン ![3 つのドットのアイコン](images/actions.png) を選択し、**「パネルの複製 (Duplicate panel)」** ![「コピー」アイコン](images/duplicate.png) を選択します。


## パネルの削除
{: #panels_delete}

現在のダッシュボードでパネルを削除するには、以下のステップを実行します。

1. Web UI で*「ダッシュボード」** セクションにナビゲートします。 ダッシュボードを選択します。 次に、コピーするメトリックを表示するパネルを特定します。

2. *「その他のオプション (More options)」*アイコン ![3 つのドットのアイコン](images/actions.png) を選択し、**「パネルの削除 (Delete panel)」** ![「コピー」アイコン](images/delete.png) を選択します。

3. **「はい、パネルを削除します (Yes, delete panel)」**をクリックして、パネルの削除を確認します。



## パネルからのデータのエクスポート
{: #panels_export}

データをエクスポートする場合は、以下の情報を考慮してください。

* 折れ線グラフから、**JSON ファイル**にデータをエクスポートできます。
* テーブル・チャートまたは折れ線グラフから、**CSV ファイル**にデータをエクスポートできます。

パネルからデータをエクスポートするには、以下のステップを実行します。

1. Web UI で*「ダッシュボード」** セクションにナビゲートします。 ダッシュボードを選択します。 次に、コピーするメトリックを表示するパネルを特定します。

2. *「その他のオプション (More options)」*アイコン ![3 つのドットのアイコン](images/actions.png) を選択します。

3. 次のオプションのいずれかを選択してください。

    * JSON 形式のファイルにデータをエクスポートするには、**「JSON のエクスポート (Export JSON)」**を選択します。

    * CSV 形式のファイルにデータをエクスポートするには、**「CSV のエクスポート」**を選択します。

4. **「ファイルの保存 (Save the file)」**をクリックします。




## アラートの作成
{: #panels_alert}

パネルから直接アラートを作成できます。

アラートを作成するには、以下のステップを実行します。

1. Web UI で*「ダッシュボード」** セクションにナビゲートします。 ダッシュボードを選択します。 次に、コピーするメトリックを表示するパネルを特定します。

2. *「その他のオプション (More options)」*アイコン ![3 つのドットのアイコン](images/actions.png) を選択します。

3. **「アラートの作成 (Create Alert)」**を選択します。

4. アラートを構成します。

5. **「作成」**をクリックします。

