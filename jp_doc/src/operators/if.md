# if>: 条件付き実行

**if>** もし`true`が与えられたら`_do`サブタスクを実行するオペレータ。

    +run_if_param_is_true:
      if>: ${param}
      _do:
        echo>: ${param} == true

もし`false`がオペレータの条件に与えられると`_else_do`サブタスクが実行される。

    +run_if_param_is_false:
      if>: ${param}
      _do:
        echo>: ${param} == true
      _else_do:
        echo>: ${param} == false

## オプション

* **if>**: BOOL値

  `true` または `false`.

* **\_do**: タスク

  もし`true`が与えれると実行されるタスク。

* **\_else_do**: タスク

  もし`false`が与えれると実行されるタスク。

注意: _doか_else_doのどちらかが指定されなければならない。
