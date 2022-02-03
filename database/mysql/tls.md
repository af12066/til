# TLS

## ユーザ側の設定

すべての接続でTLSを有効にする場合、`CREATE USER`において`REQUIRE SSL`を指定する。

```sql
CREATE USER 'jeffrey'@'localhost' REQUIRE SSL;
```

`mysql` clientでは、以下のSSL Modeを設定できる。

* `--ssl-mode=REQUIRED`: TLS接続を必須化
* `--ssl-mode=PREFFERED`: TLS接続を試み、接続できなければ平文での通信にフォールバック
* `--ssl-mode=DISABLED`: TLS接続をしない

## サーバ側の設定

`require_secure_transport`システム変数をONにしておく。また、`ssl_ca`, `ssl_cert`, `ssl_key`も設定する。

現在のセッションで暗号化が有効化どうかは、`Ssl_cipher`というステータス変数を確認する。

```sql
SHOW SESSION STATUS LIKE 'Ssl_cipher';
```

## その他

サービスの要件次第で導入し、TLSハンドシェイクや暗号化・復号のオーバヘッドを確認すること。

### 参考資料

* https://dev.mysql.com/doc/refman/8.0/ja/encrypted-connection-protocols-ciphers.html
* https://dev.mysql.com/doc/refman/8.0/ja/using-encrypted-connections.html
