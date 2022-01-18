# Instant DDL

## 概要 

`ALGORITHM=INSTANT` なDDL。

https://dev.mysql.com/doc/refman/8.0/en/innodb-online-ddl-operations.html において、Instant: YesであるDDLが対象。このとき、従来のOnline DDLと比較してテーブルのメタデータロックがかからないのが特徴。

特に、`ADD COLUMN` の改善が顕著。MySQL 5.7では内部でテーブルの作り替えが発生していたが、MySQL 8.0にてデータディクショナリのメタデータ変更だけで済むようになった。  
ただし、Instant ADD COLUMNを適用するには、テーブルの末尾にカラムを追加すること、fulltext indexのないテーブルである必要がある。

https://dev.mysql.com/blog-archive/mysql-8-0-innodb-now-supports-instant-add-column/
