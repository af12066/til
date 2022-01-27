# innodb_flush_log_at_trx_commit

## 概要

https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit

`innodb_flush_log_at_trx_commit`というパラメータについて、0, 1, 2のいずれかの値を設定できる。これは、

- ログバッファをログファイルに書き込むタイミング
- ログのFlush（OSのファイルシステムキャッシュ(メモリ)からディスクへのFlush）のタイミング

を制御できる。

- 0: 1秒ごとにログバッファからログファイルへの書き込みが行われる。ディスクへのFlushもまた1秒ごとに実行される
- 1: トランザクションのCOMMITの直後にログバッファからログファイルへの書き込みが行われる。ディスクへのFlushもまたCOMMITの直後に行われる
- 2: トランザクションのCOMMITの直後にログバッファからログファイルへの書き込みが行われる。ディスクへのFlushは1秒ごとに実行される

デフォルトは1だが、2に変更してFlushを遅延させることでパフォーマンスを向上させる場合がある。ただし、Flushの前にシステムがクラッシュした場合はメモリ上のログが消失する。

## 参考資料

- [MySQLを止めずにレプリケーションをブーストする小技](https://mita2db.hateblo.jp/entry/2020/07/18/142220)
- [ゲームを題材に学ぶ 内部構造から理解するMySQL 第2回　ゲーム系で確認すべきパラメータ](https://gihyo.jp/dev/serial/01/game_mysql/0002)

# sync_binlog

## 概要

https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html

`innodb_flush_log_at_trx_commit`に関連して、`sync_binlog`を調整することもある。

マスタ・レプリカ間の同期のためのbinlogについて、`sync_binlog`を0に設定するとbinlogをディスクに書き込まなくなる。デフォルトは1。  
0のとき、ディスクへのFlushはMySQLでなくOSに依存させることで、レプリケーションのパフォーマンスを向上できる。ただし、システムがクラッシュしたときはbinlogも消失する。

## 参考資料

- [Amazon RDS for MySQL のパラメータ設定 パート 2: レプリケーション関連のパラメータ](https://aws.amazon.com/jp/blogs/news/best-practices-for-configuring-parameters-for-amazon-rds-for-mysql-part-2-parameters-related-to-replication/)
- [私は如何にして詳解 MySQL 5.7を執筆するに至ったか](http://nippondanji.blogspot.com/2016/10/mysql-57.html)
