# TRUNCATE TABLE

https://dev.mysql.com/doc/refman/8.0/en/truncate-table.html

## 動作概要

- MySQL 8.0においては、`DROP TABLE` + `CREATE TABLE`の動作をする
- `AUTO_INCREMENT`は初期化される

## 注意点

> In MySQL 5.7 and earlier, on a system with a large buffer pool and innodb_adaptive_hash_index enabled, a TRUNCATE TABLE operation could cause a temporary drop in system performance due to an LRU scan that occurred when removing the table's adaptive hash index entries (Bug #68184). The remapping of TRUNCATE TABLE to DROP TABLE and CREATE TABLE in MySQL 8.0 avoids the problematic LRU scan.

MySQL 5.7とそれ以前では、`innodb_adaptive_hash_index`パラメータによってAdaptive Hash Index (AHI) が有効化されているときにTRUNCATE TABLEによってAHIのLRUスキャンが走り、これが重くなる。  
また、Buffer Poolのサイズが大きい場合でも、それをなめて削除するのに時間がかかる。

- https://next4us-ti.hatenablog.com/entry/2020/12/12/233844
