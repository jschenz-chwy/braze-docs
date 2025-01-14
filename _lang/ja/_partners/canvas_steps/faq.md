---
nav_title: よくある質問
article_title: オーディエンス・シンク FAQ
description: "この記事では、Audience Syncに関するよくある質問に対する回答を提供する。"
page_order: 80
Tool:
  - Canvas

---

# よくある質問

> この記事では、Audience Syncに関するよくある質問に対する回答を紹介する。

### Audience Syncパートナー・ダッシュボードにオーディエンスが表示されるまでにどのくらい時間がかかるか？ 

視聴者の入力にかかる時間は、特定のパートナーによって異なる。

すべてのネットワークがBrazeからのリクエストを処理し、ユーザーとのマッチングを試みる。このプロセスには通常6～48時間かかる。

具体的な時間範囲は、各Audience Syncパートナーのドキュメント内の「Troubleshooting」セクションで確認できる。 

### Audience Syncで使用できるファーストパーティデータの種類は？

各パートナーに使用される特定のフィールドは、パートナーの要件によって異なる場合がある。 

例えば、Facebookにオーディエンス・シンクを設定する場合、Eメール、電話、名前、姓など、さまざまなファーストパーティ・フィールドを使うことができるが、Snapchatの場合は、Eメール、電話、モバイル広告主IDのいずれかしか選択できない。 

同期するために選択できるユーザーフィールドは、Brazeの標準属性とモバイル広告IDに相関していることに注意することが重要である。当社のSDKやAPIを経由して、このデータを適切に渡すようにしなければならない。 

### 私のデータが各Audience Syncパートナーに送信するために処理されるとき、何が起こるのか？

Audience Syncの送信先として選択したデータは正規化される。各パートナーは、API要件に基づいてデータの正規化について異なる仕様を持っている可能性があるため、詳細については各パートナー固有のエンドポイントを確認してほしい。

さらに、Brazeは、Audience Syncパートナーとユーザーを同期する前に、すべてのデータをハッシュ化し、すべてのPIIがSHA256を使用してハッシュ化されるようにする。

### オーディエンス・シンクを作成、管理する際に起こりうる最も一般的なエラーとは？

- **無効なトークン**<br>
  - 典型的な原因としては、特定の広告ネットワークにログインするためのパスワードを変更した場合や、認証情報の有効期限が切れた場合などがある。
  - この問題を解決するには、問題の特定のパートナーページに入り、アカウントを切断して再接続するだけでよい。
- **観客数が少なすぎる**<br>
  - このエラーは通常、オーディエンスからユーザーを削除するオーディエンス同期ステップを作成した場合に発生する。視聴者数がゼロに近づくと、ネットワークは視聴者数が少なすぎて配信できないというフラグを立てることができる。 
  - この問題を解決するには、オーディエンスのサイズが完全に枯渇しないように、定期的にユーザーを追加・削除するオーディエンス同期戦略を検討していることを確認すること。
- **観客は存在しない**<br>
  - このエラーは、オーディエンス同期ステップが存在しないオーディエンスを使用しているために発生する。これは、オーディエンスにアクセスするために必要な権限を持っていない場合にも引き起こされる可能性がある。 
  - この問題を解決するには、オーディエンス同期設定内にアクティブなオーディエンスを追加するか、新しいオーディエンスを作成する。
- **広告アカウントへのアクセスを試みる**<br>
  - このエラーは、選択した広告アカウントおよび/またはオーディエンスの権限がない場合に発生する。
  - この問題を解決するには、広告アカウントの管理者と協力して、適切なアクセス権とパーミッションを取得してほしい。 
- **無効な設定**<br>
  - このエラーは、広告アカウント、オーディエンス、またはユーザーフィールドを含む、特定のオーディエンス同期先をキャンバスで設定していない場合に発生する。 
  - この問題を解決するには、起動する前に各パートナーのコンフィギュレーションを完了させる。
- **利用規約**<br>
  - Facebookのような一部のAudience Sync配信先では、Audience Sync機能を使用するために、特定の利用規約に同意することが広告ネットワークによって義務付けられている。このエラーは、あなたが適切な条件を承諾していない場合に発生する。 
  - この問題を解決するには、各パートナーの要求する条件を受け入れていることを確認する。特にフェイス[ブックについては](https://www.braze.com/docs/partners/canvas_steps/facebook_audience_sync/#troubleshooting)、[フェイスブックのトラブルシューティングを](https://www.braze.com/docs/partners/canvas_steps/facebook_audience_sync/#troubleshooting)確認してほしい。 
