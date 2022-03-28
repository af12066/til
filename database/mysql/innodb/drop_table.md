# DROP TABLE

## DROP TABLEとロック

Buffer poolが32GB以上ある場合やAdaptive Hash Indexを有効にしている場合、DROP TABLE時にロックがかかり詰まるので注意。この問題はMySQL 8.0.23で修正された。

- https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-23.html#mysqld-8-0-23-feature
