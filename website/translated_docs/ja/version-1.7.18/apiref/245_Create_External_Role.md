---
id: version-1.7.18-245_Create_External_Role
title: ExtRole登録
sidebar_label: ExtRole登録
---
## 概要
ExtRoleを登録する
### 必要な権限
auth
### 制限事項
* OData 制限
	* リクエストヘッダのAcceptは無視される
	* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
	* リクエストボディはJSON形式のみ受け付ける
	* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
	* $formatクエリオプションは無視される

## リクエスト
### リクエストURL
```
{CellURL}__ctl/ExtRole
```
### メソッド
POST
### リクエストクエリ
|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
### リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UUIDで32文字の文字列}を設定する|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
### リクエストボディ
|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|ExtRole|外部ロール名(URL)|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn<br>Boxに紐付いている場合:/{アプリセル名}/&#95;&#95;role/&#95;&#95;/{Role名}<br>※ただし、BoxにSchema情報が登録されていない場合、Boxに紐付いていないとみなされる<br>Boxに紐付いていない場合:/{Cell名}/&#95;&#95;role/&#95;&#95;/{Role名}|○||
|_Relation.Name|関係対象のRelation名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー):(コロン)は指定不可<br>Relation登録APIにて登録済みのRelation|○||
|_Relation._Box.Name|関係対象のRelationが紐づくBox名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>Relation登録APIにて登録済みのRelationが紐づくBox名<br>null|×||
### リクエストサンプル
```JSON
{
  "ExtRole": "https://cell2.unit1.example/__role/__/role1",
  "_Relation.Name": "relation1",
  "_Relation._Box.Name": "box1"  
}
```


## レスポンス
### ステータスコード
201
### レスポンスヘッダ
|項目名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
### ODataレスポンスヘッダ
|項目名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|Location|作成したリソースへのURL||
|DataServiceVersion|ODataのバージョン||
|ETag|リソースのバージョン情報||
### レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト|項目名|Data Type|備考|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

### ExtRole固有レスポンスボディ
|オブジェクト|項目名|Data Type|備考|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ExtRole|
|{2}|ExtRole|string|外部ロールURL|
|{2}|_Relation.Name|string|Relation名|
|{2}|_Relation._Box.Name|string|Relationに紐付くBox名|
### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://cell1.unit1.example/__ctl/ExtRole(ExtRole='https%3A%2F%2Fcell2.unit1.example
%2F__role%2F__%2Frole1',_Relation.Name='relation1',_Relation._Box.Name='box1')",
        "etag": "W/\"1-1486717404966\"",
        "type": "CellCtl.ExtRole"
      },
      "ExtRole": "https://cell2.unit1.example/__role/__/role1",
      "_Relation.Name": "relation1",
      "_Relation._Box.Name": "box1",
      "__published": "/Date(1486717404966)/",
      "__updated": "/Date(1486717404966)/"
    }
  }
}
```


## cURLサンプル

```sh
curl "https://cell1.unit1.example/__ctl/ExtRole" -X POST -i \
-H 'Authorization: Bearer AA~PBDc...(省略)...FrTjA' -H 'Accept: application/json' \
-d '{ "ExtRole": "https://cell2.unit1.example/__role/__/role1", "_Relation.Name": \
"relation1", "_Relation._Box.Name": "box1"}'
```
