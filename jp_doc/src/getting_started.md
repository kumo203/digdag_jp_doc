# 初めに

## 1. 最新版のダウンロード

Digdagは一つの実行ファイルです。下記の`curl`コマンドで`~/bin`下にインストールできます。

    $ curl -o ~/bin/digdag --create-dirs -L "https://dl.digdag.io/digdag-latest"
    $ chmod +x ~/bin/digdag
    $ echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc

ターミナルウィンドウを開き直すか下記のコマンドを入力して下さい。そうすることでインストールの影響が反映されます。

    $ source ~/.bashrc

もし `digdag --help` コマンドが動作すればDigdagはインストールされています。

(注意: もしzshを使って居る場合は`~/.zshrc` file instead of `~/.bashrc`を変更して下さい).

### ウィンドウズでは？

ウィンドウズではcmd.exeかPowerShell.exeを開き下記のコマンドを正確に入力して下さい。

```
PowerShell -Command "& {[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::TLS12; mkdir -Force $env:USERPROFILE\bin; Invoke-WebRequest http://dl.digdag.io/digdag-latest.jar -OutFile $env:USERPROFILE\bin\digdag.bat}"
```

上記のコマンドが`digdag.bat`というファイルをあなたのホームフォルダーの`bin`という名前のフォルダーにダウンロードします(例:`C:\Users\YOUR_NAME\bin`).

そして下記のコマンドを入力しcmd.exeやPowerShell.exeが`bin`フォルダーの`digdag`コマンドを検索できるようにしましょう。

```
setx PATH "%PATH%;%USERPROFILE%\bin"
```

コマンドウィンドウを開き直して下さい。もし`digdag --help`コマンドが操作方法の目セージを表示すればDigdagはインストールされています。

### パッケージを使うか？

Digdagは複数のプラットフォーム向けに既にパッケージされています。

[![パッケージ状態](https://repology.org/badge/vertical-allrepos/digdag.svg)](https://repology.org/project/digdag/versions)

### curlが働かない？

いくつかの環境(例: Ubuntu 16.04)では下記のエラーが出るかもしれません。

```shell
curl: (77) error setting certificate verify locations:
  CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
```

恐らくSSL証明書ファイルが`/etc/ssl/certs/ca-certificates.crt`に有るのに`curl`がそれを`/etc/pki/tls/certs/ca-bundle.crt`と誤認しています。これを解決するには下記のコマンドを実行します。

```shell
$ sudo mkdir -p /etc/pki/tls/certs
$ sudo cp /etc/ssl/certs/ca-certificates.crt /etc/pki/tls/certs/ca-bundle.crt
```

そしてステップ1.を再び実行します。

### エラーに遭遇した？

もし**'Unsupported major.minor version 52.0'**のようなエラーに遭遇した場合、最新の[Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (**8u72**より新しい物)をダウンロードしインストールして下さい。

## 2. サンプルワークフローの実行

`digdag init <dir>`コマンドがサンプルワークフローを生成します。

    $ digdag init mydag
    $ cd mydag
    $ digdag run mydag.dig

動きましたか？次のステップは`digdag.dig`ファイルに作業を自動化する為のタスク追加です。

次のステップ
----------------------------------

* [アーキテクチャー](architecture.html)
* [ワークフローのスケジューリング](scheduling_workflow.html)
* [ワークフロー定義](workflow_definition.html)
* [更なる演算子の選択肢](operators.html)

