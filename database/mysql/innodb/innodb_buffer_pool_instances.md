# innodb_buffer_pool_instances

https://dev.mysql.com/doc/refman/8.0/ja/innodb-multiple-buffer-pools.html

InnoDBのバッファプールは複数のインスタンスで構成できる。ただし、バッファプールサイズが1GB未満であればインスタンスは1個。

`innodb_buffer_pool_size`は `innodb_buffer_pool_instances * innodb_buffer_pool_chunk_size` の倍数となるべきである。ただし、`innodb_buffer_pool_size` >= `innodb_buffer_pool_instances * innodb_buffer_pool_chunk_size`。　　
`innodb_buffer_pool_chunk_size`のサイズ単位でインスタンスサイズが増減する。

インスタンスを複数用意することで、バッファプールへのアクセス競合を減らすことができる。

```
mysql> show variables like 'innodb_buffer_pool%';
+-------------------------------------+----------------+
| Variable_name                       | Value          |
+-------------------------------------+----------------+
| innodb_buffer_pool_chunk_size       | 134217728      |
| innodb_buffer_pool_dump_at_shutdown | ON             |
| innodb_buffer_pool_dump_now         | OFF            |
| innodb_buffer_pool_dump_pct         | 25             |
| innodb_buffer_pool_filename         | ib_buffer_pool |
| innodb_buffer_pool_instances        | 1              |
| innodb_buffer_pool_load_abort       | OFF            |
| innodb_buffer_pool_load_at_startup  | ON             |
| innodb_buffer_pool_load_now         | OFF            |
| innodb_buffer_pool_size             | 134217728      |
+-------------------------------------+----------------+
10 rows in set (0.00 sec)
```
