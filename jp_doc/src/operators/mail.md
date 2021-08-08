# mail>: メール送信

**mail>** メール送信演算子

Gmail SMTPサーバーを利用するには下記のいずれかを行います:

* a) [Appパスワード](https://security.google.com/settings/security/apppasswords)で新しいAppパスワードを生成します。最初に2ステップ認証を有効化する必要があります。

* b) [安全性の低いアプリ](https://www.google.com/settings/security/lesssecureapps)で安全性の低いアプリを有効化します。最初に2ステップ認証を有効化する必要があります。

      _export:
        mail:
          from: "you@gmail.com"

      +step1:
        mail>: body.txt
        subject: workflow started
        to: [me@example.com]
        cc: [foo@example.com,bar@example.com]

      +step2:
        mail>:
          data: this is email body embedded in a .dig file
        subject: workflow started
        to: [me@example.com]
        bcc: [foo@example.com,bar@example.com]

      +step3:
        sh>: this_task_might_fail.sh
        _error:
          mail>: body.txt
          subject: this workflow failed
          to: [me@example.com]

## シークレット

下記のパラメーターをセットする際は[digdag secretsコマンド](https://docs.digdag.io/command_reference.html#secrets)を利用して下さい。

* **mail.host**: ホスト

  SMTPホスト名。

  例:

  ```
  mail.host: smtp.gmail.com
  ```

* **mail.port**: ポート

  SMTPポート番号。

  例:

  ```
  mail.port: 587
  ```

* **mail.username**: ユーザー名

  SMTPログインユーザー名。

  例:

  ```
  mail.username: me
  ```

* **mail.password**: パスワード

  SMTPログインパスワード。

  例:

  ```
  mail.password: MyPaSsWoRd
  ```
  
  もし`Unsecure 'password' parameter is deprecated.`というメッセージに遭遇した際は、下記のコマンドを利用したか確認して下さい。
  
  ```
  # サーバーモード
  digdag secret --project test_mail --set mail.password=xxxxx
  
  # `~/.config/digdag/secrets/mail.password`にパラメーターを格納したローカルモード
  digdag secret --local --set mail.password=xxxxx
  ```

* **mail.tls**: ブール値
  TLSハンドシェイクの有効化。

  例:

  ```
  mail.tls: true
  ```

* **mail.ssl**: ブール値

  レガシーSSL暗号化の有効化。

  例:

  ```
  mail.ssl: false
  ```

## オプション

* **mail>**: ファイル

  メール本文テンプレートファイルへのパス。ファイルは`${...}`構文の埋め込み変数を含むことが出来る。
  その代わりにdigファイルに本文テキストを埋め込む`{data: TEXT}`をセットすることも出来る。

  例:

  ```
  mail>: mail_body.txt
  ```

  * または :コマンド:`mail>: {data: "こんにちは、これはDigdagからです。"}`

* **subject**: 件名

  メールの件名。

  例:

  ```
  subject: Digdagからのメール
  ```

* **to**: [アドレス1, アドレス2, ...]

  宛先アドレス。

  例:

  ```
  to: [analyst@examile.com]
  ```

* **cc**: [アドレス1, アドレス2, ...]

  CCアドレス。

  例:

  ```
  cc: [analyst@examile.com]
  ```

* **bcc**: [アドレス1, アドレス2, ...]

  BCCアドレス。

  例：:

  ```
  bcc: [analyst@examile.com]
  ```

* **from**: 差出人アドレス
  

  例:

  ```
  from: admin@example.com
  ```

* **host**: 名前

  SMTPホスト名。

  例:

  ```
  host: smtp.gmail.com
  ```

* **port**: 名前

  SMTPポート番号。

  例:

  ```
  port: 587
  ```

* **username**: 名前

  SMTPログインユーザー名。

  例:

  ```
  username: me
  ```

* **tls**: Bool値

  TLSハンドシェイクの有効化。

  例:

  ```
  tls: true
  ```

* **ssl**: Bool値

  レガシーSSL暗号の有効化。

  Examples:

  ```
  ssl: false
  ```

* **html**: Bool値

  HTMLメールの使用（デフォルト: false）

  Examples:

  ```
  html: true
  ```

* **debug**: Bool値

  デバッグログの表示（デフォルト: false）

  例:

  ```
  debug: false
  ```

* **attach_files**: 配列

  添付ファイル。書くエレメントは一つのオブジェクトの物:

  * **path: ファイル**: 添付するファイルへのパス。

  * **content_type: 文字列**: ファイルのContent-Type。デフォルトはapplication/octet-stream。

  * **filename: 名前**: ファイルの名前。デフォルトはパスのベース名。

  例:

      attach_files:
        - path: data.csv
        - path: output.dat
          filename: workflow_result_data.csv
        - path: images/image1.png
          content_type: image/png

