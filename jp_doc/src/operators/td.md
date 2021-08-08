# td>: Treasure Data queries

**td>** トレジャーデータのHiveまたはPrestoクエリーを実行するオペレータ。

    _export:
      td:
        database: www_access

    +simple_query:
      td>: queries/simple_query.sql

    +simple_query_expanded:
      td>:
        data: "SELECT '${session_id}' FROM nasdaq"

    +simple_query_nonexpanded:
      td>:
      query: "SELECT * FROM nasdaq"

    +create_new_table_using_result_of_select:
      td>: queries/select_sql.sql
      create_table: mytable_${session_date_compact}

    +insert_result_of_select_into_a_table:
      td>: queries/select_sql.sql
      insert_into: mytable

    +result_with_connection:
      td>: queries/select_sql.sql
      result_connection: connection_created_on_console

    +result_with_connection_with_settings:
      td>: queries/select_sql.sql
      result_connection: my_s3_connection
      result_settings:
        bucket: my_bucket
        path: /logs/

## 例

  * [例](https://github.com/treasure-data/workflow-examples/tree/master/td)

## シークレット

下記のパラメータをセットする時は[digdag secretsコマンド](https://docs.digdag.io/command_reference.html#secrets)を使用する。

* **td.apikey**: APIキー

  トレジャーデータのクエリーを実行する際のAPIキー。

## オプション

* **td>**: ファイル.sql

  クエリーテンプレートファイルへのパス。ファイルは変数を埋め込む為に`${...}`構文を持つ事が出来る。

  例:

  ```
  td>: queries/step1.sql
  ```

* **data**: クエリー

  文字列としてクエリーを渡すことが出来る。
  （注意：これは実際にはオプションでは無くインデントが必要）
 
  例:

  ```
  td>:
    data: "SELECT * FROM nasdaq"
  ```

* **query**: クエリーテンプレート

  クエリーテンプレート。 文字列は変数を埋め込む為に`${...}`構文を持つ事が出来る。

  例:

  ```
  # 幾人かの顧客をメールするために有効化する
  _export:
    database: project_a

  # このAPIは下記のPrestoのJobとワークフローを追跡するコメントを持つJSONを返す。
  # クエリーは不幸なことに動的に決定される条件を持つ。。。。
  # ("動的"はワークフローがプッシュされた時には条件が決定されてない事を意味する)
  # {"query":"-- https://console.treasuredata.com/app/workflows/sessions/${session_id}\n-- ${task_name}\nSELECT * FROM customers WHERE ..."}
  +get_queries
    http>: http://example.com/get_queries
    store_content: true

  # 与えられた文字列を展開しない
  +td_data:
    td>:
      data: ${JSON.parse(http.last_content)["query"]}
    result_connection: mail_something

  # 与えられた文字列を展開する
  +td_query:
    td>:
    query: ${JSON.parse(http.last_content)["query"]}
    result_connection: mail_something
  ```

* **create_table**: 名前

  実行結果から作られるテーブルの名前。このオプションはもしテーブルが既に存在する場合それを削除する。

  このオプションは DROP TABLE IF EXISTS; CREATE TABLE AS (Presto) または INSERT OVERWRITE (Hive) コマンドをSELECT文の前に加える。もしクエリーが `-- DIGDAG_INSERT_LINE` 行を含む場合はコマンドはそこに挿入される。

  例:

  ```
  create_table: my_table
  ```

* **insert_into**: 名前

  実行結果を追加するテーブルの名前。テーブルがまだ存在しない場合は作成する。

  このオプションは INSERT INTO (Presto) または INSERT INTO TABLE (Hive) コマンドをSELECT文の始まりに加える。もしクエリーが `-- DIGDAG_INSERT_LINE` 行を含む場合はコマンドはそこに挿入される。

  例:

  ```
  insert_into: my_table
  ```

* **download_file**: 名前

  ローカルCSVファイルとしてクエリー結果を保存する。
  
  例:

  ```
  download_file: output.csv
  ```

* **store_last_results**: BOOL値

  最初の1列のクエリー件を `${td.last_results}` 変数へ保存する（デフォルト: false）
  td.last_results はカラム名と値の対応となる。一つの値にアクセスするには `${td.last_results.my_count}` 構文が利用出来る。

  例:

  ```
  store_last_results: true
  ```

* **preview**: BOOL値

  クエリー結果を確認するため表示を試みる。

  例:

  ```
  preview: true
  ```

* **result_url**: 名前
  URLにクエリー結果を出力する。

  例:

  ```
  result_url: tableau://username:password@my.tableauserver.com/?mode=replace
  ```

* **database**: 名前

  データベースの名前

  例:

  ```
  database: my_db
  ```

* **endpoint**: ADDRESS

  API endpoint (default: api.treasuredata.com).

* **use_ssl**: BOOLEAN

  Enable SSL (https) to access to the endpoint (default: true).

* **engine**: presto

  Query engine (`presto` or `hive`).

  Examples:

  ```
  engine: hive
  ```

  ```
  engine: presto
  ```

* **priority**: 0

  Set Priority (From `-2` (VERY LOW) to `2` (VERY HIGH) , default: 0 (NORMAL)).

* **job_retry**: 0

  Set automatic job retry count (default: 0).

  We recommend that you not set retry count over 10. If the job is not succeessful less than 10 times retry, it needs some fix a cause of failure.

* **result_connection**: NAME

  Use a connection to write the query results to an external system.

  You can create a connection using the web console.

  Examples:

  ```
  result_connection: my_s3_connection
  ```

* **result_settings**: MAP

  Add additional settings to the result connection.

  This option is valid only if `result_connection` option is set.

  Examples:

  ```
  result_connection: my_s3_connection
  result_settings:
    bucket: my_s3_bucket
    path: /logs/
  ```

  ```
  result_connection: my_http
  result_settings:
    path: /endpoint
  ```

* **presto_pool_name**: NAME

  Name of a resource pool to run the query in.
  Applicable only when ``engine`` is ``presto``.

  Examples:

  ```
  presto_pool_name: poc
  ```

* **hive_pool_name**: NAME

  Name of a resource pool to run the query in.
  Applicable only when ``engine`` is ``hive``.

  Examples:

  ```
  engine: hive
  hive_pool_name: poc
  ```

* **engine_version**: NAME

  Specify engine version for Hive and Presto.

  Examples:

  ```
  engine: hive
  engine_version: stable
  ```

* **hive_engine_version**: NAME

  Specify engine version for Hive.
  This option overrides ``engine_version`` if ``engine`` is ``hive``.

  Examples:

  ```
  engine: hive
  hive_engine_version: stable
  ```

## Output parameters

* **td.last_job_id** or **td.last_job.id**

  The job id this task executed.

  Examples:

  ```
  52036074
  ```

* **td.last_results**

  The first 1 row of the query results as a map. This is available only when `store_last_results: true` is set.

  Examples:

  ```
  {"path":"/index.html","count":1}
  ```

* **td.last_job.num_records**

  The number of records of this job output.
 
  Examples:
  
  ```
  10
  ```
