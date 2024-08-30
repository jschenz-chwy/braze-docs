---
nav_title: Amazon S3
article_title: Amazon S3
alias: /partners/amazon_s3/
description: "このリファレンス記事では、Amazon Web Services が提供するスケーラブルなストレージシステムであるBraze とAmazon S3 の連携について説明します。"
page_type: partner
search_tag: Partner

---

# Amazon S3

> [Amazon S3](https://aws.amazon.com/s3/)は、Amazon Web Servicesが提供する高度にスケーラブルなストレージシステムです。

Braze と Amazon S3 の統合では、[Currents]({{site.baseurl}}/user_guide/data_and_analytics/braze_currents/) を使用して S3 インスタンスにBrazeデータを送信し、他のプラットフォームs、ツール、および場所に接続するまでデータを保存できます。ダッシュボードデータエクスポートを使用して統合することもできます。AWS S3インテグレーションを始めるには、このページの指示に従ってください。

## 前提条件

| 要件 | 説明 |
| ----------- | ----------- |
| Amazon S3 アカウント | Amazon S3 アカウントは、このパートナーシップの前進タグe を考慮する必要があります。 |
| 専用 S3 バケット | Amazon S3 と統合するには、アプリ用の S3 バケットを作成する必要があります。<br><br>すでに S3 バケットがある場合は、権限を制限できるように、Braze専用の新しいバケットを作成することをお勧めします。新しいバケットを作成する方法については、次の手順を参照してください。 |
| Currents | データを Amazon S3 にエクスポートするには、アカウントに [Braze Currents]({{site.baseurl}}/user_guide/data_and_analytics/braze_currents/#access-currents) を設定する必要があります。 |
{: .reset-td-br-1 .reset-td-br-2}

#### 新しい S3 バケットを作成する

アプリのバケットを作成するには、[Amazon S3コンソール](https://console.aws.amazon.com/s3/)を開封し、**Sign in**または**AWS**でアカウントを作成する手順に従います。サインイン後、**Storage & Content Delivery**カテゴリから**S3**を選択します。次の画面で**Create Bucket**を選択します。バケットを作成してリージョンを選択するように求められます。

## 統合

Braze には、[Braze Currents]({{site.baseurl}}/user_guide/data_and_analytics/braze_currents/) と、すべてのダッシュボードデータエクスポート(CSV エクスポート、エンゲージメント レポートなど) に対して、Amazon S3-one で2 つの異なるインテグレーションストラテジがあります。どちらの統合も、2 つの認証/許可方法をサポートしています。

- [AWSシークレットアクセスキー方式](#aws-secret-key-auth-method)
- [AWSロールARNメソッド](#aws-role-arn-auth-method)

## AWSシークレットキー認証方式

この認証メソッドは、シークレットキーとアクセスキー ID を生成し、Brazeがバケットにデータを書き込むためにAWSアカウントのユーザーとして認証できるようにします。

### ステップ1:ユーザーの作成 {#secret-key-1}

アクセスキーID とシークレットアクセスキーを取得するには、[ IAM ユーザーと管理者グループをAWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) に作成する必要があります。

### ステップ2:認証情報を取得する {#secret-key-2}

新しいユーザーを作成した後、**ユーザーセキュリティクレデンシャルを表示**をクリックして、アクセスキーIDとシークレットアクセスキーを表示します。次に、これらの認証情報をどこかにメモするか、**Down 読み込む 認証情報 s**ボタンをクリックします。後でBraze ダッシュボードに入力する必要があります。

![][11]

### ステップ3:ポリシーの作成 {#secret-key-3}

**Policies > Get Started > Create Policy** に移動して、ユーザーの権限を追加します。次に、**Create Your Own Policy**を選択します。これにより、制限された権限が付与されるため、Brazeは指定されたバケットにのみアクセスできます。 

![][12]

{% alert note %}
"Currents"および"Dashboard Data Export"には、さまざまなポリシーが必要です。
{% endalert %}

任意のポリシー名を指定し、**Policy Document**に以下のコードスニペットを入力します。`INSERTBUCKETNAME` は必ずバケット名に置き換えてください。これらの権限がないと、インテグレーションは認証情報チェックに失敗し、作成されません。

{% tabs %}
{% tab Braze Currents %}
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["s3:ListBucket", "s3:GetBucketLocation"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME"]
        },
        {
            "Effect": "Allow",
            "Action": ["s3:PutObject", "s3:GetObject"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME/*"]
        }
    ]
}
```
{% endtab %}
{% tab ダッシュボードデータのエクスポート %}
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["s3:ListBucket", "s3:GetBucketLocation"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME"]
        },
        {
            "Effect": "Allow",
            "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME*", "arn:aws:s3:::INSERTBUCKETNAME/", "arn:aws:s3:::INSERTBUCKETNAME"]
        }
    ]
}
```
{% endtab %}
{% endtabs %}

### ステップ4:ポリシーのアタッチ {#secret-key-4}

新しいポリシーを作成したら、**ユーザー s** に移動し、指定したユーザーを選択します。**Permissions**タブで、**Attach Policy**をクリックし、作成した新しいポリシーを選択します。これで、AWS 認証情報 s をBraze アカウントに関連付ける準備ができました。

![][13]

### ステップ 5: BrazeをAWSに関連付ける {#secret-key-5}

{% tabs %}
{% tab Braze Currents %}

Braze で、**Partner Integrations** > **データエクスポート** に移動します。

{% alert note %}
[古いナビゲーション]({{site.baseurl}}/navigation)を使用している場合は、**Currents**の下に**統合**があります。
{% endalert %}

次に、**Create Current**をクリックし、**Amazon S3 Data Export**を選択します。

現在の名前を指定し、次に**Credentials**セクションで、**AWSシークレットアクセスキー**ラジオボタンが選択されていることを確認し、S3アクセスID、AWSシークレットアクセスキー、およびAWS S3バケット名を指定されたフィールドsに入力します。

![]({{site.baseurl}}/assets/img/currents-s3-example.png)

{% alert warning %}
AWSのアクセスキーID とシークレットアクセスキーを最新の状態に保ちます。コネクターの認証情報が期限切れになると、コネクターはイベントの送信を中止します。これが **48 時間** 以上続く場合、コネクタのイベントはドロップされ、データは永続的に失われます。
{% endalert %}

必要に応じて、次のカスタマイズを追加することもできます。

- **フォルダパス:**デフォルトは`currents` です。このフォルダーが存在しない場合、Braze によって自動的に作成されます。 
- **サーバ側の保管時のAES-256 暗号化:**デフォルトはオフで、`x-amz-server-side-encryption` ヘッダーが含まれます。

**Launch Current**をクリックして続行します。

認証情報が正常に検証されたかどうかを知らせる通知が表示されます。AWS S3がBraze Currents用に設定されます。

{% endtab %}
{% tab ダッシュボードデータのエクスポート %}

Brazeで、**Partner Integrations** > **Technology Partners** に移動し、**Amazon S3** をクリックします。

{% alert note %}
[古いナビゲーション]({{site.baseurl}}/navigation) を使用している場合、**Technology Partners** は**Integrations** にあります。
{% endalert %}

AWS 認証情報ページで、** AWS シークレットアクセスキー** ラジオボタンが選択されていることを確認し、AWS アクセスID、AWS シークレットアクセスキー、およびAWS S3 バケット名を指定されたフィールドs に入力します。シークレットキーを入力するときは、まず**テスト認証情報s**をクリックして認証情報が機能することを確認し、成功したら**保存**をクリックします。

![]({{site.baseurl}}/assets/img/s3_tech_partners.png)

{% alert tip %}
新しい認証情報 s を取得するには、ユーザーに移動し、**Security 認証情報 s** タブの**Create Access Key** をAWS コンソール内でクリックします。
{% endalert %}

認証情報が正常に検証されたかどうかを知らせる通知が表示されます。これで、AWS S3がBrazeの口座に統合されます。

{% endtab %}
{% endtabs %}

## AWS ロールARN 認証メソッド

この認証メソッドは、BrazeのAmazonアカウントがバケットにデータを書き込むために作成したロールのメンバーとして認証できるようにするロールAmazonリソースネーム(ARN)を生成します。

### ステップ1:ポリシーの作成 {#role-arn-1}

まず、アカウント管理者として AWS 管理コンソールにサインインします。AWS コンソールの IAM セクションに移動し、ナビゲーションバーで **Policies** をクリックし、**ポリシーの作成** をクリックします。

![]({{site.baseurl}}/assets/img/create_policy_1_list.png)

{% alert note %}
"Currents"および"Dashboard Data Export"には、さまざまなポリシーが必要です。
{% endalert %}

\[**JSON**] タブを開き、\[**ポリシードキュメント**] セクションに以下のコードスニペットを入力します。`INSERTBUCKETNAME` は必ずバケット名に置き換えてください。終了したら、**Review Policy**をクリックします。

{% tabs %}
{% tab Braze Currents %}

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["s3:ListBucket", "s3:GetBucketLocation"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME"]
        },
        {
            "Effect": "Allow",
            "Action": ["s3:PutObject", "s3:GetObject"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME/*"]
        }
    ]
}
```

{% endtab %}
{% tab ダッシュボードデータのエクスポート %}

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["s3:ListBucket", "s3:GetBucketLocation"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME"]
        },
        {
            "Effect": "Allow",
            "Action": ["s3:PutObject", "s3:GetObject","s3:DeleteObject"],
            "Resource": ["arn:aws:s3:::INSERTBUCKETNAME/*"]
        }
    ]
}
```

{% endtab %}
{% endtabs %}

次に、ポリシーに名前と説明を指定し、**ポリシーの作成**をクリックします。

![]({{site.baseurl}}/assets/img/create_policy_3_name.png)

![]({{site.baseurl}}/assets/img/create_policy_4_created.png)

### ステップ2:ロールの作成 {#role-arn-2}

コンソールの同じIAM セクションで、**Roles > Create Role** をクリックします。

![]({{site.baseurl}}/assets/img/create_role_1_list.png)

Braze アカウントからBraze アカウントID と外部ID を取得します。
- **Currents**:Braze で、**Partner Integrations** > **Data Export** に移動します。次に、**Create Current**をクリックし、**Amazon S3 Data Export**を選択します。ここには、ロールの作成に必要な識別子s があります。
- **ダッシュボードデータエクスポート**:Brazeで、**Partner Integrations** > **Technology Partners** に移動し、**Amazon S3** をクリックします。ここには、ロールの作成に必要な識別子s があります。

{% alert note %}
[古いナビゲーション]({{site.baseurl}}/navigation)を使用している場合、これらのページは別の場所にあります。<br>- **Currents** は**積分**> **Currents** にあります。<br>- **Technology Partners**は**Integrations**の下にあります。
{% endalert %}

AWSコンソールに戻り、信頼できるエンティティセレクタタイプとして**Another AWS Account** を選択します。BrazeのアカウントID を入力し、**外部ID を要求** をオンにして、Brazeの外部ID を入力します。完了したら、**Next**をクリックします。

![S3 の \[ロールの作成] ページ。このページには、ロール名、ロールの説明、信頼できるエンティティ、ポリシー、および権限境界のフィールドがあります。]({{site.baseurl}}/assets/img/create_role_2_another.png)

### ステップ3:ポリシーのアタッチ {#role-arn-3}

次に、以前に作成したポリシーをロールにアタッチします。検索バーでポリシーを検索し、ポリシーの横にチェックマークを付けて添付します。完了したら、**Next**をクリックします。

![ロールの ARN]({{site.baseurl}}/assets/img/create_role_3_attach.png)

ロールに名前と説明を付け、\[**ロールの作成**] をクリックします。

![ロールの ARN]({{site.baseurl}}/assets/img/create_role_4_name.png)

これで、新しく作成したロールがリストに表示されます。

### ステップ4:Braze AWSとの連携 {#role-arn-4}

AWS コンソールで、新しく作成したロールを一覧で見つけます。名前を選択すると、そのロールの詳細が開封されます。

![]({{site.baseurl}}/assets/img/create_role_5_created.png)

ロール概要ページの上部にある**Role ARN**をメモします。

![]({{site.baseurl}}/assets/img/create_role_6_summary.png)

Braze アカウントに戻り、提供されたフィールドにロールARN をコピーします。

{% tabs %}
{% tab Braze Currents %}

Braze で、**Integrations** の**Currents** ページに移動します。次に、**Create Current**をクリックし、**Amazon S3 Data Export**を選択します。

![]({{site.baseurl}}/assets/img/currents-role-arn.png)

現在の名前を入力します。次に、**Credentials** セクションで、**AWS Role ARN** ラジオボタンが選択されていることを確認し、指定されたフィールドs にロールARN とAWS S3 バケットの名前を入力します。

必要に応じて、次のカスタマイズを追加することもできます。

- フォルダパス(デフォルト s to `currents`)
- サーバ側の保管時のAES-256 暗号化(デフォルト s to OFF) - `x-amz-server-side-encryption` ヘッダーを含みます

**Launch Current**をクリックして続行します。

認証情報が正常に検証されたかどうかを知らせる通知が表示されます。AWS S3がBraze Currents用に設定されます。

{% alert important %}
"S3 認証情報が不正な" エラーを受け取った場合、これはAWS でロールを作成した後に統合が早すぎるためである可能性があります。しばらくしてからもう一度やり直してください。
{% endalert %}

{% endtab %}
{% tab ダッシュボードデータのエクスポート %}

Braze で、**Technology Partners** ページの**Integrations** に移動し、**Amazon S3** をクリックします。

![]({{site.baseurl}}/assets/img/data-export-role-arn.png)

** AWS Credentials** ページで、** AWS Role ARN** ラジオボタンが選択されていることを確認し、指定されたフィールドs にロールARN とAWS S3 バケットの名前を入力します。最初に**テスト認証情報s**をクリックして、認証情報が正しく動作することを確認し、成功したら**保存**をクリックします。

{% alert tip %}
新しい認証情報 s を取得するには、ユーザーに移動し、**Security 認証情報 s** タブの**Create Access Key** をAWS コンソール内でクリックします。
{% endalert %}

認証情報が正常に検証されたかどうかを知らせる通知が表示されます。これで、AWS S3がBrazeの口座に統合されます。

{% endtab %}
{% endtabs %}

## 書き出しビヘイビア

クラウドデータストレージソリューションを統合し、API、ダッシュボード レポートs、またはCSV レポートs をエクスポートしようとしているユーザは、次の処理を実行します。

- すべてのAPI エクスポートでは、レスポンスボディにダウン読み込むURL が返されず、データストレージを介して取得する必要があります。
- すべてのダッシュボード レポート s およびCSV レポートs は、ダウン読み込むのためにユーザー s メールに送信され(ストレージ権限は必要ありません)、データストレージにバックアップされます。 

## 複数のコネクタ

S3 バケットに送信する複数の Currents コネクターを作成する場合は、同じ認証情報 s を使用できますが、それぞれに別のフォルダーパスを指定する必要があります。これらは、同じワークスペース内で作成することも、複数のワークスペース内で分割して作成することもできます。また、統合ごとに1 つのポリシーを作成するか、両方の統合をカバーする1 つのポリシーを作成することもできます。 

Currentsとデータエクスポートの両方に同じ S3 バケットを使用する場合は、統合ごとに異なる権限が必要なため、2 つの個別のポリシーを作成する必要があります。


[11]: {% image_buster /assets/img_archive/S3_Credentials.png %}
[12]: {% image_buster /assets/img_archive/S3_CreatePolicy.png %}
[13]: {% image_buster /assets/img_archive/S3_AttachPolicy.png %}