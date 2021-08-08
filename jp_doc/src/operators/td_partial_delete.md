# td_partial_delete>: トレジャーデータテーブルの範囲削除

**td_partial_delete>** トレジャーデータテーブルからある範囲のレコードを削除するオペレータ。

td_partial_deleteを使った場合ストリーミングインポートを使ってインポートされたレコードは数時間の間は削除されない事に注意して下さい。INSERT INTO、データコネクター、そしてバルクインポートを使ってインポートされたレコードは即座に削除されます。

時間範囲は時間単位。ゼロでは無い値を分や秒に設定する場合は排除される。

    +step1:
      td_partial_delete>: my_table
      database: mydb
      from: 2016-01-01T00:00:00+08:00
      to:   2016-02-01T00:00:00+08:00

## シークレット

下記のパラメータをセットする時は[digdag secretsコマンド](https://docs.digdag.io/command_reference.html#secrets)を使用する。

* **td.apikey**: APIキー
  トレジャーデータクエリーを実行する際のAPIキー。

## Parameters

* **td_partial_delete>**: 名前

  テーブルの名前

  Examples:

  ```
  td_partial_delete>: my_table
  ```

* **database**: 名前

  データベースの名前。

  例:

  ```
  database: my_database
  ```

* **from**: yyyy-MM-ddTHH:mm:ss[Z]

  その時間のレコードの削除（包括）。実際の時間範囲は:コマンド:`[from, to)`。値はUNIXタイムスタンプ整数（秒）かISO-8601 (yyyy-MM-ddTHH:mm:ss[Z])フォーマットの文字列。

  例:

  ```
  from: 2016-01-01T00:00:00+08:00
  ```

* **to**: yyyy-MM-ddTHH:mm:ss[Z]

  その時間のレコードの削除（排他）。実際の時間範囲は:コマンド:`[from, to)`。値はUNIXタイムスタンプ整数（秒）かISO-8601 (yyyy-MM-ddTHH:mm:ss[Z])フォーマットの文字列。

  例:

  ```
  to: 2016-02-01T00:00:00+08:00
  ```

* **endpoint**: アドレス

  APIエンドポイント（デフォルト：api.treasuredata.com）

* **use_ssl**: Bool値

  エンドポイントへのアクセスでのSSL（https）有効化（デフォルト：true）

