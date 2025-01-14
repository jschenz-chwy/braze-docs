---
nav_title: 購入のロギング
article_title: Unityの購買の記録
platform: 
  - Unity
  - iOS
  - Android
page_order: 3
description: "このリファレンス記事では、Unity プラットフォーム での購買の記録方法について説明します。"

---
 
# 購入のロギング

> アプリ内での購入を記録して、売上を経時的にトラッキングしたり、売上源を横断してトラッキングしたりできます。また、ユーザーを生涯価値でセグメント化することもできます。

Braze は複数の通貨での購入に対応しています。ドル以外の通貨でレポートする購入は、レポートされた日付の為替レートに基づいてドルでダッシュボードに表示されます。

実施にあたっては、まずカスタムイベントs、カスタム属性s、購買イベントが提供するセグメンテーション選択肢の事例を[ベストプラクティス][5]で検討すること。

この機能を使用するには、アプリで正常に購入した後に次のメソッド呼び出しを追加します。

```csharp
AppboyBinding.LogPurchase("product_id", "currencyCode", price(decimal));
```

このメソッドは、数量が1 の購入を記録します。別の数量を渡す場合は、以下のメソッドを呼び出すことができます。

```csharp
AppboyBinding.LogPurchase("product_id", "currencyCode", price(decimal), quantity(int));
```

数量は100 以下である必要があります。Braze では、購入プロパティーの`Dictionary` を渡すことで、購入に関するメタデータの追加もサポートしています。

```csharp
AppboyBinding.LogPurchase("product_id", "currencyCode", price(decimal), quantity(int), properties(Dictionary<string, object>));
```

## 注文レベルでの購入記録
商品レベルではなく、注文レベルで購入を記録したい場合、注文名または注文カテゴリを `product_id` として使用できます。詳細については、[購入オブジェクトの仕様]({{site.baseurl}}/api/objects_filters/purchase_object/#product-id-naming-conventions)を参照してください。 

## 為替コードs

以下のコードs は、サポートされている通貨記号です。これ以外の通貨コードを指定すると警告が記録され、SDK でその他のアクションは実行されません。

- USD、AED、ALL、AMD、ANG、AOA、ARS、AWG、AZN、BAM BD、BND、BD、BD、BND、BD、BD、BT、BD、BT、BD、BT、BZD、CY、CYP、CYP、CUP、CYP、CUP、CUP、CYP、GBP、GGP、GGS、GNF、GNF、GYD、HRK、HUF、IDR、IMP、IQD、IRR、ISK、JEP、JOD、JPOD、KGS、KHR、KMF、KPW、KYD、KZD LBP、LBP、LK、LBP、LK、LBP、LYL、LBP、LK、LK、LB、LK、MK、MNT、MWR、MWN、MZN、NGN、NIO、NOK、NPR、PAB、PEN、PHP、PKR、PKR、PLN、QAR、RUB、RWF、RWF、SBD、SCR、SGD、SL、SRD、SVC、SYP、SZL、TJS、TT、TND、TRY、TWD、TZS、UYU、UZS、VEF、VND UV、WST、XAG、XAU、XCD、XDR、XOF、XPF、XPT、YER、ZMK、ZWL。

## サンプルコード

```csharp
AppboyBinding.LogPurchase("product ID", "USD", 12.5m);
```

## REST API

REST API を使用して購入を記録することもできます。詳細については、[ユーザー API][4] のドキュメントを参照してください。

[4]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-data
[5]: {{site.baseurl}}/developer_guide/platform_wide/analytics_overview/#user-data-collection
