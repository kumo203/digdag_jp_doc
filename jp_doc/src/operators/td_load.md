# td_load>: トレジャーデータバルクローディング

**td_load>** 演算子はストレージ、データベース、またはサービスからのデータをロードします。

    +step1:
      td_load>: config/guessed.yml
      database: prod
      table: raw

## 例

  * [例](https://github.com/treasure-data/workflow-examples/tree/master/td_load)

## シークレット

下記のパラメーターをセットする際は[digdag secrets コマンド](https://docs.digdag.io/command_reference.html#secrets)を利用して下さい。

* **td.apikey**: APIキー

  Treasure Dataバルクロードジョブを送出する時に使われるTreasure Data APIキー。

## オプション

* **td_load>**: ファイル.yml

  YAMLテンプレートファイルへのパス。この設定はtdコマンドを使い推測される必要がある。もしトレジャーデータのデータコネクタージョブが保存されていれば、YAMLパスの代わりに[Unique ID](https://tddocs.atlassian.net/wiki/spaces/PD/pages/1083717/Creating+a+YAML+Incremental+Data+Transfer+File#Configuring-your-Unique-ID-Incremental-Data-Transfer)を使う事が出来る。

  例:

  ```
  td_load>: imports/load.yml
  ```

  ```
  td_load>: unique_id
  ```

* **database**: 名前

  データがロードされて行くデータベースの名前。

  例:

  ```
  database: my_database
  ```

* **table**: 名前

  データがロードされていくテーブルの名前。

  例:

  ```
  table: my_table
  ```

* **endpoint**: アドレス

  APIエンドポイント（デフォルト：api.treasuredata.com）

* **use_ssl**: Bool値

  エンドポイントへのアクセスでのSSL（https）有効化（デフォルト：true）


## 出力パラメータ

* **td.last_job_id** または **td.last_job.id**

  実行されたタスクのジョブID。

  例:

  ```
  52036074
  ```

* **td.last_job.num_records**

  ジョブ出力のレコード数。
 
  例:
  
  ```
  10
  ```
