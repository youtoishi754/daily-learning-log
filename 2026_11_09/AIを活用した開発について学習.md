# DOM操作・SPA・localStorage まとめ

## DOM操作
- HTMLをJavaScriptから読み書きすること
- 取得:`querySelector`
- 変更:`textContent`、`classList`
- 追加・削除:`createElement`、`appendChild`、`remove`
- イベント:`addEventListener`

## SPA
- 最初に1枚のHTMLを読み込み、以降はDOM操作で画面を書き換える方式
- データのやり取りはAPI(JSON)で行う
- メリット:画面遷移が速い
- デメリット:初回が重い、SEOやURL設計に工夫が要る

## localStorage
- ブラウザにキーと値を無期限で保存する(約5MB、文字列のみ)
- オブジェクトは `JSON.stringify` / `JSON.parse` で変換
- 機密情報は置かない
