---
nav_title: ニュースフィードアイテムの作成
article_title: ニュースフィードアイテムの作成
page_order: 3
page_type: reference
description: "この参考記事では、ニュースフィードアイテムの作成方法について説明します。ニュースフィードアイテムを使用すると、Web ダッシュボードから永続的なコンテンツをアプリに直接挿入できます。"
channel: news feed

---

# ニュースフィードアイテムの作成

{% alert note %}
ニュースフィードは非推奨になります。Braze では、News Feed ツールを使用するお客様は、コンテンツカードメッセージングチャネルに移動することを推奨しています。これは、より柔軟でカスタマイズ可能で、信頼性が高いチャネルです。詳しくは、 [移行ガイド]({{site.baseurl}}/user_guide/message_building_by_channel/content_cards/migrating_from_news_feed/) をご覧ください。
{% endalert %}

> ニュースフィードアイテムを使用すると、Web ダッシュボードから永続的なコンテンツをアプリに直接挿入できます。さらに、ニュースフィードも、他のすべてのメッセージタイプと同様に、個々のセグメントをターゲットにすることができます。つまり、フィードに表示される内容は、他の個人とはまったく異なる可能性があります。ニュースフィードの可能性はほぼ無限です。

ニュースフィードに関する例や役立つヒントについては、[ケーススタディ][13]、[ユースケース][15]、[ベストプラクティス][16]をご覧ください。

## ステップ 1: カードを作成をクリックする

まず、ユーザーに送信するニュースフィードアイテムの種類を選択する必要があります。ドロップダウンメニューから、4 種類のニュースフィードカードのいずれかを選択できます。

![Braze ダッシュボードの [カードを作成] ボタン。カードを作成するための拡張ドロップダウンオプション:クラシック、キャプション付き画像、バナー。[1]

### ニュースフィードカードの仕様

#### ニュースフィードカード

<br>![Facebook アイコン、ヘッダーに「Facebook でいいね！」と 2 行のテキストが表示されたクラシックカードプレビュー:「ここをクリック!」と「www.facebook.com」。[2]{: style="max-width:40%;"}

標準のニュースフィードカードは次のものから構成されます。

- 110x110 の画像
- タイトル
- 本文
- リンク (アプリ内/ウェブ)

#### キャプション付き画像カード

<br>![アップルパイとリンゴの画像を含むキャプション付き画像カードのプレビュー。写真の下のヘッダー「ホリデーセール！約 50 ドルもお得！」というメッセージに次のテキストを添えます。「期間限定で、プレミアムアップルパイ 4 個を 3 個分の価格でお買い求めいただけます。お急ぎください！このセールは期間限定です。引き換えるには、www.example.com をクリックしてください」[3]{: style="max-width:40%;"}

キャプション付き画像カードは次のものから構成されます。

- 600x450 の画像
- タイトル
- 本文
- リンク (アプリ内/ウェブ)

#### バナーカード

<br>![「これはバナーです」という画像が表示されたバナーカードのプレビュー。][4]{: style="max-width:40%;"}

バナーカードは次のものから構成されます。

- 600x100 の画像
- リンク (アプリ内/ウェブ)

#### 画像のガイドライン

|          カードタイプ         |          アスペクト比         | 画像の推奨サイズ | 画像の最大サイズ |   ファイルタイプ  |
|:-----------------------------:|:----------------------:|:------------------:|:-------------:|
|          クラシック         | 1:1 (幅 110 ピクセル以上) |          500 KB         |         1 MB        | PNG、JPEG、GIF |
|          キャプション付き画像         | 4:3 (幅 600 ピクセル以上) |          500 KB         |         1 MB        | PNG、JPEG、GIF |
|          バナー         | 6:1 (幅 600 ピクセル以上) |          500 KB         |         1 MB        | PNG、JPEG、GIF |
{: .reset-td-br-1 .reset-td-br-2 .reset-td-br-3 .reset-td-br-4}

- PNG ファイルをお勧めします。
- Android で GIF を表示するには、カスタム画像読み込みライブラリが必要です。Glide をお勧めします。
- 小さく高品質な画像は読み込みが速いため、望ましい出力を得るには、可能な限り小さいアセットを使用することをお勧めします。

## ステップ 2: タイトル、概要、画像、リンクを追加する

ニュースフィードカードを作成しましょう。カードのタイトルと概要を作成し、それに添える画像をアップロードします。このページでリンクの動作を設定することもできます。このリンクは、標準リンクまたはアプリ内コンテンツへの[ディープリンク][7]にすることができます。

![カード名、カードのプレビュー、言語のカスタマイズの詳細を表示するニュースフィードアイテムエディター。][6]

## ステップ 3: スケジュールを選択する

ニュースフィードカード エディターの下に、このアイテムをいつ公開するかを指定するオプションがあります。作成後すぐに公開するか、将来の公開時刻を設定するかを選択できます。また、[**ローカルタイムゾーンのユーザーに発行する**] チェックボックスをオンにして、ユーザーのローカル時間の特定の時刻にニュースフィードカードを配信することもできます。

![][8]

## ステップ 4: セグメントを選択する

ニュースフィードカードは、ダッシュボード内で定義した任意の [セグメント][10] を任意のスケジュールでターゲットにするように設定できます。ドロップダウンメニューをクリックして、ターゲットセグメントを選択します。ここでは、メールの可用性やユーザーごとの生涯価値など、高レベルの統計情報を確認できます。

![][11]

## ステップ 5: 詳細を確認して公開する

次に、カードに関するすべての詳細 (および該当する場合はアプリ内メッセージ) を表示するページに移動します。これらの項目に関する詳細を確認し、必要に応じてヘッダーの鉛筆アイコンをクリックして編集することができます。

![][12]

以上で完了です！最初のニュースフィードカードを公開しました。

## オプション: ニュースフィードカードをアプリ内メッセージにリンクする

マルチチャネルキャンペーンは全体的なコンバージョン率とエンゲージメント率の向上につながることが多いため、Braze ではアプリ内メッセージを特定のニュースフィードカードに簡単にリンクできるようにしました。 

ニュースフィードカードを起動すると、新しいフィード統計ページに [関連付けられたアプリ内メッセージを作成] ボタンが表示されます。 これをクリックすると、新しいアプリ内メッセージキャンペーンの作成画面 に移動します。アプリ内メッセージのコピーや外観を入力すると、キャンペーンを同時に開始するため、関連するニュースフィードカードの配信ルールとターゲティングルールが自動的にコピーされます。

## ニュースフィードを整理する

ニュースフィード ページ内でカードを並べ替えることができます。
\- フィード内のカードは、ユーザーが閲覧したかどうかによって最初に並べられ、閲覧されていないアイテムはフィードの上部に表示されます。
  \- カードは、フィード内でインプレッションを受けた場合に既読とみなされます。
  \- インプレッションは、カードがフィード内で表示可能な場合にのみカウントされます (つまり、ユーザーがカードをスクロールして読まない場合、インプレッションはカウントされません)。
\- カードは作成日時順に並べられ、新しいアイテムが先頭に表示されます。

[1]: {% image_buster /assets/img_archive/newsfeed1_new.png %}
[2]: {% image_buster /assets/img_archive/classiccard.png %}
[3]: {% image_buster /assets/img_archive/captionedimage.png %}
[4]: {% image_buster /assets/img_archive/newsfeedbanner.png %}
[6]: {% image_buster /assets/img_archive/news-feed-title-summary_new.png %}
[7]: {{site.baseurl}}/developer_guide/platform_integration_guides/swift/advanced_use_cases/linking/
[8]: {% image_buster /assets/img_archive/newsfeed2_new.png %}
[10]: {{site.baseurl}}/user_guide/engagement_tools/segments/creating_a_segment/#creating-a-segment
[11]: {% image_buster /assets/img_archive/targetsegment_new.png %}
[12]: {% image_buster /assets/img_archive/newsfeedpreview_new.png %}
[13]: https://www.braze.com/customers
[14]: {% image_buster /assets/img_archive/linked-in-app_new.png %}
[15]: {{site.baseurl}}/user_guide/engagement_tools/news_feed/news_feed_use_cases/
[16]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/reporting/