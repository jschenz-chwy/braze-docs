---
page_order: 20
nav_title: ベストプラクティス
article_title: プッシュのベストプラクティス
description: "このページには、プッシュメッセージが煩わしさではなくエンゲージメントを喚起するようにするためのプッシュのベストプラクティスとユースケースが含まれています。"
channel: push
---

# プッシュのベストプラクティス

プッシュ通知は、アプリのユーザーとやり取りするための強力なツールですが、タイムリーで関連性の高いメッセージを確実に配信するために注意して使用する必要があります。プッシュメッセージを送信する前に、以下のベストプラクティスを参考にして、知っておくべきことや確認すべきことをお伝えください。

{% alert important %}
プッシュメッセージは、Apple App StoreおよびGoogleのPlayストアポリシーのガイドライン(特に、プッシュメッセージを広告、スパム、プロモーションなどとして使用することに関するガイドライン)に準拠している必要があります。[詳しくは、モバイルプッシュ規制]({{site.baseurl}}/user_guide/message_building_by_channel/push/about/#mobile-push-regulations-for-apps)についての記事をご覧ください。
{% endalert %}

## オプティマイズのターゲティング

### 関連するユーザーデータの収集

プッシュ通知は、タイムリーで関連性の高い通知でユーザーをターゲットにするように注意して扱う必要があります。Brazeは、関連するセグメントをターゲットにするために使用できる有用なデバイスおよび使用情報を収集します。この情報は、アプリに固有のカスタムイベントと属性で補完する必要があります。そのデータを使用して、メッセージを慎重にターゲットにして開封率を高め、ユーザーがプッシュを無効にするインスタンスを減らすことができます。

### 通知設定ページを作成する

アプリ内に設定ページを作成して、ユーザーが受信する通知を通知できるようにすることができます。一般的なアプローチは、アプリ設定のステータスに対応するブール型のカスタム属性をBrazeで作成することです。たとえば、ニュース アプリには、ニュース速報、スポーツ ニュース、政治のサブスクリプション設定を含めることができます。

ニュース アプリで、政治に関心のあるユーザーのみをターゲットとするキャンペーンを作成する場合は、属性フィルターをセグメントに追加します `Subscribes to Politics` 。true に設定すると、通知をサブスクライブしているユーザーのみが通知を受信します。

カスタム属性の設定については、 [iOS][6]、 [Android][7]、 [REST API][8]の以下の記事をご参照ください。

## オプトインと関連性の向上

### ユーザー権限の取得

プッシュが有効な一般的な統計は、ユーザーがオペレーティング システムで通知を承認したかどうかに関連しています。ユーザーがiOSで通知をオフにすると、Appleはプッシュトークンの送信を許可しないため、システムから自動的に削除されます。

Android 13 以降では、プッシュ通知を表示する前に許可を得る必要があります。古いバージョンの Android では、デフォルトでユーザーに通知がサブスクライブされます。

### プッシュのプライムユーザー

ユーザーにプッシュ許可を求める機会は 1 回しかなく、ユーザーが拒否した後、デバイスの設定でプッシュを再度有効にするように説得するのは非常に困難です。このため、システムプロンプトを表示する前に、アプリ内メッセージを使用してユーザーをプッシュできるように準備する必要があります。オプトインを増やす方法について詳しくは、 [入門書のアプリ内メッセージをプッシュ][2] するを参照してください。

### プッシュ サブスクリプション コントロールを追加する

ユーザーがデバイスレベルで通知をオフにしてフォアグラウンドプッシュトークンが完全に削除されないようにするには、ユーザーがアプリ内で直接プッシュサブスクリプションを制御できるようにします。詳細については、「 [プッシュサブスクリプションの状態の更新][10] 」を参照してください。

### プッシュ サブスクリプションの状態を理解する

プッシュ サブスクリプションの状態は、プッシュが配信されることを保証するものではなく、ユーザーが通知を受信するにはプッシュが有効になっている必要があります。これは、ユーザー プロファイルには、フォアグラウンド プッシュのアクセス許可が異なる複数のデバイスが含まれる場合があり、プッシュ サブスクリプションの状態は 1 つしかないためです。

ユーザーがアプリの有効なフォアグラウンド プッシュ トークンを持っていない場合 (つまり、設定を使用してデバイス レベルでプッシュ トークンをオフにし、通知を受信しないことを選択した場合)、サブスクリプションの状態は引き続きプッシュと見なす `subscribed` ことができます。ただし、フォアグラウンドプッシュトークンが有効ではないため、このユーザーはBrazeにいません `Push Enabled for App` 。

さらに、ユーザープロファイルに他のアプリの有効なプッシュトークンまたは登録済みのプッシュトークンがない場合、 `Push Enabled` セグメンテーションのフィルターもfalseになります。

## 応答しないユーザーに対するサンセットポリシーの実装

関連性のあるタイムリーなプッシュ通知のみを送信した場合でも、一部のユーザーはそれらに応答せず、スパムと見なす可能性があります。ユーザーがプッシュ通知を繰り返し無視した履歴を示しているとします。その場合は、アプリの通信にイライラしたり、アプリを完全にアンインストールしたりする前に、プッシュの送信を停止することをお勧めします。 

これを行うには、 [サンセットポリシー][9] を作成して、直接または影響を受けたオープンを長期間行っていないユーザーへのプッシュ通知の送信を最終的に停止します。

1. 直接開封または影響を受けた開封に基づいて、応答しないユーザーを特定します。
2. これらのユーザーへのプッシュ通知の送信を段階的に停止します。
3. プッシュ通知を完全に削除する前に、プッシュ通知を受け取らなくなる理由を説明する最後の通知を 1 つ配信します。これにより、ユーザーはその通知を開くことで、継続的なプッシュへの関心を示すことができます。
4. サンセットポリシーが有効になったら、 [アプリ内メッセージ][13] を使用して、プッシュを受信しなくなりますが、アプリ内メッセージングチャネルは引き続き興味深く役立つ情報を配信することをユーザーに通知します。

最初にプッシュをオプトインしたユーザーへのプッシュ送信を停止するのは気が進まないかもしれませんが、特に以前にプッシュを無視していたユーザーは、他のメッセージングチャネルでより効果的にプッシュにリーチできることに注意してください。ユーザーがメールを開封した場合、メールキャンペーンはアプリの外部にリーチするための良い方法です。そうでない場合は、ユーザーがアプリをアンインストールするリスクを冒すことなくコンテンツを配信するには、アプリ内メッセージが最適です。

## アプリ起動のコンバージョンイベントを設定する

プッシュキャンペーンに [コンバージョンイベント][11] を割り当てると、キャンペーンの受信後、一定期間のアプリの起動をトラッキングできます。アプリ起動のコンバージョンイベントを設定すると、プッシュキャンペーン後に通常受け取る結果統計とは異なるインサイトが得られます。

プッシュキャンペーンの結果はすべて、メッセージの直接開封と開封(直接 [開封と影響を受けた開封][12]の両方を含む)を分類しますが、コンバージョントラッキングは、直接開封か影響開封かにかかわらず、あらゆるタイプの開封をトラッキングします。

また、「アプリを開く」コンバージョン イベントを使用すると、コンバージョンの期限前(たとえば 3 日間)に発生したアプリの起動をトラッキングできます。これは、ユーザーが影響を受けた開封を登録する必要がある時間が、各ユーザーの過去のエンゲージメント行動に応じて人によって異なる可能性があるという点で、影響を受けた開封とは異なります。

## 関連記事

お探しのものが見つかりませんでしたか?その他のベスト プラクティスの記事もご覧ください。

- [プッシュメッセージと画像形式][1]
- [プライマーアプリ内メッセージのプッシュ][2]
- [中国のAndroidデバイス向けの配信品質][3]
- [送信前に把握する: チャネル][4]

[1]: {{site.baseurl}}/user_guide/message_building_by_channel/push/best_practices/message_format/
[2]: {{site.baseurl}}/user_guide/message_building_by_channel/push/best_practices/push_primer_messages/
[3]: {{site.baseurl}}/user_guide/message_building_by_channel/push/best_practices/chinese_push_deliverability/
[4]: {{site.baseurl}}/help/help_articles/campaigns_and_canvas/know_before_send/

[6]: {{site.baseurl}}/developer_guide/platform_integration_guides/swift/analytics/setting_custom_attributes/
[7]: {{site.baseurl}}/developer_guide/platform_integration_guides/android/analytics/setting_custom_attributes/#setting-custom-attributes
[8]: {{site.baseurl}}/developer_guide/rest_api/user_data/#user-attributes-object-specification
[9]: {{site.baseurl}}/user_guide/message_building_by_channel/email/best_practices/sunset_policies
[10]: {{site.baseurl}}/user_guide/message_building_by_channel/push/users_and_subscriptions#update-push-subscription-state
[11]: {{site.baseurl}}/user_guide/engagement_tools/campaigns/building_campaigns/conversion_events/
[12]: {{site.baseurl}}/user_guide/data_and_analytics/tracking/influenced_opens
[13]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/about/