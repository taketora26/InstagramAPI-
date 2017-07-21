## 認証
Instagram APIは、簡単で効果的な認証と認可のためにOAuth 2.0プロトコルを使用します。 OAuth 2.0は従来のスキームよりもはるかに使いやすく、開発者はInstagram APIをすぐに使用することができます。心に留めておくべきことは、APIに対するすべてのリクエストはSSLを使用して行う必要があることです。

##ログイン許可
OAuth 2.0仕様では、ユーザーから要求しているアクセスの範囲を指定できます。承認されたすべてのアプリは、デフォルトで基本アクセス権を持ちますが、公開コンテンツの閲覧、友だちのコメント、コメント、友だちの管理などの拡張アクセスを求める場合は、承認リクエストでこれらの範囲を指定する必要があります。これらの拡張アクセス許可を使用するには、まず審査のためにアプリを送信する必要があります。現在サポートしているスコープは次のとおりです。

- basic  - ユーザーのプロフィール情報とメディアを読む
- public_content  - ユーザーのためにパブリックプロファイル情報とメディアを読み込む
- follower_list  - フォロワーとそれに続くユーザーのリストを読む
- comments - ユーザーのためにコメントを投稿して削除する
- relationships - ユーザーに代わってアカウントをフォローしたりフォロー解除したりする
- likes - あなたのためにメディアを好きにしたり違う

##権限レビュー
Instagramプラットフォームで作成されたすべての新しいアプリはサンドボックスモードで起動します。このモードのアプリは、任意のAPIエンドポイントを使用できますが、限られた数のユーザーとメディアに制限されています。これは、アプリの開発とテストに最適です。

##Sandbox Mode
Instagramプラットフォームで作成されたすべての新しいアプリはサンドボックスモードで起動します。これは、レビューのためにアプリを送信する前にAPIをテストできる完全に機能的な環境です。サンドボックスモードは、Instagramプラットフォームを初めて使用しており、開発、ステージング、およびその他の非稼動環境用に複数のクライアントを必要とするチームだけでなく、APIプラットフォームを探索する開発者にとって理想的です。

アプリを開発してテストするのに役立つように、サンドボックスモードで利用できるユーザーとメディアは、実際のInstagramデータ（Instagramアプリで通常表示されるもの）ですが、次の条件があります。

サンドボックス内のアプリは10ユーザーに制限されています
データは、各ユーザーの10人のユーザーと20人の最新のメディアに制限されています
APIレート(rate limits)の制限を軽減

##Secure Requests
ほとんどのAPI呼び出しではアクセストークンが必要ですが、悪意のある開発者はOAuthクライアントを偽装したり、アクセストークンを盗む可能性があります。彼らはあなたのアプリの代わりにこれらを使ってスパムを送信します。 Instagramにはスパムを検出する自動システムがあり、これらの呼び出しを担当するOAuthクライアントを自動的に無効にします。いくつかの不正行為を制限することで、あなたのアプリの危険性を軽減することができます。このドキュメントでは、アプリを保護するためのいくつかの方法について説明します。

##API Endpoints
#####User Endpoints
```
GET/users/selfGet information about the owner of the access token.
GET/users/user-idGet information about a user.
GET/users/self/media/recentGet the most recent media of the user.
GET/users/user-id/media/recentGet the most recent media of a user.
GET/users/self/media/likedGet the recent media liked by the user.
GET/users/searchSearch for a user by name.
```
##Rate Limits
Instagramプラットフォームのすべてのレート制限は、各アクセストークンと1時間のスライドウィンドウで個別に制御されます。ライブアプリのレート制限は、サンドボックスモードのアプリよりも高くなります。

####Global Rate Limits
特定のエンドポイントに関係なく、1時間のスライドウィンドウでアクセストークンごとにアプリが行うすべてのAPI呼び出しを含め、グローバルレート制限が適用されます。レート制限は無効または不正な形式のリクエストにも適用されます。
Sandbox	500 / hour

####Endpoint-Specific Rate Limits
公開に使用されるエンドポイント（POSTまたはDELETE）には、エンドポイントごとに適用されるレート制限があります。 OAuthクライアントによってこれらのエンドポイントに行われたコールは、上記のグローバルレート制限にもカウントされます。

CLIENT STATUS	ENDPOINT	RATE LIMIT
Sandbox	/media/media-id/likes	30 / hour
Sandbox	/media/media-id/comments	30 / hour
Sandbox	/users/user-id/relationships	30 / hour

##User Subscriptions
ユーザの購読は、あなたのアプリを認証した人がInstagramで新しいメディアを投稿したときに通知を受けたい場合に便利です。このサブスクリプションの実装は、Pubsubhububプロトコルの一部を利用します。

##Embedding
Instagramの投稿を埋め込むことは、Instagramの写真やビデオを記事やウェブサイトで伝えたい記事に簡単に追加することができます。公開プロフィールの写真や動画だけでなく、自分のコンテンツを埋め込むことができます。いつものように、人々はInstagramのコンテンツを所有しています。埋め込まれた投稿はInstagramの元のコンテンツにユーザ名とリンクを表示することによって適切な帰属を与えます。

##Mobile Sharing
あなたのモバイルアプリは、iPhoneとAndroidの両方で異なる方法でInstagramアプリとやりとりすることができます。
