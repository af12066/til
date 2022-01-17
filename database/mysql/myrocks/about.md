# MyRocks

LSM-TreeベースのKVSであるLevelDBをもとに作られたRocksDBについて、それをMySQLのストレージエンジンに移植したもの。InnoDBがB+Treeであるため、それとは対照的。

- https://github.com/facebook/rocksdb
- http://myrocks.io
- https://github.com/facebook/mysql-5.6

## Wiki

GitHubにすべて載っている

- https://github.com/facebook/mysql-5.6/wiki

以下Wikiからピックアップ

### 利点

- InnoDBと比較してストレージ使用量が少ない
- Write Amplificationの少なさ
    - 例えばあるレコードを変更するとして、ページサイズに対して実際に書き込みたいサイズがそれよりも小さい場合は、ページ単位で書き込む場合に余計なオーバーヘッドが発生しているといえる
    - LSM-TreeはそもそもAppend onlyであるため、その懸念はない

### 欠点

- Online DDLが対応していない
- ギャップロックのサポートがない
- ORDER BYが比較的遅い
    - RocksDBのPrefix Key Encodingが原因

## Links

- https://tombo2.hatenablog.com/entry/2020/09/06/155018
