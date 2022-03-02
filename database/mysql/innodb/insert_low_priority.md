# INSERT LOW_PRIORITY

https://dev.mysql.com/doc/refman/8.0/ja/insert.html

LOW_PRIORITYを指定した場合、INSERT対象のテーブルに対して、他のクライアントが操作していないときにINSERTを行う。他のクライアントが操作しているときは、その操作終了までINSERTを待つ。  
**LOW_PRIORITYはInnoDBでは無効となる**。

> LOW_PRIORITY は、テーブルレベルロック (MyISAM、MEMORY、MERGE など) のみを使用するストレージエンジンにのみ影響します。

pt-online-schema-changeによるレコードのINSERTにはLOW_PRIORITYが付与されている。

https://github.com/percona/percona-toolkit/blob/40f179cf26e849f4425b2b79172087f26fd49c3c/bin/pt-online-schema-change#L10019
