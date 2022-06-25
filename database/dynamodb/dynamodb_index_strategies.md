# DynamoDB Index戦略

## Secondary Index

DynamoDBには、以下の2種類のインデックスがある。

* Global Secondary Index (GSI)
* Local Secondary Index (GSI)

通常はPartition KeyとSort Keyを用いてItemのフィルタリングや取得を行なうが、それ以外の属性を用いてフィルタリングしたい場合にSecondary Indexを用いることができる。

## Local Secondary Index

* Partition Keyはもとのテーブルと同じで、Sort Keyが異なるケース。関係代数における射影みたいなイメージ
* LSIはテーブル作成時にしか作成できない。テーブル作成後に後付け不可
* 1つのPartition Keyあたり、合計10GBを超えるItemは格納できない点に注意
* Index内の属性は、もとのテーブルに存在するものであれば任意に追加可能（属性名を指定するか、まったく含めないという設定を記述する）

たとえば以下のようなテーブルがあるとする。ただしPKはPrimary Key、SKはSort Keyを表す:

| Year (PK) | Title (SK) | Genre | Directors | Rating |
|----------:|------------|-------|-----------|-------:|
| 2013 | Rush | Action | Ron Howard | 8.3 |
| 1995 | Toy Story | Family | John Lasseter | 8.3 |
| 2014 | Paddington | Comedy | Paul King | |
| 2014 | Rio 2 | Animation | Carlos Saldanha | |

Local Secondary Indexは次のように構築できる。ただしAttrは任意の属性を表す:

| Year (PK) | Genre (SK) | Attr | Attr | Attr |
|----------:|------------|------|------|------|
| 2013 | Action | | | |
| 1995 | Family | | | |
| 2014 | Comedy | | | |
| 2014 | Animation | | | |

## Global Secondary Index

* もとのテーブルとは異なるPartition Key, Sort Keyを設定可能な射影
* Index内の属性は、もとのテーブルに存在するものであれば任意に追加可能（属性名を指定するか、まったく含めないという設定を記述する）
* もとのテーブルとは別に、キャパシティユニットを読み書きそれぞれで設定する
* GSIは1テーブルあたり20個まで設定可能。これは、GSIが増えるごとにテーブルの書き込み負荷が増えるため、それを抑える意味もある
* GSIに対してGetItemは不可。参照はScanやQueryのようなフィルタ操作のみ

## Index作成のフェーズ

https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/GSI.OnlineOps.html#GSI.OnlineOps.Creating

1. コンピューティングリソースとストレージを割り当てる
  * `IndexStatus`: `CREATING`
2. バックフィル（射影に基づいてIndexに書き込む属性を決定し、実際にIndexに書き込む）
  * `IndexStatus`: `CREATING`, `Backfilling`: `true`
3. `IndexStatus`: `ACTIVE`
  * `ACTIVE`になるまでQueryやScanは行えない

## Indexの削除

* GSIは削除可能。ただしアプリケーションがそのGSIを使うコードを記述していた場合、アプリケーションはエラーとなる

## Index戦略

* Sparse Index
  * https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/bp-indexes-general-sparse-indexes.html
  * DynamoDBのGSIはSparse（必須でない属性をPartition KeyまたはSort Keyに設定できる）
  * 必須でない属性をPartition KeyまたはSort Keyに含めることで、その時点でItemが絞られてGSIのサイズが最小限となり、キャパシティユニットの消費を抑えられる
* Inverted Index
  * もとのテーブルのPartition KeyおよびSort Keyに対して、GSIはもとのテーブルのSort KeyをPartition Keyに、もとのテーブルのPartition KeyをSort Keyに設定する戦略
  * 多対多のモデリングが可能 cf. https://codeburst.io/inverted-index-gsi-overloading-and-sparse-index-in-dynamodb-cbfb520b2696
