# Adaptive Hash Index

## 概要

Index keyのprefixを用いてメモリ内にハッシュインデックスを作成する。`innodb_adaptive_hash_index`パラメータで有効・無効化の変更が可能。  
MySQLはデフォルトで有効になっているが、Amazon Auroraではデフォルトで無効化されている。

ハッシュなので`=`や`<>`（`!=`）演算子には有効だが、比較や`LIKE`、`%`演算子のクエリは効果を得られない。同様の理由で`ORDER BY`の高速化もできない。

- https://dev.mysql.com/doc/refman/8.0/ja/innodb-adaptive-hash.html
- https://dev.mysql.com/doc/refman/8.0/ja/index-btree-hash.html

`SHOW ENGINE INNODB STATUS`に`INSERT BUFFER AND ADAPTIVE HASH INDEX`のセクションがあり、どのくらいAHIを使っているかを確認できる。

```
-------------------------------------
INSERT BUFFER AND ADAPTIVE HASH INDEX
-------------------------------------
Ibuf: size 1, free list len 0, seg size 2, 0 merges
merged operations:
 insert 0, delete mark 0, delete 0
discarded operations:
 insert 0, delete mark 0, delete 0
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
Hash table size 34679, node heap has 0 buffer(s)
0.00 hash searches/s, 4.56 non-hash searches/s
```

普段どのようなクエリが発行されるかといった特性やワークロード（更新が多いかなど）に依存するので、検証が必要。

https://www.percona.com/blog/2019/06/25/adaptive-hash-index-on-aws-aurora/
