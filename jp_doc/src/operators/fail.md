# fail>: ワークフローを失敗させる

**fail>** 常に失敗を起こし、ワークフローを失敗させる。

このオペレータは`_check`句で前のタスクの結果を検証する[if> オペレータ](if.html)との利用に有用で、検証が通らない時ワークフローが失敗する。 

    +fail_if_too_few:
      if>: ${count < 10}
      _do:
        fail>: count is less than 10!

## オプション

* **fail>**: 文字列

  メッセージであり`_error`タスクが`${error.message}`構文を使う事でそのメッセージを参照出来る。