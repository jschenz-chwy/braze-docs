---
nav_title: "削除:カタログ・フィールドを削除する"
article_title: "削除:カタログ・フィールドを削除する"
search_tag: Endpoint
page_order: 1

layout: api_page
page_type: reference
description: "この記事では、「カタログフィールドの削除」Braze エンドポイントの詳細について説明します。"

---
{% api %}
# カタログ・フィールドを削除する
{% apimethod delete %}
/catalogs/{catalog_name}/fields/{field_name}
{% endapimethod %}

> カタログ・フィールドを削除するには、このエンドポイントを使用する。
{% alert important %}
このエンドポイントは現在早期アクセス中である。この早期アクセスへ参加することに興味がある場合は、カスタマーサクセスマネージャーにお問い合わせください。
{% endalert %}

## 前提条件

このエンドポイントを使用するには、`catalogs.delete_fields` 権限を持つ [API キー]({{site.baseurl}}/api/basics#rest-api-key/)が必要です。

## レート制限

{% multi_lang_include rate_limits.md endpoint='asynchronous catalog fields' %}

## パスパラメーター

| パラメーター      | required | データ型 | 説明                |
| -------------- | -------- | --------- | -------------------------- |
| `catalog_name` | 必須 | string    | カタログ名。       |
| `field_name`   | 必須 | string    | カタログ・フィールドの名前。 |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

## リクエスト例

```
curl --location --request DELETE 'https://rest.iad-03.braze.com/catalogs/restaurants/fields/ratings' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR-REST-API-KEY' \
```

## 応答

このエンドポイントには、`202` と `404` の2つのステータスコード応答があります。

### 成功応答の例

ステータスコード `202` は、次の応答本文を返す可能性があります。

```json
{
  "message": "success"
}
```

### エラー応答例

ステータスコード `404` は、次の応答本文を返す可能性があります。遭遇する可能性のあるエラーの詳細については、「[トラブルシューティング](#troubleshooting)」を参照のこと。

```json
{
  "errors": [
    {
      "id": "catalog-not-found",
      "message": "Could not find catalog",
      "parameters": [
        "catalog_name"
      ],
      "parameter_values": [
        "restaurants"
      ]
    }
  ],
  "message": "Invalid Request"
}
```

## トラブルシューティング 

以下の表は、返される可能性のあるエラーと、それに関連するトラブルシューティングの手順を示したものである。

| エラー                           | トラブルシューティング                                                  |
| ------------------------------- | ---------------------------------------------------------------- |
| `catalog-not-found`             | カタログ名が有効であることを確認する。                            |
| `field-referenced-by-selection` | カタログフィールドが現在選択によって使用されていることを確認する。 |
| `field-is-inventory`            | カタログ・フィールドがインベントリ・フィールドとして使用されていることを確認する。      |
| `invalid-field-name`            | カタログフィールド名が有効であることを確認する。                      |
{: .reset-td-br-1 .reset-td-br-2}

{% endapi %}