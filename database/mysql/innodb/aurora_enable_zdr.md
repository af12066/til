# ZDR (Amazon Aurora)

https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Replication.html#AuroraMySQL.Replication.Availability

`aurora_enable_zdr`パラメータがONのとき、Auroraインスタンスはダウンタイムなし（接続が維持されたままということ）に再起動可能（ただしベストエフォートなので、いくつかのトランザクションはロールバックされる可能性あり）。  
Aurora 2.10.0以降はデフォルトでONになっている。

接続は維持されたままだが、`LAST_INSERT_ID`や`AUTO_INCREMENT`が巻き戻る可能性あり？
