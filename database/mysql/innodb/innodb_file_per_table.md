# innodb_file_per_table

https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_file_per_table
https://dev.mysql.com/doc/refman/8.0/en/innodb-file-per-table-tablespaces.html

## 概要

システムパラメータ。MySQL 8.0ではデフォルトで有効。テーブルごとにibdファイルが作成されるようになる。

この場合、`DROP TABLE`や`TRUNCATE TABLE`をファイル単位で削除できるメリットがある。これにより、ディスク領域をOSに返却することになる。

`innodb_file_per_table`がOFFの場合は、共有ファイルの該当する場所に削除マークをつけて走査する必要がある。

- https://gihyo.jp/dev/serial/01/mysql-road-construction-news/0055

## 注意

> The buffer pool is scanned when dropping a table that resides in a file-per-table tablespace, which can take several seconds for large buffer pools. The scan is performed with a broad internal lock, which may delay other operations.

`DROP TABLE` (`TRUNCATE TABLE`) の際にバッファプールのスキャンが走り、internal lockがかかるため、バッファプールが大きい場合は詰まる可能性がある。
