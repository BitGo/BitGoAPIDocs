* * *

タイトル： BitGo APIレファレンス

language_tabs: - javascript - shell

toc_footers: - <a href="https://www.bitgo.com/" target="_new">BitGoウェブサイト</a> - <a href="https://www.bitgo.com/terms" target="_new">サービス契約</a> - <a href="https://www.bitgo.com/settings" target="_new">BitGo 設定 (APIアクセストークンを取得)</a> - <a>言語</a> - [- English　英語](index.html) - [- Japanese 日本語](ja/index.html) - [- Chinese (Simplified) 简体中文](zh-CN/index.html)

* * *

# Getting Started はじめに<aside class="info"> 私達の開発者プラットフォームが立ち上がりました。インテグレーション支援やアクセストークン、追加情報にサインアップするには

[BitGoプラットフォームポータル](https://www.bitgo.com/platform)にお越しください。 </aside> 

### Overview 概要

BitGoは、マルチシグ技術を現行のあなたのビットコインアプリケーションやサービスに統合するためのシンプルで堅牢なRESTful API及びシンプルなクライアントサイドjavascriptを提供しています。

BitGo SDKは以下の操作を可能にします：

* P2SH(マルチシグネチャ) ウォレットの作成
* 階層的決定性ウォレット管理(BIP32)
* トランザクションの作成
* トランザクション署名
* 支出制限
* マルチサイナーウォレットフロー

### Multi-Signature Wallets マルチシグネチャウォレット

マルチシグウォレットの主要な利点は、複数のマシンや人々が協働し特定のトランザクションを承認する能力です。 トランザクションのマルチシグネチャがなければ、トランザクションを承認するための全ての証明はマシン上の１人の人間に常に常駐しなければなりません。 その人間またはマシンが攻撃者によって侵入された場合、あなたの持つ全てのビットコインが失われることがあります。

これまで、これらの１人の人間 / シングルマシンのセキュリティを確保するのは非常に困難で、多くのベンダーは単に「コールドストレージ」を用いてビットコインを完全にオフラインに置くことを選んんでいました。

マルチシグウォレットはビットコインをオフラインに置くことなく、あなたがモダンなビットコインアドレスに求める柔軟性を提供します。 BitGo APIは、自分のアプリケーションでマルチシグネチャ機能を利用し、複数ユーザー、連署者、最先端の不正検出サービスが持つ完全な柔軟性を損失や盗難から保護するため活用するのを可能にします

詳細については、<a href="https://www.bitgo.com/p2sh_safe_address" target="_new">BitGoホワイトペーパー</a>をお読みください。

### HD Wallets, HDウォレット

全てのBitGoウォレットは階層的決定性ウォレット（別名HDウォレット）です。 HDウォレットはビットコイン<a href="https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki" target="_new">BIP32規格</a>を用いて実装されています。 よって、BitGo' HDウォレットは個別の鍵というより「キーチェーン」から構築されており、2つの特徴的なセキュリティとプライバシー強化の機能を提供します。

* よりセキュアなバックアップ
    
    キーチェーンは単一のシークレットキーでバックアップされているため、ウォレットは単一のバックアップキーで保全される多数のパブリックキーを用いることが出来ます。

* ブロックチェーンプライバシー
    
    HDウォレットなら、どのトランザクションも同一のウォレットから到着したと見えないよう、アプリケーションはトランザクション毎に新たなキーを作成できます。 これにより、ウォレットの本当の大きさが漏洩することからウォレット保持者が保護されます。

## Software Development Kit ソフトウェア開発キット

BitGo APIは、開発者にマルチシグウォレットの作成、管理、ポリシーの操作、そしてビットコインネットワークとやり取りする方法を提供します。 ただし、ユーザの秘密鍵やトランザクションの署名など、いくつかセンシティブな操作がクライアントサイドで行われる必要があります。

そのような理由から、私達は<a href="https://github.com/BitGo/BitGoJS" target="_new">ソフトウェア開発キット(SDK)</a>を提供の上、利用をお勧めします。このSDKは、これらのクライアントサイドのウォレット機能を実装し、私達のAPIとインターフェースするものです。 実際には、BitGo APIの利用を検討している開発者は、おそらくBitGoクライアントサイドSDKもBitGo RESTサービスも利用するでしょう。

現在、私達のSDKはjavascriptで利用可能で、node.jsまたはブラウザで実行されます。 サポートされていないプログラミング言語を利用している場合、BitGo Expressの設定方法について「BitGo Express REST API」のドキュメンテーションのセクションを参照いただくか、私達にお問い合わせください。

Javascript SDKのインストール (npmを通じ)

`npm install bitgo --save`

Javascript SDKのインストール (Githubを通じ)

* Githubにある<a href="https://github.com/BitGo/BitGoJS" target="_new">オープンソースSDKページ</a>をご覧ください。
* Gitとnodejs/npmをインストール(例に従うことを推奨)
* 次のコマンドを実行することにより、ローカルに私達のレポジトリをクローン `git clone git@github.com:BitGo/BitGoJS.git`
* BitGoJSディレクトリで次によって依存関係をインストール：`npm install`
* 「例」のディレクトリをチェックして、SDKをどのように利用できるかを参照して、次を実行ください

`node auth.js <testusername> <testpassword> 0000000`

### Importing and initializing the library ライブラリのインポートと初期化

```javascript
// パーケージからインポートしている場合
var BitGoJS = require('BitGoJS/src/index.js');
var bitgo = new BitGoJS.BitGo();

// npm install bitgoからインストールしている場合
// var bitgo = require('bitgo');

bitgo.ping({}, function(err, res) {
    // ここでやりたいことをやる
});
```

ライブラリをインポートするには、`src/index.js`ファイルが必要なだけです。 そうしたら `BitGoJS.BitGo()` を実行することによりSDKを初期化できます。

| パラメーター        | 値                          |
| ------------- | -------------------------- |
| useproduction | プロダクションに接続するかどうか。デフォルト値は偽。 |

Javascript SDKはpromiseとコールバックの両方をサポートしています。コールバックを最後の引数として渡した場合、コールバックスタールで返します。さもなければpromiseが返されます。

### Important notes on test environment テスト環境に関する重要な注意

私達のSDKと各例は、ビットコインテストネットと接続されているBitGoテスト環境の初期値になっています。 詳細は[テスト環境](#bitgo-api-endpoints)のセクションを参照してください。

## BitGo API Endpoints, BitGo API エンドポイント

```javascript
var BitGoJS = require('BitGoJS/src/index.js');

var useProduction = false;
var bitgo = new BitGoJS.BitGo(useProduction);

bitgo.ping({}, function(err, res) {
  if (err) {
    console.dir(err);
  } else {
    console.log(JSON.stringify(res, null, 4));
  }
});
```

```shell
TEST_ENDPOINT='https://test.bitgo.com/api/v1'
PROD_ENDPOINT='https://www.bitgo.com/api/v1'

curl "$TEST_ENDPOINT/ping"
```

BitGoは開発とプロダクション向けに2つの個別の環境を利用可能にしています。

すべての応答は、`application/json`コンテンツタイプです

> 応答の例

    {
        "status": "service is ok!",
        "environment": "BitGo Test"
    }
    

### Production Environment プロダクション環境

BitGo プロダクションエンドポイントは立ち上がっており、提携パートナーとwww.bitgo.com にある弊社独自のウェブアプリケーションにより利用されています。

* プロダクションサイト: https://www.bitgo.com/
* プロダクションAPI: https://www.bitgo.com/api/v1

### Test Environment テスト環境

私達の各例とSDKでは、BitGoのテスト環境がデフォルトで利用されています。 BitGoのプロダクション環境とは完全に別個のものであり、データとアカウントにおけるオーバーラップはありません。 <a href="https://test.bitgo.com/" target="_new">test.bitgo.com</a> でアカウントを作成する必要があります。

* BitGo テスト サイト: https://test.bitgo.com/
* テスト環境 API: https://test.bitgo.com/api/v1

テスト環境の場合のみ、（自動テストを目的として）BitGoでの認証においてOTPの代わりに`0000000`が使えます。

この環境は、ナビゲーションに<a href="http://tbtc.blockr.io/" target="_new">Blockr</a>を利用できるビットコインテストネットに接続されています。 テストコインを取得するには、<a href="http://tpfaucet.appspot.com/" target="_new">フォーセット</a>か、あるいはご連絡ください。

## BitGo Express REST API

```shell
./bitgo-express --debug --port 3080 --env test --bind localhost
```

```shell
BITGO_EXPRESS_HOST='localhost'
curl http://$BITGO_EXPRESS_HOST:3080/api/v1/ping
```

BitGo Express REST APIは、BitGoを利用したいがネイティブのBitGo SDKのない言語環境で開発している開発者向けのライトウェイトサービスです。

BitGo Expressはあなたのデータセンターのサービスとして稼働し、BitGoに送信する前の部分的なトランザクションの署名など、あなた自身の鍵を伴うクライアントサイドの操作を処理します。 これにより、あなたの鍵は決してネットワーク外に出ることなく、BitGoの方で表示されることはありません。 BitGo Expressは、標準のBitGo REST APのプロクシとして振舞うことも出来、単一のREST APIを通じBitGoへの統一インターフェースを提供します。

BitGo Expressを利用するには:

* [BitGoJS](#software-development-kit) をインストールします
* bin ディレクトリで次のコマンドを実行します:

`./bitgo-express --debug --port 3080 --env test --bind localhost`

* **全ての**BitGo REST APIの呼び出しを、bitgo-expressを実行しているマシンに対し行う

## Error Handling エラー処理

> JSON エラーの例

```json
{
  "status":  "400 Bad Request",
  "error": "missing user name"
}
```

全てのエラーは一般的なRESTの原則に従います。エラー応答の本文(例えば非200ステータスコード)に含まれるのは次の形式のエラーオブジェクトになります：

| パラメーター | 値                                     |
| ------ | ------------------------------------- |
| status | The HTTP error status returned        |
| error  | The detailed description of the error |

# User Authentication ユーザー認証

BitGoの認証は"Authorization"のヘッダーを通じて行われ、呼び出し元がアクセストークンを指定するのを可能にします。

アクセストークンはセッションを維持するのに利用され、パスワードログイン（ワンタイムパスワード（OTP）が必要）によって作成されます。 典型的なアクセストークンは1時間の間有効で、消費された資金をアンロックするのにOTPを必要とします。

デフォルトで、トークンは単一のIPアドレスに制限され、60分間有効です。それが過ぎたらユーザーは再認証する必要があります。

一部のAPIコールについては、有効なセッショントークンだけでは不十分です。 これらのAPIコールにアクセスするには、セッションはUnlock APIを用いて、追加の2要素コードによって明示的にアンロックされなければなりません。 単一のアンロックコールはユーザーが任意のサイズのトランザクション（ウォレットポリシーの対象）、または内部のBitGoが管理するクォータ以下の、任意の回数のトランザクションを行うことを可能にします。<aside class="info"> アンロックが必要なAPIは、セッションが現在ロックされている場合または現在のアンロックセッションのトランザクションクォータの残りが不十分でない場合、応答にneedsUnlock=trueを含みます。 </aside> 

また、API 用に作成されたアクセス トークンは一定の額まで無期限にロックすることができますが、作成時に特定のスコープにバインドされる必要があります。

## API Access Tokens, APIアクセストークン

```shell
ACCESS_TOKEN='DeveloperAccessToken'
curl -X GET -H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{}' \
-H "Content-Type: application/json" \
https://test.bitgo.com/api/v1/user/session
```

```javascript
var bitgo = new BitGoJS.BitGo({accessToken:'DeveloperAccessToken'});
bitgo.session({}, function callback(err, session) {
  if (err) {
    // handle error
  }
  console.dir(session);
});
```

自動化の目的で、開発者は、1時間で期限が切れない一定額の資金について、アンロックされた長寿命のアクセストークンをリクエストすることができます。

  1. BitGoダッシュボードへアクセスして、「設定」のページへ行く
  2. 「開発者」のタブをクリック
  3. 長寿命のアクセストークンを作成できるようになりました

トークンは、デフォルトでは、あなたが指定した支出制限に基づきロックされていない状態で来ます。アンロックがリセットされるので、再度API経由でトークンをアンロックしようとしないで下さい。

### Token Parameters トークンパラメーター

| パラメーター         | 説明                                                                           |
| -------------- | ---------------------------------------------------------------------------- |
| Label          | 後で無効にすることを選択できるようトークンを特定するために用いられるラベル                                        |
| Duration       | トークンが有効であり続ける秒数                                                              |
| Spending Limit | トークンは、BTC建ての支出制限の額までのアンロックされた状態で来ます。制限がリセットされるので、API経由でトークンをアンロックしようとしないで下さい |
| IP Addresses   | BitGoが特定のIPアドレスからのみ受け付けるよう、トークンをロックダウンします                                    |
| Permissions    | トークンが生成される際の認証の範囲                                                            |

## Current User Profile 現在のユーザープロファイル

```shell
curl -X GET -H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/user/me
```

```javascript
bitgo.me({}, function callback(err, user) {
  if (err) {
    // handle error
  }
  // etc
});
```

現在認証されているユーザーについて情報を得る。

### HTTP Request HTTPリクエスト

`GET /api/v1/user/me`

> ユーザモデルの応答の例

```json
{
  "user": {
    "id": "53532a4b43fd69a42f000005f0a2ed87fd8b020040739beb513524b5",
    "isActive": true,
    "name": {
      "first": "Jane",
        "full": "Jane Doe",
        "last": "Doe"
    },
    "username": "janedoe@bitgo.com"
  }
}
```

### 応答

現在認証されているユーザーのユーザモデルのオブジェクトを返す。

## Login　ログイン　

BitGo APIへのファーストパーティアクセスのためのトークンを取得します。 ファーストパーティアクセスは、自身のBitGoアカウントへアクセスしているユーザーのみ向けです。 別のユーザーの代わりでのBitGo APIへサードパーティアクセスの場合、**パートナー認証**を参照下さい。

```shell
EMAIL="janedoe@bitgo.com"
PASSWORD="mypassword"
HMAC=`echo -n "$PASSWORD" | openssl dgst -sha256 -hmac "$EMAIL" | sed 's/^.* //' `

curl -X POST \
-H "Content-Type: application/json" \
-d "{\"email\": \"$EMAIL\", \"password\": \"$HMAC\", \"otp\": \"0000000\"}" \
https://test.bitgo.com/api/v1/user/login

注意 : 以降のシェルの例では、シェル変数ACCESS_TOKENがアクセストークンを含んでいることを前提とします。
```

```javascript
var useTestnet = false;
var bitgo = new Bitgo(useTestnet);
bitgo.authenticate({ username: user, password: password, otp: otp }, function callback(err, response) {
  if (err) {
    // handle error
  }
  var token = response.token;
  var user = response.user;
  // etc
});
```

### HTTP Request HTTPリクエスト

`POST /api/v1/user/login`

### BODY Parameters BODYパラメーター

| パラメーター     | 種類    | 必須か | 説明                      |
| ---------- | ----- | --- | ----------------------- |
| email      | 文字列   | YES | ユーザのメールアドレス             |
| password   | 文字列   | YES | ユーザのパスワード               |
| otp        | 文字列   | YES | 2要素認証トークン(Authyトークン)。   |
| extensible | ブーリアン | NO  | セッションが１時間よりも延長可能である場合に真 |

> 応答の例

```json
{
    "access_token": "3fe0bf671c943af5ee3b739d2b17ffced7fdde62",
    "expires_in": 3600,
    "token_type": "bearer",
    "user": {
        "id": "53532a4b43fd69a42f000005f0a2ed87fd8b020040739beb513524b5",
        "isActive": true,
        "name": {
            "first": "Jane",
            "full": "Jane Doe",
            "last": "Doe"
        },
        "username": "janedoe@bitgo.com"
    }
}
```

### Response 応答

APIで使用するトークンを返す。

トークンはHTTP"Authorization"ヘッダーにある全てのAPIコールへへ、HTTPヘッダーとして追加されなければなりません。

`Authorization: Bearer <あなたのトークンはここ>`

### Errors　エラー

| 応答               | 説明                    |
| ---------------- | --------------------- |
| 400 Bad Request  | 要求パラメーターが見つからないか正しくない |
| 401 Unauthorized | 認証パラメーターが一致しない        |

## Logout ログアウト

BitGoサービスからのログアウト。

```shell
curl -X GET -H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/user/logout
```

```javascript
bitgo.logout({}, function callback(err) {
  if (err) {
    // handle error
  }
  // the user is now logged out.
});
```

### HTTP Request HTTPリクエスト

`GET /api/v1/user/logout`

### BODY Parameters BODYパラメーター

なし

### Response 応答

なし

## Session Information セッション情報

現在のセッションのアクセストークンに関する情報を取得する

```shell
curl -X GET -H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{}' \
-H "Content-Type: application/json" \
https://test.bitgo.com/api/v1/user/session
```

```javascript
bitgo.session({}, function callback(err, session) {
  if (err) {
    // handle error
  }
  console.dir(session);
});
```

> 応答の例

    { "client": "bitgo",
      "user": "5458141599f715232500000530a94fd2",
      "scope":
       [ "user_manage",
         "openid",
         "profile",
         "wallet_create",
         "wallet_manage_all",
         "wallet_approve_all",
         "wallet_spend_all",
         "wallet_edit_all",
         "wallet_view_all" ],
      "expires": "2015-01-21T02:18:11.534Z",
      "origin": "test.bitgo.com",
      "unlock":
          { "time": "2015-01-21T01:20:25.366Z",
            "expires": "2015-01-21T01:30:25.366Z",
            "txCount": 0,
            "txValue": 0
          }
    }
    

### HTTP Request HTTPリクエスト

`GET /api/v1/user/session`

### Response 応答

| フィールド   | 説明                                             |
| ------- | ---------------------------------------------- |
| client  | ユーザートークンが取得された所のOAuthクライアントID                  |
| user    | BitGo ユーザー ID                                  |
| expires | ログインセッションがその時間まで有効なタイムスタンプ                     |
| scope   | このセッショントークンについて許可されている権限のリスト                   |
| origin  | ブラウザでセッションが開始された場合、トークンが作成されたオリジンホスト名          |
| unlock  | セッションがアンロックされた時に利用可能。トランザクションの回数とアンロックの有効期限を示す |

## Send OTP ワンタイムパスワードを送信

ワンタイムパスワード(第二因子認証) トークンをユーザーに送信します。ログイン/アンロックに使用することができます。

```shell
curl -X POST -H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{}' \
-H "Content-Type: application/json" \
https://test.bitgo.com/api/v1/user/sendotp
```

```javascript
bitgo.sendOTP({forceSMS: true}, function callback(err) {
  if (err) {
    // handle error
  }
  // etc
});
```

### HTTP Request HTTPリクエスト 

`POST /api/v1/user/sendotp`

### BODY Parameters BODYパラメーター

| Name     | Type    | Required | Description                                                         |
| -------- | ------- | -------- | ------------------------------------------------------------------- |
| forceSMS | boolean | NO       | Use SMS to send the OTP to the user, even if they have Authy set up |

### Response

None

## Unlock

Unlock the current session, which is required for certain other sensitive API calls.

```shell
curl -X POST -H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{"otp": "0000000"}' \
-H "Content-Type: application/json" \
https://test.bitgo.com/api/v1/user/unlock
```

```javascript
bitgo.unlock({otp: otp}, function callback(err) {
  if (err) {
    // handle error
  }
  // etc
});
```

### HTTP Request

`POST /api/v1/user/unlock`

### BODY Parameters

| Parameter | Type   | Required | Description                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| otp       | string | YES      | An Authy OTP code for the account                                 |
| duration  | number | NO       | Desired duration of the unlock in seconds (default=600, max=3600) |

### Response

None

### Errors

| Response         | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect                       |
| 401 Unauthorized | The authentication parameters did not match, or OTP code was incorrect |

## Lock

Re-lock the current session.

```shell
curl -X POST -H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/user/lock \
```

```javascript
bitgo.lock({}, function callback(err) {
  // ...
});
```

### HTTP Request

`POST /api/v1/user/lock`

### Response

None

### Errors

| Response         | Description                                 |
| ---------------- | ------------------------------------------- |
| 401 Unauthorized | The authentication parameters did not match |

## Partner Authentication

3rd party applications using the BitGo API use OAuth to authenticate through BitGo. Please contact BitGo for a partner ID and more information.

# Keychains

All BitGo wallets are created using keychains. A keychain is a standard <a href="https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki" target="_new">BIP32</a> extended HD key. Unlike traditional bitcoin keys, which represent a single <a href="http://en.wikipedia.org/wiki/Elliptic_Curve_DSA" target="_new">ECDSA</a> key pair, a keychain can represent many key pairs, all derived from a common private key. This allows the user to retain a single private key, but generate an infinite number of public keys. BitGo uses these extended keys to keep your bitcoin more private and secure.

To make wallet creation simple, BitGo maintains a list of Keychains for each user. Each keychain may be used in any number of BitGo Wallets.

There are two types of keychains:

* Public Keychains
    
    These are comprised of a single BIP32 extended public key (xpub).

* Private Keychains
    
    These are comprised of a single BIP32 extended private key (xprv), which is always stored in encrypted form.

All keychains are identified by their xpub. For convenience, each keychain may have a label.

Before creating your first wallet, you must create keychains to use with that wallet.<aside class="success"> Note that accessing the private keychain (even in encrypted form) always requires 2-factor-authentication. </aside> 

## List Keychains

```shell
curl -X GET -H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/keychain
```

```javascript
var keychains = bitgo.keychains();
keychains.list({}, function callback(err, keychains) {
  if (err) {
    // handle error
  }
  console.dir(keychains);
});
```

Get the list of public keychains for the user<aside class="success"> This API only provides the public keys and never the private data for a keychain. </aside> 

### HTTP Request

`GET /api/v1/keychain`

> Example Keychain Model response

```json
{
    "keychains": [
        {
            "xpub": "xpub661MyMwAqRbcEnCqci8PwkTJgBPbfU54z5rS44bBFtm23hudhYaf73e5Jt8e3JhYYizyFpFcXb4Ahd7wBHmoDFqf2GTGZFaHSC1JyJoURpw",
            "ethAddress": "0xcc59479226f57543ae99ca47630275d9c899bed0"
        },
        {
            "xpub": "xpub661MyMwAqRbcFHjomRhjJybA7rh75j4Cn9Hz4EPG9FUumurJFVbWVgqeDCkCJtnoB5zBHvqsSoi4ArVUKQszzfzuAk9L3LwLZKKQ4RQe8k3",
            "ethAddress": "0x0ddbe738936e911f0db97bdab76cf731f813b378"
        },
        {
            "xpub": "xpub69gHkWcECsrNhcpkPBvvpHq8uX4AwYWcciVZAkvdaKNEj2Cr2mAkUzyQ2X4f9W8fBTt6jp6rvAwUDym4GypkApJZUgKt81vpjyYVs4RDvE8",
            "ethAddress": "0x848ce3d81e017ace770dbc69cf0c752e267a1509",
            "isBitGo": true
        }
    ],
    "start": 0,
    "count": 3,
    "total": 3
}
```

### QUERY Parameters

| Parameter | Type   | Required | Description                                                             |
| --------- | ------ | -------- | ----------------------------------------------------------------------- |
| skip      | number | NO       | The starting index number to list from. Default is 0.                   |
| limit     | number | NO       | Max number of results to return in a single call (default=100, max=500) |

### Response

Returns an array of Keychain Model objects.

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |

## Create Keychain

```shell
Available only as a local method (BitGo Express)

curl -X POST http://$BITGO_EXPRESS_HOST:3080/api/v1/keychain/local
```

```javascript
var keychains = bitgo.keychains();
var keychain = keychains.create();

console.dir(keychain);
```

> Example response

```json
  {
    "xpub": "xpub661MyMwAqRbcFdEUWdhDhgKKQ9tQzLSuqZzVM2AZf9TiijPMD84tPYmemZcQosg63SZit3jGQpCDsbbeXPv7A3aT1phPaBgAWQWKwFUioPR",
    "xprv": "xprv9s21ZrQH143K39A1QcADLYNar83vasj4UM4tYdkx6ovjqw4CfakdqkTAvFrY8BoR8TFTCYShYJ3XjZCicvyuavmYjPLVbmASmHrz144nVCy",
    "ethAddress":"0x49e03847622a266335e2688be6e71a4975fe9615"
  }
```

Local client-side function to create a new keychain.

Optionally, a single parameter, 'seed', may be provided which uses a deterministic seed to create your keychain. The seed should be an array of numbers at least 32 elements long. Calling this function with the same seed will generate the same BIP32 keychain.<aside class="warning"> Creating your keychains is a critical step for safely securing your Bitcoin. When generating new keychains, this API uses a random number generator that adheres to industry standards. If you provide your own seed, you must take extreme caution when creating it. </aside> 

Returns an object containing the xprv and xpub for the new chain. The created keychain is not known to the BitGo service. To use it with the BitGo service, use the Keychains.Add API.

For security reasons, it is highly recommended that you [encrypt](#encrypt) and destroy the original xprv immediately to prevent theft.

## Add Keychain

```shell
curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{"xpub": "xpub661MyMwAqRbcGn6m3YB7CJ2ToyUJYEsBpCc2UDJP9s3hzFif9dKucLotrJBbLgNqojM4q4Sddweka1WG2NvMccYyo3SpnfRrTvMuXUTpHwC"}' \
https://test.bitgo.com/api/v1/keychain
```

```javascript
var data = {
  "xpub": "xpub.....",
  "encryptedXprv": "<encrypted xprv goes here>" // optional, provide it for the user key
};

bitgo.keychains().add(data, function callback(err, keychain) {
  if (err) {
    // handle error
  }
  // etc
});
```

Registers a new keychain for the user. You must supply at least the public key. An encrypted private key may be uploaded, but it will be treated as opaque to the server.

The purpose in providing an encrypted private key with the address is so users will be able to access their keys securely whenever they are connected to BitGo without the burden of storing it. It is highly recommended that you encrypt any private keys stored at the server with a strong password from the user. Encryption must be performed on the client. For convenience, you can use BitGo's [encrypt/decrypt functions](#encrypt), but you can use any encryption you wish.<aside class="warning"> If you provide the encrypted xprv, the security of this keychain is only as good as your encryption. Encryption is your responsibility. </aside> 

### HTTP Request

`POST /api/v1/keychain`

### BODY Parameters

| Parameter     | Type   | Required | Description                                 |
| ------------- | ------ | -------- | ------------------------------------------- |
| xpub          | string | YES      | The BIP32 xpub for this keychain            |
| encryptedXprv | string | NO       | The encrypted, BIP32 xprv for this keychain |

> Example Keychain Model response

```json
{
  "xpub": "xpub661MyMwAqRbcGn6m3YB7CJ2ToyUJYEsBpCc2UDJP9s3hzFif9dKucLotrJBbLgNqojM4q4Sddweka1WG2NvMccYyo3SpnfRrTvMuXUTpHwC",
  "encryptedXprv": "<encrypted data here>",
  "path": "m",
  "ethAddress":"0x49e03847622a266335e2688be6e71a4975fe9615"
}
```

### Response

Returns a Keychain Model object.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |

## Create BitGo Keychain

```shell
curl -X POST \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/keychain/bitgo
```

```javascript
bitgo.keychains().createBitGo({}, function callback(err, keychain) {
  if (err) {
    // handle error
  }
  console.dir(keychain);
});
```

Creates a new keychain on BitGo's servers and returns the public keychain to the caller.

### HTTP Request

`POST /api/v1/keychain/bitgo`

### BODY Parameters

None.

> Example Keychain Model response

```json
{
  "xpub": "xpub661MyMwAqRbcGzVAChrAGb6MYDrAXndUC7h8T7AF8UhfbjS7Au7UKTXmVXaFasQPdfmnUjccreRTMrW7kTmjzwMqVrTHNAFs8M3CXTJpcnL",
  "isBitGo": true,
  "path": "m",
  "ethAddress":"0x49e03847622a266335e2688be6e71a4975fe9615"
}
```

### Response

Returns a Keychain Model object.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |

## Create Backup Keychain

```shell
curl -X POST \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{ \"provider\": \"lastkeysolutions\" }" \
https://test.bitgo.com/api/v1/keychain/backup
```

```javascript
bitgo.keychains().createBackup({ provider: 'lastkeysolutions' }, function callback(err, keychain) {
  console.dir(keychain);
});
```

Creates a new backup keychain on a third party specializing in key recovery services. This keychain will be stored on the third party service and usable for recovery purposes only.

### HTTP Request

`POST /api/v1/keychain/backup`

### BODY Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| provider  | string | YES      | name of the KRS or backup key provider to use |

> Example Keychain Model response

```json
{
  "xpub": "xpub661MyMwAqRbcGzVAChrAGb6MYDrAXndUC7h8T7AF8UhfbjS7Au7UKTXmVXaFasQPdfmnUjccreRTMrW7kTmjzwMqVrTHNAFs8M3CXTJpcnL",
  "ethAddress":"0x49e03847622a266335e2688be6e71a4975fe9615",
  "path": "m"
}
```

### Response

Returns a Keychain Model object.

## Get Keychain

```shell
XPUB=xpub661MyMwAqRbcGn6m3YB7CJ2ToyUJYEsBpCc2UDJP9s3hzFif9dKucLotrJBbLgNqojM4q4Sddweka1WG2NvMccYyo3SpnfRrTvMuXUTpHwC

curl -X POST \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/keychain/$XPUB
```

```javascript
bitgo.keychains().get({xpub: xpub}, function callback(err, keychain) {
  console.dir(keychain);
});
```

Lookup a keychain by xpub<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

### HTTP Request

`POST /api/v1/keychain/:xpub`

### URL Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| xpub      | string | YES      | The BIP32 xpub to lookup |

> Example Keychain Model response

```json
{
  "xpub": "xpub661MyMwAqRbcGn6m3YB7CJ2ToyUJYEsBpCc2UDJP9s3hzFif9dKucLotrJBbLgNqojM4q4Sddweka1WG2NvMccYyo3SpnfRrTvMuXUTpHwC",
  "ethAddress":"0x49e03847622a266335e2688be6e71a4975fe9615",
  "encryptedXprv": "<client-encrypted data here>",
  "path": "m"
}
```

### Response

Returns a Keychain Model object.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |
| 404 Bad Request  | The xpub was not found                                              |

## Update Keychain

```shell
XPUB=xpub661MyMwAqRbcGn6m3YB7CJ2ToyUJYEsBpCc2UDJP9s3hzFif9dKucLotrJBbLgNqojM4q4Sddweka1WG2NvMccYyo3SpnfRrTvMuXUTpHwC

curl -X PUT \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{"encryptedXprv": "my encrypted data"}' \
https://test.bitgo.com/api/v1/keychain/$XPUB
```

```javascript
var params = {
  xpub: xpub,
  encryptedXprv: "<encrypted xprv goes here>"
};
bitgo.keychains().update(params, function callback(err, keychain) {
  // handle error, do something with keychain
  console.dir(keychain);
});
```

Update a keychain. This is used if you wish to store a new version of the xprv (for example, if you changed the password used to encrypt the xprv).<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> <aside class="warning"> If you change the encryptedXprv, the existing value is overwritten. If the new value is incorrect, or you forget the password to the new value, your ability to sign with this keychain will be lost forever. </aside> 

### HTTP Request

`PUT /api/v1/keychain/:xpub`

### BODY Parameters

| Parameter     | Type   | Required | Description                                   |
| ------------- | ------ | -------- | --------------------------------------------- |
| encryptedXprv | string | NO       | A new encrypted, BIP32 xprv for this keychain |

> Example Keychain Model response

```json
{
  "xpub": "xpub661MyMwAqRbcGzVAChrAGb6MYDrAXndUC7h8T7AF8UhfbjS7Au7UKTXmVXaFasQPdfmnUjccreRTMrW7kTmjzwMqVrTHNAFs8M3CXTJpcnL",
  "encryptedXprv": "<encrypted data here>",
  "ethAddress":"0x49e03847622a266335e2688be6e71a4975fe9615",
  "path": "m"
}
```

### Response

Returns a Keychain Model object.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |
| 404 Not Found    | The xpub was not found                                              |

# Wallets

All BitGo Wallets are multi-signature, hierarchical, deterministic wallets. Multi-signature wallets are comprised of *N* keys, and require *M* keys to sign a transaction before the transaction is valid. This is called an *M-of-N* wallet.

BitGo currently supports only 2-of-3 wallets. We use a policy layer to support m-of-n permission models.

To create a wallet, 3 keychains must be provided. The first two keychains are provided by the user; the last must be a BitGo keychain. While BitGo can see the public portion of the first two keys, BitGo never has access to the private portion of these keys and therefore cannot conduct transactions without the user. BitGo's single key is not sufficient to sign transactions, and BitGo will only use this key in accordance with the policies set by the user.

## List Wallets

```shell
curl -X GET \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet
```

```javascript
var wallets = bitgo.wallets();
wallets.list({}, function callback(err, wallets) {
// handle error, do something with wallets
for (id in wallets) {
  var wallet = wallets[id].wallet;
  console.log(JSON.stringify(wallet, null, 4));
}
});
```

Get the list of wallets for the user

### HTTP Request

`GET /api/v1/wallet`

> Example response

```json
{
    "wallets": [
        {
            "admin": {},
            "id": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
            "isActive": true,
            "label": "Example wallet",
            "permissions": "admin,spend,view",
            "private": {
                "keychains": [
                    "... truncated for clarity ..."
                ]
            },
            "spendingAccount": true,
            "type": "safehd"
        },
        {
            "admin": {},
            "id": "2MyWgQ3PMAWPenJnHJUPpVjFDLHhaPkZCz5",
            "isActive": true,
            "label": "My wallet",
            "permissions": "admin,spend,view",
            "private": {
                "keychains": [
                    "... truncated for clarity ..."
                ]
            },
            "spendingAccount": true,
            "type": "safehd"
        }
        ...
    ]
    "start": 0,
    "count": 25,
    "total": 36
}
```

### QUERY Parameters

| Parameter | Type   | Required | Description                                                            |
| --------- | ------ | -------- | ---------------------------------------------------------------------- |
| skip      | number | NO       | The starting index number to list from. Default is 0.                  |
| limit     | number | NO       | Max number of results to return in a single call (default=25, max=250) |

### Response

Returns an array of Wallet Model objects.

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |

## Add Wallet<aside class="warning"> This method is for advanced API users and allows manual creation of keys and specification of user and backup key xPubs. For most scenarios in the SDK, 

[Create Wallet With Keychains](#create-wallet-with-keychains) is the simpler and recommended SDK method to send bitcoins from a wallet. </aside> 

```shell
XPUB_USER=xpub661MyMwAqRbcF8BFQAaLnkkDar6uHQZn9cvzPX5qdfUL42gyts7YeYHZgWvNVjcUDP8BEDMduMBhtKLnVAKaT3sW1g14xnv29w5D3ts8LVd

XPUB_BACKUP=xpub661MyMwAqRbcF3MqMfvo7swQwRzEVRyCPry4rmp346Dv6zR4CNFzrtu4bMpeNazCNNa9p3p5z56svzRcnF9LEm3Ha2zSgq2cLrperm7z4oh

XPUB_BITGO=xpub661MyMwAqRbcG96fDEgYGmjKwdQvxG38rr9gTcqszjdU1s5kjLkTD8pTDxYf4ECrkVbGdpTktuc2BwUR1YKvTdNu5U8mxnDp68QAnvdy1WE

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"label\": \"My Test Wallet\", \"m\": 2, \"n\": 3, \"keychains\": [{\"xpub\": \"$XPUB_USER\"}, {\"xpub\": \"$XPUB_BACKUP\"}, {\"xpub\": \"$XPUB_BITGO\"}] }" \
https://test.bitgo.com/api/v1/wallet
```

```javascript
var data = {
  "label": "My Wallet",
  "m": 2,
  "n": 3,
  "keychains": [
    { "xpub": "xPub of user keychain (may be created with keychains.create)"},
    { "xpub": "xPub of backup keychain (may be created with keychains.create or keychains.createBackup)"},
    { "xpub": "xPub of BitGo keychain (created with keychains.createBitGo)"}
  ]
};
bitgo.wallets().add(data, function callback(err, wallet) {
  if (err) {
    // handle error
  }
  console.dir(wallet);
});
```

This API creates a new wallet for the user. The keychains to use with the new wallet must be registered with BitGo prior to using this API.

BitGo currently only supports 2-of-3 (e.g. m=2 and n=3) wallets. The third keychain, and **only** the third keychain, *must* be a BitGo key. The first keychain is by convention the user key, with it's **encrypted** xpriv is stored on BitGo.

BitGo wallets currently are hard-coded with their root at **m/0/0** across all 3 keychains (however, older legacy wallets may use different key paths). Below the root, the wallet supports two chains of addresses, 0 and 1. The **0-chain** is for external receiving addresses, while the **1-chain** is for internal (change) addresses.

The first receiving address of a wallet is at the BIP32 path **m/0/0/0/0**, which is also the ID used to refer to a wallet in BitGo's system. The first change address of a wallet is at **m/0/0/1/0**. </aside>

### HTTP Request

`POST /api/v1/wallet`

### BODY Parameters

| Parameter                       | Type    | Required | Description                                                                        |
| ------------------------------- | ------- | -------- | ---------------------------------------------------------------------------------- |
| label                           | string  | YES      | A label for this wallet                                                            |
| m                               | number  | YES      | The number of signatures required to redeem (must be 2)                            |
| n                               | number  | YES      | The number of keys in the wallet (must be 3)                                       |
| keychains                       | array   | YES      | An array of **n** keychain xpubs to use with this wallet; last must be a BitGo key |
| enterprise                      | string  | NO       | Enterprise ID to create this wallet under.                                         |
| disableTransactionNotifications | boolean | NO       | Set to true to prevent wallet transaction notifications.                           |

> Example response

```json
{
  "id":"2MsajnHwkzQvggkb6Zbi7kaLMcpeCko4BKB",
  "label":"My Test Wallet",
  "isActive":true,
  "private":{
    "keychains":[
      {
        "xpub":"xpub661MyMwAqRbcF8BFQAaLnkkDar6uHQZn9cvzPX5qdfUL42gyts7YeYHZgWvNVjcUDP8BEDMduMBhtKLnVAKaT3sW1g14xnv29w5D3ts8LVd",
        "path":"/0/0"
      },
      {
        "xpub":"xpub661MyMwAqRbcF3MqMfvo7swQwRzEVRyCPry4rmp346Dv6zR4CNFzrtu4bMpeNazCNNa9p3p5z56svzRcnF9LEm3Ha2zSgq2cLrperm7z4oh",
        "path":"/0/0"
      },
      {
        "xpub":"xpub661MyMwAqRbcG96fDEgYGmjKwdQvxG38rr9gTcqszjdU1s5kjLkTD8pTDxYf4ECrkVbGdpTktuc2BwUR1YKvTdNu5U8mxnDp68QAnvdy1WE",
        "path":"/0/0"
      }
    ]
  },
  "permissions":"admin,spend,view",
  "admin":{},
  "spendingAccount":true,
  "confirmedBalance":0,
  "balance":0,
  "pendingApprovals":[]
}
```

### Response

Returns a Wallet Model object.

### Errors

| Response           | Description                                       |
| ------------------ | ------------------------------------------------- |
| 400 Bad Request    | The request parameters were missing or incorrect. |
| 401 Unauthorized   | The authentication parameters did not match.      |
| 406 Not acceptable | One of the keychains provided was not acceptable. |

## Get Wallet

```shell
WALLET=2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb

curl -X GET \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET
```

```javascript
var wallets = bitgo.wallets();
var data = {
  "type": "bitcoin",
  "id": "2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb",
};
wallets.get(data, function callback(err, wallet) {
  if (err) {
    // handle error
  }
  console.dir(wallet);
});

var wallets = bitgo.wallets();
var data = {
  "type": "bitcoin",
  "id": "2My4uGbvFoBvBtLpADVTTai1w6roxQafJki",
};
wallets.get(data, function callback(err, wallet) {
  if (err) {
    // handle error
  }
  // Use wallet object here
  console.dir(wallet);
  console.dir(wallet.balance());
});

```

Lookup wallet information, returning the wallet model including balances, permissions etc. The ID of a wallet is its first receiving address (/0/0)

### HTTP Request

`GET /api/v1/wallet/:id`

> Example response

```json
{
  "id":"2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb",
  "label":"TestNet Wallet",
  "isActive":true,
  "type":"safehd",
  "private":{
    "keychains":[
      {
        "xpub":"xpub681KE1ZbZM8UNecAGowRFH1vJfxrhzA3fMwKHB5yRs7YGU4FQUPJdbZdTaRuyweAXGDSrJt3AWDuDsoXh4CD4h43GPPzAq2H7Cb18xHGivZ",
        "path":"/101",
        "_id":"536d3cc573c8329a78000016"
      },
      {
        "xpub":"xpub661MyMwAqRbcGZp3eJrpEW1VzomFeDqs2nd9vdZhLFdazv2ekEznRZB2BTPVwrcacYTCvy7mPjxw3F9joMCji4WFgxsgd7J5SxkHuRbUNjx",
        "path":"/102",
        "_id":"536d3cc573c8329a78000015"
      },
      {
        "xpub":"xpub661MyMwAqRbcGR4F5XxMfiHGPycWehJT9XA5KZJg4aBcNZogiWnrDMowki4ZsCtZdWFUtHRxMnhTnm9XMEyxSVV7Am9eG7xUnxL3aoAt88d",
        "path":"/103",
        "_id":"536d3cc573c8329a78000014"
      }
    ]
  },
  "permissions":"admin,spend,view",
  "admin":{
    "policy":[
      {
        "type":"transactionLimit",
        "condition":{
          "currency":"BTC",
          "amount":10
        },
        "action":{
          "type":"getApproval"
        }
      },
      {
        "type":"dailyLimit",
        "condition":{
          "currency":"BTC",
          "amount":50
        },
        "action":{
          "type":"getApproval"
        }
      }
    ],
    "users":[
      {
        "user":"51f4a86305b9442c68000005c4666e34ff33aba3d78a2d3197e7b46b",
        "permissions":"admin,spend,view"
      },
      {
        "user":"53532a4b43fd69a42f000005f0a2ed87fd8b020040739beb513524b5",
        "permissions":"admin,spend,view"
      },
      {
        "user":"52e41fcec1a256b31c00001a5ba4eff09976d7278cbce93fcfeb8b25",
        "permissions":"admin,spend,view"
      },
      {
        "user":"53796821a75b0358c626a4ad16fa7fc4f824cbb58e4a88a5bbc04611",
        "permissions":"spend,view"
      }
    ]
  },
  "spendingAccount":true,
  "confirmedBalance":39564772124,
  "balance":39564772124,
  "pendingApprovals":[ ],
  "canSendInstant": true
}
```

### Response

Returns a Wallet Model object.

| Field            | Description                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| id               | id of the wallet (also the first receiving address)                                                                     |
| label            | the wallet label, as shown in the UI                                                                                    |
| index            | the index of the address within the chain (0, 1, 2, ...)                                                                |
| private          | contains summarised version of keychains                                                                                |
| permissions      | user's permissions on this wallet                                                                                       |
| admin            | policy information on the wallet's administrators                                                                       |
| pendingApprovals | pending transaction approvals on the wallet                                                                             |
| confirmedBalance | the confirmed balance                                                                                                   |
| balance          | the balance, including transactions with 0 confirmations                                                                |
| canSendInstant   | boolean indicating if wallet is eligible to send instant transactions backed by BitGo's guarantee against double spends |

### Errors

| Response           | Description                                        |
| ------------------ | -------------------------------------------------- |
| 400 Bad Request    | The request parameters were missing or incorrect.  |
| 401 Unauthorized   | The authentication parameters did not match.       |
| 404 Not Found      | The wallet was not found                           |
| 406 Not acceptable | One of the keychains provided were not acceptable. |

## Create Wallet With Keychains

```shell
Available only as a local method (BitGo Express)
Advanced users should consider the Create Wallet API.

WALLETPASSPHRASE='newverylongishsecretivepassword'
LABEL='nicenewpurse'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"passphrase\": \"$WALLETPASSPHRASE\", \"label\": \"$LABEL\", \"backupXpub\": \"$BACKUPXPUB\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallets/simplecreate
```

```javascript
var data = {
  "passphrase": password,
  "label": label,
  "backupXpubProvider": "keyternal"
}

bitgo.wallets().createWalletWithKeychains(data, function(err, result) {
  if (err) { console.dir(err); throw new Error("Could not create wallet!"); }
  console.dir(result.wallet.wallet);
  console.log("User keychain encrypted xPrv: " + result.userKeychain.encryptedXprv);
  console.log("Backup keychain xPub: " + result.backupKeychain.xPub);
});
```

This method is available on the client SDK as an easy way to create a wallet. It performs the following:

  1. Creates the user keychain and the backup keychain
  2. Encrypts the user keychain
  3. Uploads the encrypted user and backup keychains to BitGo
  4. Creates the BitGo key on the service
  5. Creates the wallet on BitGo with the 3 public keys above<aside class="warning"> It is **VERY IMPORTANT** to have the user print out / back up their user and backup keys. Failure to do so can result in the loss of funds! </aside> 

### BitGo Instant Wallets

By default, this method will create backup keychains locally. To create a wallet that can be used to send BitGo Instant, use the **backupXpubProvider** parameter to specify a KRS, e.g. "keyternal".

### Method Parameters

| Name                            | Type    | Required | Description                                                                                                                                                                                      |
| ------------------------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| passphrase                      | string  | YES      | The passphrase that will be used to encrypt the user keys of the wallet before sending it to BitGo                                                                                               |
| label                           | string  | YES      | A label for this wallet                                                                                                                                                                          |
| backupXpub                      | string  | NO       | Public key of a backup keychain, created on another device, such that no 2 private keys are ever on the same machine. See also backupXpubProvider as an option to have your key hosted remotely. |
| backupXpubProvider              | string  | NO       | Create a backup xPub on your KRS of choice, e.g. "keyternal". This will make the wallet BitGo Instant compatible.                                                                                |
| enterprise                      | string  | NO       | Enterprise ID to create this wallet under.                                                                                                                                                       |
| disableTransactionNotifications | boolean | NO       | Set to true to prevent wallet transaction notifications..                                                                                                                                        |

> Example response

```json
{ "id": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
 "label": "testwallet",
 "isActive": true,
 "type": "safehd",
 "private": { "keychains": [{}] },
 "permissions": "admin,spend,view",
 "admin": {},
 "spendingAccount": true,
 "confirmedBalance": 0,
 "balance": 0,
 "pendingApprovals": [] }
```

    User keychain encrypted xPrv: {"iv":"v2aVEG5A8VwnI+ewS..."}
    Backup keychain encrypted xPrv: {"iv":"vNOUQpzUmHNPwKt..."}
    

### Response

| Response       | Description                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------- |
| wallet         | the wallet model object                                                                     |
| userKeychain   | the newly created user keychain, which has an encrypted xprv stored on BitGo - back this up |
| backupKeychain | the newly created backup keychain - back this up                                            |

### Errors

| Response           | Description                                        |
| ------------------ | -------------------------------------------------- |
| 400 Bad Request    | The request parameters were missing or incorrect.  |
| 401 Unauthorized   | The authentication parameters did not match.       |
| 406 Not acceptable | One of the keychains provided were not acceptable. |

# Wallet Operations - Basic

Each wallet is comprised of many addresses, and each address can be used to receive Bitcoin. The Wallet API provides helpful interfaces for interacting with a user's wallets.

## Create Address

Creates a new address for an existing wallet. BitGo wallets consist of two independent chains of addresses, designated 0 and 1. The 0-chain is typically used for receiving funds, while the 1-chain is used internally for creating change when spending from a wallet. It is considered best practice to generate a new receiving address for each new incoming transaction, in order to help maximize privacy.

```shell
CHAIN=0
WALLETID="3GonoRKFSzcqJCktZPA2NHDd3jJoH4154o"
curl -X POST -H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/address/$CHAIN
```

```javascript
var id = '2My4uGbvFoBvBtLpADVTTai1w6roxQafJki';
bitgo.wallets().get({ "id": id }, function callback(err, wallet) {
  if (err) {
    throw err;
  }
  wallet.createAddress({ "chain": 0 }, function callback(err, address) {
    console.dir(address);
  });
});
```

### HTTP Request

`POST /api/v1/wallet/:walletId/address/:chain`

### URL Parameters

| Parameter | Type                     | Required | Description          |
| --------- | ------------------------ | -------- | -------------------- |
| walletid  | bitcoin address (string) | YES      | The ID of the wallet |
| chain     | number                   | YES      | 0 or 1               |

> Example response

```json
{
  "address": "3GonoRKFSzcqJCktZPA2NHDd3jJoH4154o",
  "chain": 0,
  "index": 42,
  "path": "/0/42",
  "redeemScript": "522102d83bba35a8022c247b645eed6f81ac41b7c1580de550e7e82c75ad63ee9ac2fd2103aeb681df5ac19e449a872b9e9347f1db5a0394d2ec5caf2a9c143f86e232b0d92103d728ad6758d4784effea04d47baafa216cf474866c2d4dc99b1e8e3eb936e73053ae"
}
```

### Response

Returns a new bitcoin address which is associated with the wallet.

| Field        | Description                                               |
| ------------ | --------------------------------------------------------- |
| address      | The chained address                                       |
| chain        | the chain (0 or 1)                                        |
| index        | the index of the address within the chain (0, 1, 2, ...)  |
| path         | the BIP32 path of the address relative to the wallet root |
| redeemScript | the redeemScript for the address                          |

### Errors

| Response           | Description                                         |
| ------------------ | --------------------------------------------------- |
| 400 Bad Request    | The request parameters were missing or incorrect.   |
| 401 Unauthorized   | The authentication parameters did not match.        |
| 403 Forbidden      | The wallet is not a multi-sig BIP32 (SafeHD) wallet |
| 404 Not Found      | The wallet was not found                            |
| 406 Not acceptable | One of the keychains provided were not acceptable.  |

## Send Coins to Address

```javascript
var destinationAddress = '2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD';
var amountSatoshis = 0.1 * 1e8; // send 0.1 bitcoins
var walletPassphrase = 'incorrect horse battery stable' // replace with wallet passphrase

bitgo.wallets().get({id: walletId}, function(err, wallet) {
  if (err) { console.log("Error getting wallet!"); console.dir(err); return process.exit(-1); }
  console.log("Balance is: " + (wallet.balance() / 1e8).toFixed(4));

  wallet.sendCoins({ address: destinationAddress, amount: amountSatoshis, walletPassphrase: walletPassphrase }, function(err, result) {
    if (err) { console.log("Error sending coins!"); console.dir(err); return process.exit(-1); }

    console.dir(result);
  });
});
```

```shell
Available only as a local method (BitGo Express)
Advanced users should consider the Send Transaction API.

WALLETID='2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa'
DESTINATIONADDRESS='2N9JiEUYgRwKAw6FfnUca54VUeaSYSL9qqG'
AMOUNT=1000000
WALLETPASSPHRASE='password'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"address\": \"$DESTINATIONADDRESS\", \"amount\": $AMOUNT, \"walletPassphrase\": \"$WALLETPASSPHRASE\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/sendcoins
```

Easiest way to send coins from your BitGo wallet to a destination Bitcoin address.<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

This method will perform the following on the client:

* Get the user keychain by polling the wallet on the server for a stored keychain (with encrypted private key)
* Decrypt the user key
* Create the transaction to the destination address, with change sent to a newly created chain address on the wallet (path of /1)
* Sign the transaction with the decrypted user key

It will send the partially signed transaction to BitGo servers for processing, where we will:

* Potentially gather additional approvals from other wallet admins
* Apply the final signature
* Broadcast the transaction to the Bitcoin P2P network

### Parameters

| Name                         | Type    | Required | Description                                                                                                                                                                                                                                                                                                                          |
| ---------------------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| address                      | string  | YES      | Destination bitcoin address                                                                                                                                                                                                                                                                                                          |
| amount                       | number  | YES      | Amount to be sent (in Satoshis), e.g. 0.1 * 1e8 for a tenth of a Bitcoin                                                                                                                                                                                                                                                             |
| walletPassphrase             | string  | YES      | Passphrase for the wallet, used to decrypt the encrypted user key (on client)                                                                                                                                                                                                                                                        |
| fee                          | number  | NO       | Fee (in Satoshis), leave blank for autodetect. Do not specify unless you are sure it is sufficient.                                                                                                                                                                                                                                  |
| message                      | String  | NO       | User-provided string (this does not hit the blockchain)                                                                                                                                                                                                                                                                              |
| feeTxConfirmTarget           | number  | NO       | Calculate fees per kilobyte, targeting transaction confirmation in this number of blocks. Default: 2, Minimum: 2, Maximum: 20.                                                                                                                                                                                                       |
| minConfirms                  | number  | NO       | only choose unspent inputs with a certain number of confirmations. We recommend setting this to 1 and using enforceMinConfirmsForChange.                                                                                                                                                                                             |
| enforceMinConfirms ForChange | boolean | NO       | Defaults to false. Set to true to require a minConfirms (explain in the line above) number of confirmations for unspents originating from the wallet's change addresses. If set to false then the minConfirms will only be enforced for unspents originating from wallets other than this user's wallet (i.e. non-change addresses). |
| sequenceId                   | String  | NO       | A custom user-provided string that can be used to uniquely identify the state of this transaction before and after signing                                                                                                                                                                                                           |
| instant                      | boolean | NO       | set to true to request that the transaction be sent with BitGo's instant guarantee against double-spends (fees may apply).                                                                                                                                                                                                           |
| otp                          | String  | NO       | A 7 digit code used to bypass a policy with the "getOTP" action type. See [Wallet Policy](#wallet-policy) for more details                                                                                                                                                                                                           |

> Example Response

```json
{ "tx": "0100000001a366332472cebceabdd541beb582d5dbaaecccdbec639b0a2d2b...",
  "hash": "b3bd8ac76de2340c1159337acdfcaabf08a9470e8157870bd1d84631a0acc67b",
  "fee": 10000,
  "status": "signed",
  "instant": true,
  "instantId": "56651c4c4e82b975616b58647c77fe1dc"
}
```

> Example Response (pending approval required because of wallet policy)

```json
{
  "error": "exceeds daily limit",
  "pendingApproval": "56050b1368217cde3667fcfe0157556b",
  "triggeredPolicy": "5578defa5e44dbcb20c0caf98b297ad7",
  "status": "pendingApproval"
}
```

### Success Response

| Field   | Description                                                                            |
| ------- | -------------------------------------------------------------------------------------- |
| tx      | hex-encoded form of the signed transaction                                             |
| hash    | the transaction id                                                                     |
| fee     | amount in satoshis sent to the Bitcoin miners as part of this transaction              |
| feeRate | amount in satoshis per kilobyte sent to the Bitcoin miners as part of this transaction |

### Policy/Failure Response

| Field           | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| error           | the message from the policy that triggered this pending approval        |
| pendingApproval | the pending approval id, which will need to be approved by another user |
| otp             | set to true if the policy that fired was a "getOTP" type                |
| triggeredPolicy | id of the policy that triggered this pending approval                   |
| status          | the transaction status                                                  |

## Send Coins to Multiple Addresses

```javascript
var recipients = [];
recipients.push({address: '2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD', amount: 0.1 * 1e8});
recipients.push({address: '2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu', amount: 0.2 * 1e8});

var walletPassphrase = 'incorrect horse battery stable' // replace with wallet passphrase

bitgo.wallets().get({id: walletId}, function(err, wallet) {
  if (err) { console.log("Error getting wallet!"); console.dir(err); return process.exit(-1); }
  console.log("Balance is: " + (wallet.balance() / 1e8).toFixed(4));

  wallet.sendMany({ recipients: recipients, walletPassphrase: walletPassphrase }, function(err, result) {
    if (err) { console.log("Error sending coins!"); console.dir(err); return process.exit(-1); }

    console.dir(result);
  });
});
```

```shell
Available only as a local method (BitGo Express)
Advanced users should consider the Send Transaction API.

WALLETID='2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa'
WALLETPASSPHRASE='watashinobitcoin'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"recipients\": [{ \"address\": \"2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD\", \"amount\": 1500000}, { \"address\": \"2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu\", \"amount\": 2500000 }], \"walletPassphrase\": \"$WALLETPASSPHRASE\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/sendmany
```

Convenience function to send Bitcoin to multiple destination addresses in a single transaction.<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

### Parameters

| Name                         | Type    | Required | Description                                                                                                                              |
| ---------------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| recipients                   | string  | YES      | array of recipient objects and the amount to send to each e.g. [{address: '38BKDNZbPcLogvVbcx2ekJ9E6Vv94DqDqw', amount: 1500}, ..]       |
| message                      | string  | NO       | Notes about the transaction                                                                                                              |
| fee                          | number  | NO       | Fee (in Satoshis), leave blank for autodetect. Do not specify unless you are sure it is sufficient.                                      |
| feeTxConfirmTarget           | number  | NO       | Calculate fees per kilobyte, targeting transaction confirmation in this number of blocks. Default: 2, Minimum: 2, Maximum: 20.           |
| minConfirms                  | number  | NO       | only choose unspent inputs with a certain number of confirmations. We recommend setting this to 1 and using enforceMinConfirmsForChange. |
| enforceMinConfirms ForChange | boolean | NO       | Defaults to false. When constructing a transaction, minConfirms will only be enforced for unspents not originating from the wallet.      |
| sequenceId                   | String  | NO       | A custom user-provided string that can be used to uniquely identify the state of this transaction before and after signing               |
| otp                          | String  | NO       | A 7 digit code used to bypass a policy with the "getOTP" action type. See [Wallet Policy](#wallet-policy) for more details               |

> Example Response

```json
{ "tx": "0100000001c69d05d3897a25c611324a935d0c688669dc416cb8d8e9ebb36e364fa79547c8000..",
  "hash": "27374a97df2cd8a63640dd7a40fce9a1d8dfeec3cf7a8b2fbf1ef609f4a5370d",
  "fee": 10000 }
```

### Response

| Field   | Description                                                                            |
| ------- | -------------------------------------------------------------------------------------- |
| tx      | hex-encoded form of the signed transaction                                             |
| hash    | the transaction id                                                                     |
| fee     | amount in satoshis sent to the Bitcoin miners as part of this transaction              |
| feeRate | amount in satoshis per kilobyte sent to the Bitcoin miners as part of this transaction |

### Policy/Failure Response

| Field           | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| error           | the message from the policy that triggered this pending approval        |
| pendingApproval | the pending approval id, which will need to be approved by another user |
| otp             | set to true if the policy that fired was a "getOTP" type                |
| triggeredPolicy | id of the policy that triggered this pending approval                   |
| status          | the transaction status                                                  |

## List Wallet Transactions

```shell
WALLET=2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/tx
```

```javascript
var walletId = '2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov';
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) { throw err; }
  wallet.transactions({}, function callback(err, transactions) {
    // handle transactions
    console.log(JSON.stringify(transactions, null, 4));
  });
});
```

> Example response

```json
{
  "transactions": [
    {
      "blockhash": "00000000000004b6b832552c96f8c88e6da1a99a8411573b702c455ae0f967a1",
      "confirmations": 4842,
      "date": "2015-01-16T01:12:52.000Z",
      "entries": [
        {
          "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
          "value": -10000
        }
      ],
      "fee": 10000,
      "height": 318528,
      "id": "90669b53a2004e0c4efb918f71f60ecbdc63fbdbad53b5c84ef940f31ddef552",
      "inputs": [
        {
          "previousHash": "c9a55f32d4b63d02a617eea58873227e4f011d0dfc836cb2e9cab531c4db0c4a",
          "previousOutputIndex": 1
        }
      ],
      "outputs": [
        {
          "account": "2MupvpwQQ7dqUizKFookiSzkmxxv8HFgnx6",
          "isMine": true,
          "value": 2000000,
          "chain": 0,
          "chainIndex": 15,
          "vout": 0
        },
        {
          "account": "2Mxy4voQQRi81vm8U7EEhKjgLLHu2njS5ia",
          "isMine": true,
          "value": 7990000,
          "chain": 1,
          "chainIndex": 5,
          "vout": 1
        }
      ],
      "pending": false,
      "instant": true,
      "instantId": "56651c4c4e82b975616b58647c788fad",
      "sequenceId": "CustomID1",
      "comment": "test transaction"
    },
    {
      "blockhash": "000000007e190d73d38f1372cb21562405c7ad88fdc3fe6bcba841228dc354c1",
      "confirmations": 16672,
      "date": "2014-11-06T03:22:58.000Z",
      "entries": [
        {
          "account": "monQzkZiHkBSzLJtAJpiDAEF2fQhvyPdeb",
          "value": -36855030260
        },
        {
          "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
          "value": 81000000
        },
        {
          "account": "n1VbwcNawZ2vBTrza3SxbHDPFg9tY1f9eg",
          "value": 36774030260
        }
      ],
      "fee": 0,
      "height": 306698,
      "id": "16c5e8cb01a453382723730afb6ca4621cad84ea759b8727c046b38a9c41193f",
      "inputs": [
        {
          "previousHash": "c9a55f32d4b63d02a617eea58873227e4f011d0dfc836cb2e9cab531c4db0c4a",
          "previousOutputIndex": 1
        }
      ],
      "outputs": [
        {
          "account": "n1VbwcNawZ2vBTrza3SxbHDPFg9tY1f9eg",
          "value": 36774030260,
          "vout": 0
        },
        {
          "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
          "isMine": true,
          "chain": 0,
          "chainIndex": 28,
          "value": 81000000,
          "vout": 1
        }
      ],
      "pending": false,
      "instant": false,
      "sequenceId": "CustomID2",
      "comment": "test transaction"
    }
  ],
  "start": 0,
  "count": 2,
  "total": 2
}
```

Get transactions for a given wallet, ordered by reverse block height (unconfirmed transactions first).

### HTTP Request

`GET /api/v1/wallet/:walletId/tx`

### URL Parameters

| Parameter | Type                     | Required | Description          |
| --------- | ------------------------ | -------- | -------------------- |
| walletId  | bitcoin address (string) | YES      | The ID of the wallet |

### QUERY Parameters

| Parameter | Type    | Required | Description                                                            |
| --------- | ------- | -------- | ---------------------------------------------------------------------- |
| skip      | number  | NO       | The starting index number to list from. Default is 0.                  |
| limit     | number  | NO       | Max number of results to return in a single call (default=25, max=250) |
| compact   | boolean | NO       | Omit inputs and outputs in the transaction results                     |

### Response

Returns an array of Transaction objects. Each transaction contains summary information about how that transaction affected any wallet or bitcoin address involved in the transaction.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |

## Get Wallet Transaction

```shell
WALLET=2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov
TXID=af867c86000da76df7ddb1054b273ca9e034e8c89d049b5b2795f9f590f67648

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/tx/$TXID
```

```javascript
var walletId = '2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov';
var transactionId = 'af867c86000da76df7ddb1054b273ca9e034e8c89d049b5b2795f9f590f67648';

bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) { throw err; }
  wallet.getTransaction({ "id": transactionId }, function callback(err, transaction) {
    console.log(JSON.stringify(transaction, null, 4));
  });
});
```

> Example response

```json
{
    "blockhash": "000000009249e7d725cc087cb781ade1dbfaf2bd777822948d5fccd4044f8299",
    "confirmations": 16661,
    "date": "2014-11-06T02:22:55.000Z",
    "entries": [
        {
            "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
            "value": 84000000
        },
        {
            "account": "msj42CCGruhRsFrGATiUuh25dtxYtnpbTx",
            "value": -85900000
        },
        {
            "account": "mwahoJcaVuy2TiMtGDZV9PaujFeD9z1a1q",
            "value": 1890000
        }
    ],
    "fee": 10000,
    "height": 306695,
    "hex": "0100000001b6e8b36132d351b3d66b5452d8f4601e2271a7bb52b644397db956a4ffe2a053000000006a4730440220127c4adc1cf985cd884c383e69440ce4d48a0c4fdce6bf9d70faa0ee8092acb80220632cb6c99ded7f261814e602fc8fa8e7fe8cb6a95d45c497846b8624f7d19b3c012103df001c8b58ac42b6cbfc2223b8efaa7e9a1911e529bd2c8b7f90140079034e75ffffffff0200bd01050000000017a914c449a7fafb3b13b2952e064f2c3c58e851bb943087d0d61c00000000001976a914b0379374df5eab8be9a21ee96711712bdb781a9588ac00000000",
    "id": "af867c86000da76df7ddb1054b273ca9e034e8c89d049b5b2795f9f590f67648",
    "outputs": [
        {
            "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
            "isMine": true,
            "chain": 0,
            "chainIndex": 28,
            "value": 84000000,
            "vout": 0
        },
        {
            "account": "mwahoJcaVuy2TiMtGDZV9PaujFeD9z1a1q",
            "value": 1890000,
            "vout": 1
        }
    ],
    "pending": false,
    "instant": true,
    "instantId": "56651c4c4e82b975616b58661bad3ac",
    "sequenceId": "Custom1a",
    "comment": "test transaction"
}
```

Get information about a transaction on a wallet.

### HTTP Request

`GET /api/v1/wallet/:walletId/tx/:txId`

### URL Parameters

| Parameter | Type                      | Required | Description                          |
| --------- | ------------------------- | -------- | ------------------------------------ |
| walletId  | bitcoin address (string)  | YES      | The ID of the wallet                 |
| txId      | transaction hash (string) | YES      | The hash of the transaction to fetch |

### Response

Returns a Transaction object

| Parameter     | Type     | Description                                                                                                                                                  |
| ------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| id            | String   | Hash of the transaction                                                                                                                                      |
| hex           | String   | Raw hex of the transaction                                                                                                                                   |
| date          | DateTime | Date this transaction was first seen                                                                                                                         |
| blockhash     | String   | Hash of the block, if this transaction has been confirmed                                                                                                    |
| height        | Number   | Height of the block this transaction was seen in                                                                                                             |
| confirmations | Number   | Number of blocks this transaction has been part of the blockchain                                                                                            |
| entries       | Array    | Consolidated entries of the transaction, taking into account net inputs/outputs                                                                              |
| outputs       | Array    | Information about outputs of the transaction, including the wallet account, value, vout index, isMine, chain (0 for normal addresses, 1 for change addresss) |
| fee           | Number   | Amount in Satoshis paid to the miners for this transaction                                                                                                   |
| pending       | Boolean  | Set to true if the transaction has not yet been confirmed on the blockchain                                                                                  |
| instant       | Boolean  | Set to true if this transaction was sent using BitGo instant                                                                                                 |
| instantId     | String   | The identifier for the instant transaction to be used to reference / obtain the guarantee from BitGo                                                         |
| sequenceId    | String   | The sequenceId (unique custom data provided when the transaction was sent)                                                                                   |
| comment       | String   | The comment as set on the transaction                                                                                                                        |

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |
| 404 Not Found    | The transaction was not found on the wallet                         |

## List Wallet Addresses

Gets a list of addresses which have been instantiated for a wallet using the New Address API.

```shell
WALLET=2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/addresses
```

```javascript
var id = '2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb';
bitgo.wallets().get({ "id": id }, function(err, wallet) {
  if (err) { console.log(err); process.exit(-1); }
  wallet.addresses({}, function(err, walletAddress) {
    console.dir(walletAddress);
  });
});
```

### HTTP Request

`GET /api/v1/wallet/:walletId/addresses`

### URL Parameters

| Parameter | Type                     | Required | Description          |
| --------- | ------------------------ | -------- | -------------------- |
| walletId  | bitcoin address (string) | YES      | The ID of the wallet |

### QUERY Parameters

| Parameter | Type   | Required | Description                                                  |
| --------- | ------ | -------- | ------------------------------------------------------------ |
| chain     | number | NO       | Optionally restrict to chain 0 or chain 1                    |
| skip      | number | NO       | Skip this number of results                                  |
| limit     | number | NO       | Limit number of results to this number (default=25, max=500) |

> Example response

```json
{
  "addresses":[
    {
      "chain":0,
      "index":0,
      "path":"/0/0",
      "address":"2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb"
    },
    {
      "chain":0,
      "index":1,
      "path":"/0/1",
      "address":"2NCT7eymRYvAoYALNkcBPuftr5scSpBKLuY"
    },
    {
      "chain":0,
      "index":2,
      "path":"/0/2",
      "address":"2NCngTQw49WEHQD7pvDEAc1LzVVPQcoxEU7"
    }
  ],
  "start":0,
  "count":3,
  "total":28,
  "hasMore":true
}
```

### Response

Returns an array of Wallet Address objects.

| Field   | Description                                       |
| ------- | ------------------------------------------------- |
| chain   | Which chain is the address on (0 or 1, currently) |
| index   | BIP32 index on the chain                          |
| path    | BIP32 path from wallet                            |
| address | the bitcoin address                               |

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |

## Get Single Wallet Address

Gets information about an address on a wallet. Can also be used to check if an address exists on a wallet.

```shell
WALLET=2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa
ADDRESS=2NBMGw7K9XiBPfvW3nUQcrANKncmAoLUdDX

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/addresses/$ADDRESS
```

```javascript
var id = '2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa';
var address = '2NBMGw7K9XiBPfvW3nUQcrANKncmAoLUdDX';
bitgo.wallets().get({ "id": id }, function(err, wallet) {
  if (err) { console.log(err); process.exit(-1); }
  wallet.address({ address: address }, function(err, walletAddress) {
    console.dir(walletAddress);
  });
});
```

### HTTP Request

`GET /api/v1/wallet/:walletId/addresses/:address`

### URL Parameters

| Parameter | Type                     | Required | Description                                     |
| --------- | ------------------------ | -------- | ----------------------------------------------- |
| walletId  | bitcoin address (string) | YES      | The ID of the wallet                            |
| address   | bitcoin address (string) | YES      | The address on the wallet to get information of |

> Example response

```json
{
    "address": "2NBMGw7K9XiBPfvW3nUQcrANKncmAoLUdDX",
    "balance": 0,
    "chain": 1,
    "index": 3,
    "path": "/1/3",
    "received": 879990000,
    "redeemScript": "522102d22299c1bc9fb7b2788ba48241bdd51a83740b4eb10c0e95b28f6fa1121877482102f91a6957b2a230e958396152ae70076b7c031f5aa446d87bb071da5eac158a1d2103025d939cf6b546e3e44c92097c534366cc64747c3a227664a7ff70e2ebeab32653ae",
    "sent": 879990000,
    "txCount": 2
}
```

### Response

Returns a wallet address object

| Field        | Description                                                                     |
| ------------ | ------------------------------------------------------------------------------- |
| address      | The bitcoin address being looked up                                             |
| balance      | Current balance (satoshis) in this address                                      |
| chain        | The HD chain used to generate this address (0 for user-generated, 1 for change) |
| index        | The index in the HD chain used to generate this address                         |
| path         | The HD path of the address on the wallet                                        |
| received     | Total amount (satoshis) received on this address                                |
| sent         | Total amount (satoshis) sent on this address                                    |
| txCount      | Total number of transactions on this address                                    |
| redeemScript | The redeemScript that may be used to spend funds from this P2SH address         |

# Wallet Operations - Advanced

These features are available and recommended for advanced developers. Using these APIs will provide expanded (but potentially complex) functionality and greater control of the transaction creation process.

## Get Transaction By Sequence Id

```shell
WALLET=2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa
SEQUENCEID=hello123

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/tx/sequence/$SEQUENCEID
```

```javascript
var walletId = '2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa';
var sequenceId = 'hello123';

bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) { throw err; }
  wallet.getWalletTransactionBySequenceId({ "sequenceId": sequenceId }, function callback(err, transaction) {
    console.log(JSON.stringify(transaction, null, 4));
  });
});
```

> Example response

```json
{
    "transaction": {
        "amount": -106215,
        "createdDate": "2015-09-25T08:39:38.897Z",
        "creator": "5458141599f715232500000530a94fd2",
        "date": "2015-09-25T08:39:39.893Z",
        "fee": 6215,
        "history": [
            {
                "action": "unconfirmed",
                "date": "2015-09-25T08:39:39.893Z"
            },
            {
                "action": "signed",
                "date": "2015-09-25T08:39:38.897Z",
                "user": "5458141599f715232500000530a94fd2"
            },
            {
                "action": "created",
                "date": "2015-09-25T08:39:38.897Z",
                "user": "5458141599f715232500000530a94fd2"
            }
        ],
        "id": "5605084a68217cde3667f2ad4a640875",
        "sequenceId": "hello123",
        "signedDate": "2015-09-25T08:39:38.897Z",
        "size": 367,
        "state": "unconfirmed",
        "transactionId": "50430eeffdd1272ff39d0d3667cbc8e60de0a8ea6bb118e6236e0964389e6d19",
        "walletId": "2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa"
    }
}
```

Get the transaction on a wallet sequence ID that was passed in when sending an outgoing transaction (via sendCoins or sendTransaction). This is useful for tracking an unsigned/unconfirmed transaction via your own unique ID, as Bitcoin transaction IDs are not defined before co-signing and malleable before confirmation.

A pending transaction that has not yet been co-signed by BitGo will still have a sequence id.

### HTTP Request

`GET /api/v1/wallet/:walletId/tx/sequence/:sequenceId`

### URL Parameters

| Parameter  | Type                        | Required | Description                                                 |
| ---------- | --------------------------- | -------- | ----------------------------------------------------------- |
| walletId   | bitcoin address (string)    | YES      | The ID of the wallet                                        |
| sequenceId | custom user-provided string | YES      | The unique id previously sent with an outgoing transaction. |

### Response

Returns a WalletTx object, containing the history and state of the transaction on the Bitcoin network.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |
| 404 Not Found    | The transaction was not found on the wallet                         |

## List Wallet Unspents

```shell
WALLET=2N91XzUxLrSkfDMaRcwQhe9DauhZMhUoxGr

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/unspents
```

```javascript
  var id = '2N91XzUxLrSkfDMaRcwQhe9DauhZMhUoxGr';
  bitgo.wallets().get({ id: id }, function callback(err, wallet) {
    if (err) {
      throw err;
    }
    wallet.unspents({limit: 4.2}, function callback(err, wallet) {
      // handle error, use unspents
      console.dir(wallet);
    });
  });
```

Gets a list of unspent input transactions for a wallet.

In order to create a bitcoin transaction, the creator of the transaction will need to accumulate a set of bitcoin 'inputs' for use in creation of that transaction.

### HTTP Request

`GET /api/v1/wallet/:walletId/unspents`

### URL Parameters

| Parameter | Type                     | Required | Description          |
| --------- | ------------------------ | -------- | -------------------- |
| walletId  | bitcoin address (string) | YES      | The ID of the wallet |

### QUERY Parameters

| Parameter | Type   | Required | Description                                                                                         |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------- |
| target    | number | NO       | The API will attempt to return enough unspents to accumulate to at least this amount (in satoshis). |
| skip      | number | NO       | The starting index number to list from. Default is 0.                                               |
| limit     | number | NO       | Max number of results to return in a single call (default=100, max=250)                             |

> Example response

```json
{
    "count": 2,
    "pendingTransactions": false,
    "start": 0,
    "total": 2,
    "unspents": [
        {
            "address": "2N26EdwtVNQe6P9QkVgLHGhoWtU5W98ohNB",
            "blockHeight": 570611,
            "chainPath": "/1/117",
            "confirmations": 3474,
            "date": "2015-09-18T21:58:11.620Z",
            "isChange": true,
            "redeemScript": "522102f90f2bb90f6572af7bf5c7317ebd48311b417b005352ae71c3c79990fea1f60f2102f817f403092d09abbbb955410d1e50fca4d1ee56e145a29dde01e505558dec43210307527a3928d2711212730ef6585d1a82af80d1fe2979e167b7cc1a397c654ba253ae",
            "script": "a9146105ee32b12a94436f19592e18b135d206e5f46987",
            "tx_hash": "3246b59fcec99c81e5f59522327b632f5c54e4da42ccb512550ed91a3f9b5ce6",
            "tx_output_n": 0,
            "value": 78273186932,
            "wallet": "2N91XzUxLrSkfDMaRcwQhe9DauhZMhUoxGr",
            "instant": false
        },
        {
            "address": "2NCB6qVywiBvWmrpcnFJ4jq8m6oZrFTCKDd",
            "blockHeight": 570611,
            "chainPath": "/1/118",
            "confirmations": 3474,
            "date": "2015-09-18T21:58:23.698Z",
            "isChange": true,
            "redeemScript": "522103e48e6f9e7c2f1011b104f3353973e08c8eb83e5d276bea3d274abd1e458700582103ac2554d01691c694683fc2976183afb25492645bc338deb6425371119c163ec7210212d5e3a68be3b78f266e6247cc34d8ce8bba8d964dbe7da3d1101261ffa07af953ae",
            "script": "a914cfa2c19e759f7a3bc58ed7ae1f72d0125965a81a87",
            "tx_hash": "c005f114cbf443c7c0d2fc82bba6b78d0fd677f131467a6d7b17a67ffacd79b9",
            "tx_output_n": 1,
            "value": 1808807240,
            "wallet": "2N91XzUxLrSkfDMaRcwQhe9DauhZMhUoxGr",
            "instant": true
        }
    ]
}
```

### Response

Returns an array of Unspent Input objects.

| Field         | Description                                                                                                                               |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| tx_hash       | The hash of the unspent input                                                                                                             |
| tx_output_n | The index of the unspent input from *tx_hash*                                                                                             |
| value         | The value, in satoshis of the unspent input                                                                                               |
| script        | Output script hash (in hex format)                                                                                                        |
| redeemScript  | The redeem script                                                                                                                         |
| chainPath     | The BIP32 path of the unspent output relative to the wallet                                                                               |
| confirmations | Number of blocks seen on and after the unspent transaction was included in a block                                                        |
| isChange      | Boolean indicating this is an output from a previous spend originating on this wallet, and may be safe to spend even with 0 confirmations |
| instant       | Boolean indicating if this unspent can be used to create a BitGo Instant transaction guaranteed against double spends                     |

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |

## Consolidate Unspents

```javascript
bitgo.wallets().get({ "id": walletId }, function callback (err, wallet) {
  if (err) { throw err; }
  wallet.consolidateUnspents(
    {
      target: desiredUnspentCount,
      minConfirms: minConfirmCountPerIteration,
      walletPassphrase: passphrase
    },
    function (err, consolidationTransactions) {
      if (err) { throw err; }
      // All the transactions used for consolidation, by reverse block height
      console.dir(consolidationTransactions);
    }
  );
});
```

```shell
Available only as a local method (BitGo Express)
WALLETID="2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa"
WALLETPASSPHRASE="mypassword"

curl -X PUT \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"walletPassphrase\": \"$WALLETPASSPHRASE\", \"target\": 1, \"minConfirms\": 1 }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/consolidateunspents
```

Coalesce the unspents currently held in a wallet to a smaller number. This is an iterative process, largely due to transaction size limits and signing speed. Each iteration requires its own transaction fees.

### Parameters

| Parameter                     | Type     | Required | Description                                                                            |
| ----------------------------- | -------- | -------- | -------------------------------------------------------------------------------------- |
| target                        | number   | NO       | desired number of unspents after running the function                                  |
| maxInputCountPerConsolidation | number   | NO       | maximum number of unspents to be used for each iteration. Defaults to 85.              |
| minConfirms                   | number   | NO       | only choose unspent inputs with a certain number of confirmations                      |
| walletPassphrase              | string   | NO       | Passphrase of the wallet                                                               |
| progressCallback              | function | NO       | Closure to be called after each iteration. It can be used for monitoring the progress. |

## Fan Out Unspents

```javascript
bitgo.wallets().get({ "id": walletId }, function callback (err, wallet) {
  if (err) { throw err; }
  wallet.fanOutUnspents(
    {
      target: desiredUnspentCount,
      minConfirms: minConfirmCount,
      walletPassphrase: passphrase
    },
    function (err, fanoutTransaction) {
      if (err) { throw err; }
      // Transaction used to fan out unspents
      console.dir(fanoutTransaction);
    }
  );
});
```

```shell
Available only as a local method (BitGo Express)
WALLETID="2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa"
WALLETPASSPHRASE="mypassword"

curl -X PUT \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"walletPassphrase\": \"$WALLETPASSPHRASE\", \"target\": 85, \"minConfirms\": 1 }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/fanoutunspents
```

Take all the wallet's unspents (that match the selection criteria, such as minimum confirm count) and spread them into a higher number of unspents.

### Parameters

| Parameter        | Type   | Required | Description                                                       |
| ---------------- | ------ | -------- | ----------------------------------------------------------------- |
| target           | number | YES      | desired number of unspents after running the function             |
| minConfirms      | number | NO       | only choose unspent inputs with a certain number of confirmations |
| walletPassphrase | string | NO       | Passphrase of the wallet                                          |

## Create Transaction

```javascript
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) { throw err; }
  var recipients = [];
  recipients.push({address: address, amount: amountSatoshis});
  wallet.createTransaction(
    {
      recipients: recipients,
      fee: fee
    },
    function(err, transaction) {
      // The transaction with selected unspents, ready to be signed with signTransaction
      console.dir(transaction);
    });
  });
});
```

```shell
Available only as a local method (BitGo Express)
WALLETID='2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"recipients\": [{ \"address\": \"2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD\", \"amount\": 1500000}, { \"address\": \"2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu\", \"amount\": 2500000 }] }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/createtransaction
```<aside class="warning"> This method is for advanced API users. For most scenarios, \[sendCoins\](#send-coins-to-address) is the recommended method to send bitcoins from a wallet. </aside> 

Create a transaction with multiple recipients from a wallet using unspents from addresses on that wallet. This is client-side functionality only in the SDK.

Typically used before signTransaction, which signs a created transaction. Change will be sent to a newly created change address (path of /1) on the wallet.

This is an advanced method that allows you to manually specify the miner fee (could be 0) and decrypted keychain.

**WARNING**: If you provide an insufficient fee, your transaction may not get confirmed and your unspents may be unusable for some time.

> Example Response

```json
{
    "changeAddress": {
        "address": "2MvaEJUHmL6wzsxwqW8Xm8Qac4PTa3zMuvE",
        "path": "/1/65"
    },
    "fee": 20000,
    "transactionHex": "0100000003f6a05b9ab9d7c62cb70a662a4016cbd4740d1d6d35d3c903e3a74fd9f943d...",
    "unspents": [
        {
            "chainPath": "/0/10",
            "redeemScript": "522102f96f3d88ca43810306a7fff2e99da9f052d56a26c73a094237c55b5ec72fead62102981092615521d2d6a44a652631309b5136f589f6c625dbf94efa3edae74ddd2f2103b08e8933fc38a3eb2dc0616f336910b884f9ad80b4a7984f2d9f9e19759ff01553ae"
        },
        ...
    ],
    "walletId": "2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa",
    "walletKeychains": [
        {
            "path": "/0/0",
            "xpub": "xpub661MyMwAqRbcGM9kbvxAfLzr9WwqaVKSqw1dEm16mZmPmigQBrqzVQz314kd9jV68JjpRLCtrRWRpEJqe3FCaN4fuMTZNaDa3uMSjxUaZEj"
        },
        ...
    ]
}
```

### Parameters

| Parameter                    | Type    | Required | Description                                                                                                                              |
| ---------------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| recipients                   | string  | YES      | array of recipient objects and the amount to send to each e.g. [{address: '38BKDNZbPcLogvVbcx2ekJ9E6Vv94DqDqw', amount: 1500}, ..]       |
| fee                          | number  | NO       | The absolute fee in Satoshis to be paid to the Bitcoin miners. Set as 'undefined' for automatic.                                         |
| feeRate                      | number  | NO       | The fee in Satoshis to be paid to the Bitcoin miners PER KB of transaction size. Set as 'undefined' for automatic.                       |
| feeTxConfirmTarget           | number  | NO       | Calculate fees per kilobyte, targeting transaction confirmation in this number of blocks. Default: 2, Minimum: 2, Maximum: 20.           |
| minConfirms                  | number  | NO       | only choose unspent inputs with a certain number of confirmations. We recommend setting this to 1 and using enforceMinConfirmsForChange. |
| enforceMinConfirms ForChange | boolean | NO       | Defaults to false. When constructing a transaction, minConfirms will only be enforced for unspents not originating from the wallet.      |
| minUnspentSize               | number  | NO       | Minimum amount in satoshis for an unspent to be considered usable. Defaults to 5460 (to combat tx dust spam).                            |
| instant                      | boolean | NO       | set to true to request that the transaction be sent with BitGo's instant guarantee against double-spends (fees may apply).               |

## Sign Transaction

```javascript
bitgo.wallets().get({id: walletId}, function(err, wallet) {
  wallet.getEncryptedUserKeychain({}, function(err, keychain) {
    console.log("Got encrypted user keychain");
    // Decrypt the user key with a passphrase
    keychain.xprv = bitgo.decrypt({ password: walletPassphrase, input: keychain.encryptedXprv });
    var transactionHex = "0100000003f6a05b9ab9d7c62cb70a662a4016cbd4740d1d6d35d3c903e3a74fd9f943d09c0000000017a91...";
    // Unspents Can be obtained from the createTransaction method
    var unspents = [
      {
        "chainPath": "/0/10",
        "redeemScript": "522102f96f3d88ca43810306a7fff2e99da9f052d56a26c73a094237c55b5ec72fead6"
      }
    ];

    wallet.signTransaction({
      transactionHex: transactionHex,
      unspents: unspents,
      keychain: keychain
    },
    function (err, transaction) {
      console.dir(transaction);
    });
  });
});
```

```shell
Available only as a local method (BitGo Express)
WALLETID='2NB5G2jmqSswk7C427ZiHuwuAt1GPs5WeGa'

UNSPENTS='[
    {
    "chainPath": "/0/10",
    "redeemScript": "522102f96f3d88ca43810306a7fff2e99da9f052d56a26c73a094237c55b5ec72fead62102981092615521d2d6a44a652631309b5136f589f6c625dbf94efa3edae74ddd2f2103b08e8933fc38a3eb2dc0616f336910b884f9ad80b4a7984f2d9f9e19759ff01553ae"
    },
    {
    "chainPath": "/0/10",
    "redeemScript": "522102f96f3d88ca43810306a7fff2e99da9f052d56a26c73a094237c55b5ec72fead62102981092615521d2d6a44a652631309b5136f589f6c625dbf94efa3edae74ddd2f2103b08e8933fc38a3eb2dc0616f336910b884f9ad80b4a7984f2d9f9e19759ff01553ae"
    },
    {
    "chainPath": "/0/0",
    "redeemScript": "52210362a0c0e532afd7c72111525ef6157a32550b2308a0afbedef01505ab2f8aa5a5210281c598ce3155665a8578e51ab4033def358644dae5f1c4abb9b9f038867804ef21031414644c53953b0557c4a3a6f9f99c0b033583b67e5c02bf0297906f60d2ee9d53ae"
    }
]'

TRANSACTIONHEX=0100000003f6a05b9ab9d7c62cb70a662a4016cbd4740d1d6d35d3c903e3a74fd9f943d09c0000000017a914b02ad680c6ff1d2fa953720940ff8e3ac6eef37987ffffffff15e66c75b266d2077c45a3370fe0da92c0c20ebe400343bf82ac522fcc8fe60a0000000017a914b02ad680c6ff1d2fa953720940ff8e3ac6eef37987ffffffff3f7d55ff4330a5b8da69abf675c18ff558e9cf04479779a998a00d3029cbe4070100000017a914c38fcf303ad53d925df43d21b45be7b8d0895c9387ffffffff0360e316000000000017a9147bd4d50e34791a6373bfa0175bf2cb9796a08f5587a02526000000000017a914fce3c3bbd6e96dd3b54f689f33fcd12c94e9dd5587308527000000000017a91424808ad9ba7f68ae0460ab3c21f5281ead31814f8700000000

KEYCHAIN='{ "xpub": "xpub661MyMwAqRbcGM9kbvxAfLzr9WwqaVKSqw1dEm16mZmPmigQBrqzVQz314kd9jV68JjpRLCtrRWRpEJqe3FCaN4fuMTZNaDa3uMSjxUaZEj",
            "encryptedXprv": "{\"iv\":\"ToxBefI++Jf7FhRUV7MNFA==\",\"v\":1,\"iter\":10000,\"ks\":256,\"ts\":64,\"mode\":\"ccm\",\"adata\":\"\",\"cipher\":\"aes\",\"salt\":\"X3Ibf2DfHd8=\",\"ct\":\"ri+99W0cLUpNCgjksZJVl425OGVrJaNJVllaldTjXumD8oxySjGALoj630Qf5xe0WD0DMJjVIAhMwlPz4g6eG6n8a2ZeKRdvfWMhp7PBEdGGCywPbtLNTfbjrS9kfM9bedncp9QXud7fERfH63vorzEP0rUOZJ0=\"}",
            "path": "m",
            "xprv": "xprv9s21ZrQH143K3s5HVuRAJD47bV7MB2bbUi62SNbVDEEQtvMFeKXjwcfZ9otQ4vZMtHzumu9LyFriSFzcjEXJp6eAL4muKaahwxAaiDPWt4R" }'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"transactionHex\": \"$TRANSACTIONHEX\", \"unspents\": $UNSPENTS, \"keychain\": $KEYCHAIN }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/signtransaction
```<aside class="warning"> This method is for advanced API users. For most scenarios, \[sendCoins\](#send-coins-to-address) is the recommended method to send bitcoins from a wallet. </aside> 

Sign a multi-sig transaction using a created transaction hex, keychain and unspent information (derivation paths and redeem scripts). Typically used with the output from createTransaction.

Can be performed offline.

This is client-side functionality only in the SDK.

> Example Response

```json
{"tx":"0100000003f6a05b9ab9d7c62cb70a662a4016cbd4740d1d6d35d3c903e3a74fd9f943d09c00....."}
```

### Parameters

| Parameter      | Type            | Required | Description                                                              |
| -------------- | --------------- | -------- | ------------------------------------------------------------------------ |
| transactionHex | string          | YES      | The unsigned transaction, in hex string form                             |
| unspents       | array           | YES      | Array of unspents objects, which contain the chainpath and redeemScript. |
| keychain       | keychain object | YES      | The decrypted keychain (object), with available xprv property.           |

## Send Transaction

```shell
TX={raw hex transaction}

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"tx\": \"$TX\", \"otp\": \"0000000\" }" \
https://test.bitgo.com/api/v1/tx/send
```

```javascript
bitgo.wallets().get({id: walletId}, function(err, wallet) {
  console.log("Balance is: " + (wallet.balance() / 1e8).toFixed(4));
  wallet.getEncryptedUserKeychain({}, function(err, keychain) {
    if (err) {
      console.log("Error getting encrypted keychain!");
      console.dir(err);
      return process.exit(-1);
    }
    console.log("Got encrypted user keychain");

    // Decrypt the user key with a passphrase
    keychain.xprv = bitgo.decrypt({ password: walletPassphrase, input: keychain.encryptedXprv });

    // Set recipients
    var recipients = {};
    recipients[destinationAddress] = amountSatoshis;
    console.log("Creating transaction");
    wallet.createTransaction({
      recipients: recipients,
      fee: fee
      },
      function(err, transaction) {
        if (err) {
          console.log("Failed to create transaction!");
          console.dir(err);
          return process.exit(-1);
        }

        console.dir(transaction);
        console.log("Signing transaction");
        wallet.signTransaction({
          transactionHex: transaction.transactionHex,
          unspents: transaction.unspents,
          keychain: keychain
        },
        function (err, transaction) {
          if (err) {
            console.log("Failed to sign transaction!");
            console.dir(err);
            return process.exit(-1);
          }

          console.dir(transaction);
          console.log("Sending transaction");
          wallet.sendTransaction({tx: transaction.tx}, function (err, callback) {
            console.log("Transaction sent: " + callback.tx);
          });
        });
      }
    );
  });
});
```<aside class="warning"> This method is for advanced API users. For most scenarios, \[sendCoins\](#send-coins-to-address) is the recommended method to send bitcoins from a wallet. </aside> 

Send a partially-signed transaction. The server will do one of the following: * reject the transaction * gather additional approvals from other wallet admins * apply the final signature, and submit to the Bitcoin P2P network<aside class="info"> This API requires the session to be unlocked using the Unlock API A single call to the Unlock API allows any single transaction, or multiple transactions up to an internally-set BitGo quota (currently set at 50 BTC). </aside> 

### HTTP Request

`POST /api/v1/tx/send`

> Example Response

```json
{
  "tx": "01000000022cbc51a1d73a7e8ef3feaf30ad51b08b53cf91c672ba73..",
  "hash": "b3bd8ac76de2340c1159337acdfcaabf08a9470e8157870bd1d846..",
  "status": "accepted",
  "instant": true,
  "instantId": "56651c4c4e82b975616b58647c77fe1dc"
}
```

### BODY Parameters

| Parameter  | Type               | Required | Description                                                                                                                |
| ---------- | ------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| tx         | Transaction Object | YES      | The transaction, in hex string form                                                                                        |
| sequenceId | String             | NO       | A custom user-provided string that can be used to uniquely identify the state of this transaction before and after signing |
| message    | String             | NO       | User-provided string (this does not hit the blockchain)                                                                    |
| instant    | boolean            | NO       | set to true to request that the transaction be sent with BitGo's instant guarantee against double-spends (fees may apply). |
| otp        | String             | NO       | A 7 digit code used to bypass a policy with the "getOTP" action type. See [Wallet Policy](#wallet-policy) for more details |

### Response

Returns the sent transaction and its hash in hex-encoded form.

### Errors

| Response             | Description                                                         |
| -------------------- | ------------------------------------------------------------------- |
| 400 Bad Request      | The request parameters were missing or incorrect.                   |
| 401 Unauthorized     | The authentication parameters did not match, or unlock is required. |
| 402 Payment Required | The transaction fee in this request seems too low.                  |

### Policy/Failure Response

| Field           | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| error           | the message from the policy that triggered this pending approval        |
| pendingApproval | the pending approval id, which will need to be approved by another user |
| otp             | set to true if the policy that fired was a "getOTP" type                |
| triggeredPolicy | id of the policy that triggered this pending approval                   |
| status          | the transaction status                                                  |

## Get Instant Guarantee

```shell
INSTANTID=564ea1fa95f4344c6db00773d1277160

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/instant/$INSTANTID
```

```javascript
bitgo.instantGuarantee({ id: '56562ee923ab7f3a28d638085ba6955a' }, function(err, result) {
  if (err) {
      console.log("Error getting guarantee!");
      console.dir(err);
      return process.exit(-1);
  }
  console.dir(result);
});
```

BitGo Instant is built on top of our wallet platform, as a guarantee by BitGo against double spends. As a co-signer on a multi-sig wallet, BitGo will never double-spend an output. We back our promise with a cryptographically signed guarantee on each transaction, enabling receivers to accept funds without the need for any block confirmations.<aside class="info"> Anyone can receive instant transactions. Sending an instant transaction requires an instant-compatible / KRS BitGo wallet. </aside> 

### HTTP Request

`GET /api/v1/instant/<id>`

> Example Response

```json
{
    "amount": 600000,
    "createTime": "2015-11-20T04:30:49.894Z",
    "guarantee": "BitGo Inc. guarantees the transaction with hash 8ba08ef2a745246f309ec4eaff5d7652c4fc01e61eebd9aabc1c58996355acd7 or normalized hash 62f76eb48d60a1c46cb74ce42063bd9c0816ca5e17877738a824525c9794ceaf for the USD value of 0.00600000 BTC at Fri Nov 20 2015 04:30:49 GMT+0000 (UTC) until confirmed to a depth of 6 confirms.",
    "id": "564ea1fa95f4344c6db00773d1277160",
    "normalizedHash": "62f76eb48d60a1c46cb74ce42063bd9c0816ca5e17877738a824525c9794ceaf",
    "signature": "1c2ea42ab4b9afdc069441401a1...",
    "state": "open",
    "transactionId": "8ba08ef2a745246f309ec4eaff5d7652c4fc01e61eebd9aabc1c58996355acd7"
}
```

### Response

Returns the instant guarantee message, including the amount and Transaction ID.

| Parameter      | Type     | Description                                                                                   |
| -------------- | -------- | --------------------------------------------------------------------------------------------- |
| amount         | Number   | Amount in Satoshis of the instant guarantee                                                   |
| createTime     | DateTime | The time at which the transaction was created                                                 |
| guarantee      | String   | The message by BitGo to guarantee the instant transaction                                     |
| id             | String   | The instant guarantee ID on BitGo                                                             |
| transactionId  | String   | The hash of the guaranteed transaction                                                        |
| normalizedHash | String   | The hash of the guaranteed transaction without signatures                                     |
| signature      | String   | Cryptographically signed guarantee, to provide an audit record in cases of a dispute          |
| state          | String   | The state of a transaction as monitored by BitGo (you do not need to take any action on this) |

### Verifying BitGo's Guarantee

BitGo’s guarantee is signed using our corporate signing key, which corresponds to the public Bitcoin address 1BitGo3gxRZ6mQSEH52dvCKSUgVCAH4Rja.

To confirm, verify the guarantee with our signature. Example:

`assert(bitcoin.Message.verify('1BitGo3gxRZ6mQSEH52dvCKSUgVCAH4Rja', signature, guarantee, process.config.bitcoin.network));`

If the signature is valid, you may accept the transaction instantly without the need for any block information. You can save the guarantee & signature locally to provide an audit record in case of a dispute.

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |

## Get Wallet by Address

Given an address, returns the address information (including balances) and wallet the address is associated with. Useful where one has many addresses / wallets, but does not know the wallet an address belongs to.

```shell
ADDRESS=2NBMGw7K9XiBPfvW3nUQcrANKncmAoLUdDX

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/walletaddress/$ADDRESS
```

```javascript
var address = '2NBMGw7K9XiBPfvW3nUQcrANKncmAoLUdDX';
bitgo.getWalletAddress({ address: address }, function(err, result) {
  if (err) { console.log(err); process.exit(-1); }
  console.dir(result);
});
```

### HTTP Request

`GET /api/v1/walletaddress/:address`

### URL Parameters

| Parameter | Type                     | Required | Description                           |
| --------- | ------------------------ | -------- | ------------------------------------- |
| address   | bitcoin address (string) | YES      | The address to look up information on |

> Example response

```json
{
    "address": "3JNUVmyjdLySccafwPSGDfS3dCVEz3Rnpj",
    "balance": 0,
    "chain": 0,
    "index": 7,
    "path": "/0/7",
    "received": 0,
    "redeemScript": "52210306d46ec69fdbf36c0eb1c4fe96ac0417d1375490a6811fdfa6f1dc3c78aad50c2102fb38d7ffb3e354a74b3231eeee6b361c7f3c5ddccac15c7f06e5557f4e6a5318210270467cb619033ef9bcb7cc39d4ce45084418229c0e8aa65821345a222a24a2e453ae",
    "sent": 0,
    "txCount": 0,
    "wallet": "32uFQgny5bgj24nywQPo5cgEUGX6a1PuAp"
}
```

### Response

Returns a wallet address object

| Field        | Description                                                                     |
| ------------ | ------------------------------------------------------------------------------- |
| address      | The bitcoin address being looked up                                             |
| balance      | Current balance (satoshis) in this address                                      |
| chain        | The HD chain used to generate this address (0 for user-generated, 1 for change) |
| index        | The index in the HD chain used to generate this address                         |
| path         | The HD path of the address on the wallet                                        |
| received     | Total amount (satoshis) received on this address                                |
| sent         | Total amount (satoshis) sent on this address                                    |
| txCount      | Total number of transactions on this address                                    |
| redeemScript | The redeemScript that may be used to spend funds from this P2SH address         |
| wallet       | The base address (wallet ID) of the wallet this address is on                   |

## Freeze Wallet

```shell
WALLETID='2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe'
DURATION='4000'
curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"duration\": \"$DURATION\" }" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/freeze
```

```javascript
bitgo.wallets().get({ "id": walletId }, function(err, wallet) {
  bitgo.unlock({ otp: otp }, function(err) {
    if (err) {
      console.dir(err);
      throw new Error("Could not unlock!");
    }
    wallet.freeze({ "duration": 4000 }, function(err, result) {
      console.dir(result);
    });
  });
});
```

> Example Response

```json
{
  "time" : "2016-04-01T03:51:39.779Z",
  "expires" : "2016-04-01T04:58:19.779Z"
}
```

Prevent all spend activity on a wallet. This call is designed to be used in cases of emergency, and prevent spends for a default of 1 hour.

### Parameters

| Parameter | Type   | Required | Description                                                             |
| --------- | ------ | -------- | ----------------------------------------------------------------------- |
| duration  | number | NO       | length of time in seconds to freeze spend activity. Defaults to 1 hour. |

### Response

| Field   | Description                                         |
| ------- | --------------------------------------------------- |
| time    | The date the freeze command was called              |
| expires | The date after which spend activity will be allowed |

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |

# Wallet Sharing

A BitGo wallet may be shared between multiple users. All users on a wallet share the same private key (although each individual user may encrypt it separately).

Security on a shared wallet is enforced by BitGo, which requires that users log in and authenticate before co-signing. Wallet permission levels define what an individual user is able to do on a wallet.

### Wallet Permissions

| Permission | Description                                                             |
| ---------- | ----------------------------------------------------------------------- |
| View       | View transactions on the wallet                                         |
| Spend      | Initiate transactions on the wallet, which are subject to wallet policy |
| Admin      | Change policy and manage users and settings on the wallet               |

## Sharing a wallet

```shell
Available only as a local method (BitGo Express)

WALLETID='2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe'
PASSPHRASE='walletpassphrase'
RECEIVEREMAIL='test@bitgo.com'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"walletPassphrase\": \"$PASSPHRASE\", \"email\": \"$RECEIVEREMAIL\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/wallet/$WALLETID/simpleshare
```

```javascript
bitgo.wallets().get({ "id": walletId }, function(err, wallet) {
    wallet.shareWallet({ email: receiverUser, walletPassphrase: senderPassword, permissions: 'view,spend', skipKeychain: false }, function callback(err, result) {
        console.dir(result);
    });
});
```

> Example Response

```json
{
  "id": "54c594802ebe8510790092958f526f47",
  "walletId": "2MsaMz4tYy5RieZ8qBeW1rhsTpNAXjpofSC",
  "walletLabel": "test2",
  "fromUser": "54c589521758c40e79008e91a7088190",
  "toUser": "5458141599f715232500000530a94fd2",
  "permissions": "view,spend,admin",
  "state": "active",
  "keychain":
   { "xpub": "xpub661MyMwAqRbcFcDmevjhDfSebRy8toH17gJ64a2qAMr5gC4JZRWLUJwPDFeKg6aTMNPa1hYQ3s5kJkDNPMbAw5oQdcjn9N58rYQeUnUozFn",
     "encryptedXprv": "{iv:uoXLSMgfplagVOku...",
     "fromPubKey": "041363164D0521A...",
     "toPubKey": "031D388E7B16FE..",
     "path": "m/999999/26697279/124485569"
   }
}
```<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

Sharing a wallet involves giving another user permission to use the wallet through BitGo.

In order for the receiver to use the wallet, we also need to share the private key with them. Each user on BitGo creates a public-private keypair for this purpose during their signup process.

The BitGo SDK does the following client-side to create a new wallet share:

* Get the receiving user's sharing key (a derived path of the receiver's public key)
* Decrypt the wallet to be shared locally.
* Re-encrypt the wallet against the public key above, so that only the receiver may decrypt it.
* Upload the encrypted keys to the BitGo service, which informs the receiver they have a pending share.

### Parameters

| Parameter        | Type    | Required | Description                                                                                |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------------ |
| email            | string  | YES      | Email of the user to share the wallet with                                                 |
| permissions      | string  | YES      | Comma-separated list of permissions, e.g. view,spend,admin                                 |
| walletPassphrase | string  | NO       | Passphrase on the wallet being shared                                                      |
| skipKeychain     | boolean | NO       | Set to true if sharing a wallet with another user who will obtain the keychain out-of-band |
| disableEmail     | boolean | NO       | Set to true to prevent a notification email sent to the user added                         |

### Response

| Field       | Description                                                                               |
| ----------- | ----------------------------------------------------------------------------------------- |
| id          | The id of the walletShare, used to accept it                                              |
| walletId    | The id of the wallet being shared                                                         |
| walletLabel | Label of the wallet to present to the user                                                |
| fromUser    | BitGo ID of the user sharing the wallet                                                   |
| toUser      | BitGo ID of the user receiving the wallet                                                 |
| permissions | Comma-separated list of permissions that the wallet share will give to the receiving user |
| keychain    | The encrypted keychain for the receiver to decrypt (to obtain the private key)            |

## List Wallet Shares

```shell
curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/walletShare
```

```javascript
  bitgo.wallets().listShares({}, function callback(err, result) {
    if (err) {
      throw err;
    }
    console.dir(result);
  });
```

Gets lists of incoming and outgoing wallet shares for the logged-on account.

### HTTP Request

`GET /api/v1/walletShare`

> Example response

```json
{
   "incoming":
   [
     { "id": "54c58f8d2ebe851079008fe3bbd9e50f",
       "walletId": "2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe",
       "walletLabel": "test1",
       "fromUser": "54c589521758c40e79008e91a7088190",
       "toUser": "5458141599f715232500000530a94fd2",
       "permissions": "view,spend,admin",
       "state": "active" },
     { "id": "54c594802ebe8510790092958f526f47",
       "walletId": "2MsaMz4tYy5RieZ8qBeW1rhsTpNAXjpofSC",
       "walletLabel": "test2",
       "fromUser": "54c589521758c40e79008e91a7088190",
       "toUser": "5458141599f715232500000530a94fd2",
       "permissions": "view,spend,admin",
       "state": "active" }
   ],
   "outgoing": []
}
```

### Response

Each wallet share object returned contains the following fields:

| Field       | Description                                                                               |
| ----------- | ----------------------------------------------------------------------------------------- |
| id          | The id of the walletShare, used to accept it                                              |
| walletId    | The id of the wallet being shared                                                         |
| walletLabel | Label of the wallet to present to the user                                                |
| fromUser    | BitGo ID of the user sharing the wallet                                                   |
| toUser      | BitGo ID of the user receiving the wallet                                                 |
| permissions | Comma-separated list of permissions that the wallet share will give to the receiving user |

## Accept Wallet Share

```shell
Available only as a local method (BitGo Express)

SHAREID='54c594802ebe8510790092958f526f47'
NEWPASSPHRASE='receiverpassphrase'
PASSWORD='sharingkeypassword'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"newWalletPassphrase\": \"$NEWPASSPHRASE\", \"userPassword\": \"$PASSWORD\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/walletshare/$SHAREID/acceptShare
```

```javascript
bitgo.wallets().acceptShare(
    { walletShareId: shareId,
      newWalletPassphrase: 'receiverpassphrase',
      userPassword: 'receiverpassword'
    }, function(err, result) {
        console.dir(result);
    }
)
```

> Example Response

```json
{ "state": "accepted", "changed": "true" }
```<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

Client-side operation to accept a wallet share. Performs the following steps:

* Get the incoming wallet share, including the encrypted private keychain.
* Using the user's sharing private key and the wallet share xPub, derive the key to decrypt the private keychain.
* Re-encrypt the wallet with the user's chosen passphrase for future use.
* Upload the encrypted keys to the BitGo service and sets the share to accepted, giving the user access to the wallet on BitGo.

### Parameters

| Parameter             | Type                     | Required | Description                                                                                    |
| --------------------- | ------------------------ | -------- | ---------------------------------------------------------------------------------------------- |
| walletShareId         | bitcoin address (string) | YES      | The incoming wallet share ID to accept                                                         |
| newWalletPassphrase   | string                   | NO       | the passphrase to set on the wallet, for use during future spends                              |
| userPassword          | string                   | NO       | the user's password to decrypt the shared private key                                          |
| overrideEncryptedXprv | string                   | NO       | Set to an alternate encrypted xprv if you wish to store an encrypted xprv received out-of-band |

### Response

| Field   | Description                                     |
| ------- | ----------------------------------------------- |
| state   | end state of walletShare (should be 'accepted') |
| changed | true if successful                              |

## Cancel Wallet Share

```shell
SHAREID='54c594802ebe8510790092958f526f47'

curl -X DELETE \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/walletshare/$SHAREID
```

```javascript
bitgo.wallets().cancelShare({ walletShareId: cancelledWalletShareId }, function(err, result) {
    if (err) { throw err; }
    console.dir(result);
)
```

> Example Response

```json
{ "state": "canceled", "changed": "true" }
```

Can be used to cancel a pending outgoing wallet share, or reject an incoming share. The share should not have been accepted yet.

### HTTP Request

`DELETE/api/v1/walletshare/:SHAREID`

### Response

| Field   | Description              |
| ------- | ------------------------ |
| state   | new state of walletShare |
| changed | true if successful       |

## Remove Wallet User

After a user has accepted a wallet share, they become a party on a wallet and the wallet share is considered "complete".

In order to revoke the share after they have accepted, you can remove the user from the wallet.

This may require approval by another wallet administrator if there is more than a single administrator on a wallet.

```javascript
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) {
    throw err;
  }

  wallet.removeUser({ "user": userId }, function callback(err, wallet) {
    if (err) {
      // handle error
    }
    // etc
  });
});
```

```shell
WALLETID=2MvPJ8GMoFFWdUTh6p42FJV6XpmJEVjxv92
USERID=5479ac2bfb1d3e5e71000005d4dc49e3
curl -X DELETE \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/user/$USERID
```

### HTTP Request

`DELETE /api/v1/wallet/:wallet/user/:userId`

### URL Parameters

| Parameter | Required | Description                                                           |
| --------- | -------- | --------------------------------------------------------------------- |
| wallet    | YES      | The ID of the wallet                                                  |
| userId    | YES      | The user id of the user to remove (can be found on the wallet object) |

### Response

Returns the updated Wallet Model for this wallet, or a Pending Approval object (if approval is required).

### Errors

| Response         | Description                                                         |
| ---------------- | ------------------------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect.                   |
| 401 Unauthorized | The authentication parameters did not match, or unlock is required. |

# Wallet Policy

BitGo wallets feature advanced security features such as multi-user or 2FA approval of transactions and spending limits. To take advantage of this, a user/developer may add and modify policy rules on a wallet. Rules will triggered an associated action (set by the user). The policy engine will collect all triggered rule results, and perform any triggered actions in the order of deny, get approval (from another user), get OTP (sent via SMS to another user) or allow (the default).

If a wallet carries a balance and there are more than two "admin" users associated with a Wallet, any policy change will require approval by another administrator before it will take effect (if there are no additional "admin" users, this will not be necessary). It is thus highly recommended to create wallets with at least 2 administrators by [performing a wallet share](#wallet-sharing). This way, policy can be effective even if a single user is compromised.

For policies with the "getOTP" action type, successfully sending a transaction will require a 7 digit OTP code before the transaction is signed and sent. This policy effectively lets you offer a 2FA security option for your own service, without implementing it yourself. The first attempt to send a transaction will fail and send out the code to the phone specified on the policy. Once you acquire the code from the user, make another send transaction call with the otp code included as a parameter in the API call and the transaction will successfully send. See the "otp" parameter at [Sends Coins to Address](#send-coins-to-address) for further details.

This documentation provides API and SDK coverage of basic BitGo policy involving a single wallet. Further custom policy may be implemented using the [webhook policy type](#set-policy-rule), which causes BitGo to call out to a URL endpoint capable of evaluating any custom policy behavior involving external state.

Advanced policy involving multiple wallets may be implemented by contacting BitGo directly.

## Get Policy

```shell
WALLET=2NEe9QhKPB2gnQLB3hffMuDcoFKZFjHYJYx

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/policy
```

```javascript
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) {
    throw err;
  }

  wallet.getPolicy({}, function callback(err, policy) {
    if (err) {
      // handle error
    }
    console.dir(policy);
  });
});
```

> Example response

```json
{
    "date": "2015-04-29T19:03:37.189Z",
    "id": "55380d34d0d3b00364a52285f09e23a4",
    "rules": [
        {
            "action": {
                "type": "getApproval"
            },
            "condition": {
                "amount": 200000000
            },
            "id": "com.bitgo.limit.day",
            "type": "dailyLimit"
        },
        {
            "action": {
                "type": "getOTP"
                "actionParams": {
                    "otpType": "sms",
                    "phone": "+15417543010",
                    "duration": "3600"
                }
            },
            "condition": {
                "amount": 200000000
            },
            "id": "com.bitgo.limit.tx",
            "type": "transactionLimit"
        }
    ],
    "version": 4
}
```

Gets the policy rules in operation on a wallet.

### HTTP Request

`GET /api/v1/wallet/:walletId/policy`

### URL Parameters

| Parameter | Type                     | Required | Description          |
| --------- | ------------------------ | -------- | -------------------- |
| walletId  | bitcoin address (string) | YES      | The ID of the wallet |

### Response

Returns a Wallet Policy object, containing the rules set up on it

## Get Policy Status

```shell
WALLET=2NEe9QhKPB2gnQLB3hffMuDcoFKZFjHYJYx

curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLET/policy/status
```

```javascript
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) {
    throw err;
  }

  wallet.getPolicyStatus({}, function callback(err, policy) {
    if (err) {
      // handle error
    }
    console.dir(policy);
  });
});
```

> Example response

```json
{
    "statusResults": [
        {
            "policy": "55380d34d0d3b00364a52285f09e23a4",
            "ruleId": "com.bitgo.limit.day",
            "status": {
                "remaining": 94982419
            }
        },
        {
            "policy": "55380d34d0d3b00364a52285f09e23a4",
            "ruleId": "com.bitgo.limit.tx",
            "status": {
                "remaining": 200000000
            }
        }
    ]
}
```

### HTTP Request

`GET /api/v1/wallet/:walletId/policy/status`

### URL Parameters

| Parameter | Type                     | Required | Description          |
| --------- | ------------------------ | -------- | -------------------- |
| walletId  | bitcoin address (string) | YES      | The ID of the wallet |

### Response

Returns status results as generated by rules active on the wallet policy, including information about the limit usage.

## Set Policy Rule

Set the policy on a wallet. A wallet policy controls the conditions under which BitGo will use its single key to sign a transaction.

```javascript
var walletId = '2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2';
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) { throw err; }
  // Sets the policy
  var rule = {
    id: "test1",
    type: "dailyLimit"
    action: { type: "getApproval" },
    condition: { "amount": 101*1e8 },
  };
  wallet.setPolicyRule(rule, function callback(err, wallet) {
    if (err) { throw err; }
    console.dir(wallet.admin.policy);
  });
});
```

```shell
curl -X PUT -H "Content-Type: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" \
-d '{ "action" : { "type" : "getApproval" }, "condition": { "amount" : 100000000 }, "id": "com.bitgo.limit.tx", "type": "transactionLimit", "default":true}' \
https://test.bitgo.com/api/v1/wallet/$WALLETID/policy/rule
```

### HTTP Request

`PUT /api/v1/wallet/:wallet/policy/rule`<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

### BODY Parameters

Policy rule objects have a type, a condition, and an action. The type of policy often dictates the condition values.

| Parameter | Type                                           | Required                                           | Example |
| --------- | ---------------------------------------------- | -------------------------------------------------- | ------- |
| id        | the id of the policy                           | "com.bitgo.limit.tx", "custom1", "anyUniqueRuleId" |         |
| type      | The type of policy                             | *See Policy Types*                                 |         |
| condition | The condition for this policy                  | *See Policy Types*                                 |         |
| action    | The action to take when the condition is false | *See Policy Action Object*                         |         |

> Example response

```json
{
  "date": "2015-10-29T23:33:20.166Z",
  "id": "5631d2f48cad5fab2c6abe16682372bb",
  "rules": [
    {
      "action": {
        "type": "getApproval"
      },
      "condition": {
        "amount": 100000000
      },
      "id": "com.bitgo.limit.tx",
      "type": "transactionLimit"
    },
    {
      "action": {
        "type": "getApproval"
      },
      "condition": {
        "addresses": [
          "muDjD4YdVzi1vyqKtYVbAkLi2J84GkKj5h"
        ]
      },
      "id": "com.bitgo.whitelist.address",
      "type": "bitcoinAddressWhitelist"
    }
  ],
  "version": 4
}
```

### Response

Returns the updated Wallet Model object.

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |

### Policy Object

The aggregate Wallet Policy is an array of Policy Rule Objects. Policy rule objects have a type, a condition, and an action.

| Field     | Description                                    | Possible Values                                                        |
| --------- | ---------------------------------------------- | ---------------------------------------------------------------------- |
| id        | the id of the policy                           | "com.bitgo.limit.tx", "custom1", "anyUniqueRuleId"                     |
| type      | The type of policy                             | "transactionLimit", "dailyLimit", "bitcoinAddressWhitelist", "webhook" |
| condition | The condition for this policy                  | *Depends on policy rule type used*                                     |
| action    | The action to take when the condition is false | *See policy action object*                                             |

### Policy Type - dailyLimit

A dailyLimit policy rule will trigger when the amount of Bitcoin within the last rolling 24 hours exceeds the specified amount.

Conditions for this include:

| Field  | Description                                           | Possible Values    |
| ------ | ----------------------------------------------------- | ------------------ |
| amount | The maximum allowed value of all transactions per day | Number of satoshis |

### Policy Type - transactionLimit

A transactionLimit policy rule will trigger when a single transaction exceeds the specified amount.

Conditions for this include:

| Field  | Description                                                 | Possible Values    |
| ------ | ----------------------------------------------------------- | ------------------ |
| amount | The maximum allowed value of each transaction on the wallet | Number of satoshis |

### Policy Type - bitcoinAddressWhitelist

When active, a Bitcoin address whitelist rule will be triggered whenever any destination Bitcoin address (non-change) of an outgoing transaction is not in the white list.

Conditions for this include:

| Field  | Description                                      | Possible Values |
| ------ | ------------------------------------------------ | --------------- |
| add    | The bitcoin address to add to the whitelist      | Bitcoin address |
| remove | The bitcoin address to remove from the whitelist | Bitcoin address |

> Example webhook callback (sent to your server, any non-200 response will trigger the policy rule action)

```json
{
    "approvalCount": 0,
    "outputs": [
        {
            "outputAddress": "2N9kNR8iS46WuwekvQVaTUa74w2fbvAXHQn",
            "outputWallet": "2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2",
            "value": 24885327
        },
        {
            "outputAddress": "2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe",
            "outputWallet": "2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe",
            "value": 10000000
        }
    ],
    "ruleId": "webhookRule1",
    "spendAmount": 10009060,
    "type": "webhook",
    "unsignedRawTx": "0100000001f8de6273285b13f20b59195c4...",
    "walletId": "2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2"
}
```

### Policy Type - webhook

When active, a webhook rule will issue a callback to the HTTPS endpoint specified in the condition. The rule will trigger an action if the HTTPS endpoint returns a non-200 (status) response.

Conditions for this rule:

| Field | Description                      | Possible Values |
| ----- | -------------------------------- | --------------- |
| url   | The URL to issue the callback to | HTTPs endpoint  |

Body parameters sent in the callback:

| Field         | Description                                                               | Possible Values                                                              |
| ------------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| walletId      | The ID of the wallet from which the transaction is originating            | "2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe"                                        |
| ruleId        | The ID of the wallet from which the transaction is originating            | "webhookPolicy1"                                                             |
| outputs       | Array of output objects containing outputAddress and value                | [{ "outputAddress":"2N9kNR8iS46WuwekvQVaTUa74w2fbvAXHQn", "value":24885327}] |
| spendAmount   | The net spend amount from the wallet, less change (includes fees)         | 10009060                                                                     |
| approvalCount | The number of user approvals on the wallet thus far                       |                                                                              |
| unsignedRawTx | The hex string of the half-signed raw transaction                         | "0100000001... 0794c5382a38700000000"                                        |
| sequenceId    | The custom sequence ID provided by the sender when creating a transaction | "custom1"                                                                    |

### Policy Action Object

An action is the action to take when the condition is not met by a transaction.

| Field        | Description                                                                  | Possible Values                 |
| ------------ | ---------------------------------------------------------------------------- | ------------------------------- |
| type         | The type of action                                                           | "deny", "getApproval", "getOTP" |
| actionParams | JSON object containing phone/otpType/duration                                | *See below*                     |
| otpType      | Determines how the code should be sent (must set type === "getOTP")          | "sms"                           |
| phone        | The phone number that will receive the code (must set type === "getOTP")     | "541-754-3010", "+498963648018" |
| duration     | The time in seconds the OTP should be valid for (must set type === "getOTP") | 3600                            |

## Remove Policy Rule

```shell
RULEID='test1'
WALLETID='2MvPJ8GMoFFWdUTh6p42FJV6XpmJEVjxv92'

curl -X DELETE \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"id\": \"$RULEID\" }" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/policy/rule
```

```javascript
var walletId = '2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2';
bitgo.wallets().get({ "id": walletId }, function callback(err, wallet) {
  if (err) { throw err; }
  wallet.removePolicyRule({ "id": ruleId }, function callback(err, wallet) {
    if (err) { throw err; }
    console.dir(wallet.admin.policy);
  });
});
```

> Example Response

```json
{
  "date": "2015-10-29T23:33:20.166Z",
  "id": "5631d2f48cad5fab2c6abe16682372bb",
  "rules": [ ],
  "version": 5
}
```

Removes a policy rule with the id specified. This may require a secondary approval if there is more than 1 administrator on the wallet.

### HTTP Request

`DELETE /api/v1/wallet/:WALLETID/policy/rule`<aside class="info"> This operation requires the session to be unlocked using the Unlock API. </aside> 

### BODY Parameters

| Parameter | Type   | Required | Description                         |
| --------- | ------ | -------- | ----------------------------------- |
| id        | string | YES      | the id of the policy rule to remove |

### Response

Returns the updated Wallet Model object.

# Pending Approvals

## List Pending Approvals

List pending approvals on a wallet or an enterprise by providing either a wallet id or an enterprise in the url. By default, the request returns all the pending approvals for a user.

```shell
curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/pendingapprovals?enterprise=$ENTID
```

```javascript
var enterpriseId = '2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2';
bitgo.pendingapprovals().list({
  "enterpriseId": enterpriseId
}, function(err, pendingapprovals) {
  if (err) { throw err; }
  console.dir(pendingapprovals);
});
```

> Example Response

```json
{
   "pendingApprovals": [
       {
           "createDate": "2015-04-28T21:57:52.747Z",
           "creator": "54c2...0c79",
           "enterprise": "54a2...1e88",
           "id": "5540...0633",
           "info": {
               "policyRuleRequest": {
                   "action": "update",
                   "policyChanged": "553e...84c7",
                   "update": {
                       "action": {
                           "type": "getApproval"
                       },
                       "condition": {
                           "amount": 300000000
                       },
                       "id": "com.bitgo.limit.day",
                       "type": "dailyLimit"
                   }
               },
               "type": "policyRuleRequest"
           },
           "state": "pending"
       }
   ]
}
```

### HTTP Request

`GET /api/v1/pendingapprovals`

### URL Parameters

| Parameter  | Type             | Required | Description                     |
| ---------- | ---------------- | -------- | ------------------------------- |
| walletId   | address (string) | NO       | The base address of the wallet  |
| enterprise | string           | NO       | The public ID of the enterprise |

### Response

Returns a list of pending approvals.

## Update Pending Approval

Update the state of a pending approval to either 'approved' or 'rejected'.

```shell
curl -X PUT \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN_OTHERENTERPRISEUSER" \
-d '{ "state": "approved" }' \
https://test.bitgo.com/api/v1/pendingapprovals/:PENDINGAPPROVALID
```

```javascript
var enterpriseId = '2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2';
bitgo.pendingapprovals().list({
  "enterpriseId": enterpriseId
}, function(err, pendingapprovals) {
  if (err) { throw err; }
  var pendingapproval = pendingapprovals[0];
  pendingapproval.approve({
    "walletPassphrase": "pa55w0rd"
  }, function(err, res) {
    if (err) { throw err; }
    console.dir(res);
  });
});
```

> Example Response

```json
{
   "createDate": "2015-04-28T21:58:02.416Z",
   "creator": "54c2...0c79",
   "enterprise": "54a2...1e88",
   "id": "5540...10f7",
   "info": {
       "policyRuleRequest": {
           "action": "update",
           "policyChanged": "553e...84c7",
           "update": {
               "action": {
                   "type": "getApproval"
               },
               "condition": {
                   "amount": 300000000
               },
               "id": "com.bitgo.limit.day",
               "type": "dailyLimit"
           }
       },
       "type": "policyRuleRequest"
   },
   "resolvers": [
       {
           "date": "2015-04-28T22:02:32.395Z",
           "user": "5458...4fd2"
       }
   ],
   "state": "approved"
}
```

### HTTP Request

`PUT /api/v1/pendingapprovals/:pendingApprovalId`

### BODY Parameters

| Parameter | Type   | Required | Description                                                   |
| --------- | ------ | -------- | ------------------------------------------------------------- |
| state     | string | YES      | the new state of the pending approval: 'approved', 'rejected' |

### Response

Returns the updated pending approvals with the new state.

# Address Labels

Labels allow you to keep track of addresses with human readable notes. You can add a label to any valid address; the address does not need to be one controlled by the wallet. Address labels are distinct from wallet labels, but they are tied to a wallet so that when you share the wallet with other users they will be also able to view the labels.

## List Labels For All Wallets

```shell
curl -X GET \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/labels
```

```javascript
bitgo.labels({}, function callback(err, labels) {
  if (err) {
    // handle error
  }
  // do something with labels
  console.dir(labels);
});
```

Get the list of labels for the user

### HTTP Request

`GET /api/v1/labels`

> Example response

```json
{
  "labels": [
    {
      "walletId": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
      "address": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
      "label": "My Favorite Multi-Sig Address"
    },
    {
      "walletId": "2MyWgQ3PMAWPenJnHJUPpVjFDLHhaPkZCz5",
      "address": "msWpfbCKSaz4wvqtqMwfmFkE58mTKxsRFG",
      "label": "Watch-only Address"
    }
  ]
}
```

### Response

Returns an array of Label Model objects.

| Field    | Description                                         |
| -------- | --------------------------------------------------- |
| walletId | id of the wallet (also the first receiving address) |
| address  | the bitcoin address being labeled                   |
| label    | the address label                                   |

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |

## List Labels For Specific Wallet

```shell
WALLET=2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf
curl -X GET \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/labels/$WALLET
```

```javascript
var wallets = bitgo.wallets();
var data = {
  "type": "bitcoin",
  "id": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
};
wallets.get(data, function callback(err, wallet) {
  if (err) {
    // handle error
  }
  // do something with labels
  wallet.labels({}, function (err, labels) {
    if (err) {
      console.log(err);
      process.exit(-1);
    }
    console.dir(labels);
  });
});
```

Get the list of labels for the wallet

### HTTP Request

`GET /api/v1/labels/:walletId`

> Example response

```json
{
  "labels": [
    {
      "walletId": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
      "address": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
      "label": "My Favorite Multi-Sig Address"
    }
  ]
}
```

### Response

Returns an array of Label Model objects.

| Field    | Description                                         |
| -------- | --------------------------------------------------- |
| walletId | id of the wallet (also the first receiving address) |
| address  | the bitcoin address being labeled                   |
| label    | the address label                                   |

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |
| 404 Not Found    | The wallet could not be found.                    |

## Set Label

```shell
WALLET=2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf
ADDRESS=2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
curl -X PUT \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"label\": \"A Noteworthy Label\" }" \
https://test.bitgo.com/api/v1/labels/$WALLET/$ADDRESS
```

```javascript
var wallets = bitgo.wallets();
var data = {
  "type": "bitcoin",
  "id": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
};
wallets.get(data, function callback(err, wallet) {
  if (err) {
      console.log(err);
      process.exit(-1);
  }
  wallet.setLabel({label: "A Noteworthy Label", address: "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD"}, function (err, label) {
    if (err) {
      console.log(err);
      process.exit(-1);
    }
    console.dir(label);
  });
});
```

Set a label on a specific address and associate it with a specific wallet. Labels are limited to 250 characters in length. Labels cannot be set on a wallet's first receiving address because it reserved for the wallet's label.

### HTTP Request

`PUT /api/v1/labels/:walletId/:address`

> Example response

```json
{
  "walletId": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
  "address": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
  "label": "A Noteworthy Label"
}
```

### URL Parameters

| Name     | Type                     | Required | Description                                         |
| -------- | ------------------------ | -------- | --------------------------------------------------- |
| walletId | bitcoin address (string) | YES      | id of the wallet (also the first receiving address) |
| address  | bitcoin address (string) | YES      | the bitcoin address being labeled                   |

### PUT Parameters

| Name  | Type   | Required | Description       |
| ----- | ------ | -------- | ----------------- |
| label | string | YES      | the address label |

### Response

Returns a Label Model object.

| Field    | Description                                         |
| -------- | --------------------------------------------------- |
| walletId | id of the wallet (also the first receiving address) |
| address  | the bitcoin address being labeled                   |
| label    | the address label                                   |

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |
| 404 Not Found    | The wallet could not be found.                    |

## Delete Label

```shell
WALLET=2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf
ADDRESS=2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
curl -X DELETE \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/labels/$WALLET/$ADDRESS
```

```javascript
wallet.deleteLabel({address: "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD"}, function (err, label) {
  if (err) {
    // handle error
  }
  console.dir(label);
});
```

Delete a label from a specific address and wallet.

### HTTP Request

`DELETE /api/v1/labels/:walletId/:address`

> Example response

```json
{
  "walletId": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
  "address": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
  "label": "A Noteworthy Label"
}
```

### URL Parameters

| Name     | Type                     | Required | Description                                         |
| -------- | ------------------------ | -------- | --------------------------------------------------- |
| walletId | bitcoin address (string) | YES      | id of the wallet (also the first receiving address) |
| address  | bitcoin address (string) | YES      | the bitcoin address being labeled                   |

### Response

Returns a Label Model object of the label that was deleted.

| Field    | Description                                         |
| -------- | --------------------------------------------------- |
| walletId | id of the wallet (also the first receiving address) |
| address  | the bitcoin address being labeled                   |
| label    | the address label                                   |

### Errors

| Response         | Description                                       |
| ---------------- | ------------------------------------------------- |
| 400 Bad Request  | The request parameters were missing or incorrect. |
| 401 Unauthorized | The authentication parameters did not match.      |
| 404 Not Found    | The wallet or label could not be found.           |

# Tags

Tags are an advanced policy feature for enterprise clients.

## Create a tag

### HTTP Request

`POST /api/v1/tag`

### BODY Parameters

The tag must have a name and exactly one of either a user, wallet, or enterprise.

<table>
  <tr>
    <th>
      Parameter
    </th>
    
    <th>
      Type
    </th>
    
    <th>
      Required
    </th>
    
    <th>
      Description
    </th>
    
    <th>
      Possible Values
    </th>
  </tr>
  
  <tr>
    <td>
      name
    </td>
    
    <td>
      string
    </td>
    
    <td>
      YES
    </td>
    
    <td>
      The name of the tag.
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      user
    </td>
    
    <td>
      id
    </td>
    
    <td>
      NO
    </td>
    
    <td>
      The user id of the user who owns the tag, which can only be the id of the user adding the tag.
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      wallet
    </td>
    
    <td>
      id
    </td>
    
    <td>
      NO
    </td>
    
    <td>
      The id, not bitcoin address, of the wallet to own the tag.
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      enterprise
    </td>
    
    <td>
      id
    </td>
    
    <td>
      NO
    </td>
    
    <td>
      The id of the enterprise to own the tag.
    </td>
    
    <td>
    </td>
  </tr>
</table>

### Response

Returns the tag.

```json
{
  "_id": "[id]",
  "name": "name of tag",
  "user": "[id]"
}
```

### Errors

| Response        | Description                                       |
| --------------- | ------------------------------------------------- |
| 400 Bad Request | The request parameters were missing or incorrect. |

## Add a tag to a wallet

### HTTP Request

`POST /api/v1/wallet/:wallet/tag`

### BODY Parameters

You must specify the id of the tag you are adding to the wallet, and no other parameters are required.

<table>
  <tr>
    <th>
      Parameter
    </th>
    
    <th>
      Type
    </th>
    
    <th>
      Required
    </th>
    
    <th>
      Description
    </th>
    
    <th>
      Possible Values
    </th>
  </tr>
  
  <tr>
    <td>
      tag
    </td>
    
    <td>
      id
    </td>
    
    <td>
      YES
    </td>
    
    <td>
      The id of the tag.
    </td>
    
    <td>
    </td>
  </tr>
</table>

### Response

Returns the wallet, tag, and number of wallettxs affected.

```json
{
  "wallet": "[id]",
  "tag": "[id]",
  "walletTxsAffected": 0
}
```

### Errors

| Response      | Description           |
| ------------- | --------------------- |
| 404 Not Found | The tag was not found |

# Webhook Notifications

> Example transaction Webhook callback

    POST http://your.server.com/webhook
    {
      "type": "transaction",
      "walletId": "2MwLxgWaAGmMT9asT4nAdeewWzPEz3Sn5Eg",
      "hash": "c9a55f32d4b63d02a617eea58873227e4f011d0dfc836cb2e9cab531c4db0c4a"
    }
    

> Example pending approval Webhook callback

    POST http://your.server.com/webhook
    {
      "type": "pendingapproval",
      "pendingApprovalId": "55e79a1b5f9a20da1d3fe5b988a71c93",
      "walletId": "2MwLxgWaAGmMT9asT4nAdeewWzPEz3Sn5Eg",
      "state": "accepted"
    }
    

Webhooks may be setup up to programmatically receive callbacks from BitGo. These may be attached to wallets (in the case of transactions), or to a user (for block notifications). Webhook notifications are triggered when the specified event occurs, such as an incoming transaction.

BitGo servers will make a POST http request to the URL defined with a JSON payload, and expect a `HTTP 200 OK`. If a successful response is not received, BitGo will attempt to retry the webhook with an increasing delay between each retry.

Developers should take care to ensure that their application succeeds even in the cases of transient network error, or if receive the same webhook twice due to an improper acknowledgement.

### Request Schema

The Webhook URL will be called with the following JSON-encoded fields in the HTTP body.

| Field             | Description                                                                                                |
| ----------------- | ---------------------------------------------------------------------------------------------------------- |
| type              | type of Webhook: 'transaction', 'transactionExpire', 'transactionRemoved', 'block', and 'pendingapproval'. |
| walletId          | ID of the wallet associated with the webhook event, if this is a wallet webhook.                           |
| hash              | transaction ID, if this is a transaction webhook                                                           |
| pendingApprovalId | pending approval ID, if this is a pending approval webhook                                                 |
| state             | the state of the pending approval (pending, approved, or rejected), if this is a pending approval webhook  |

### Webhook Types

BitGo is currently actively working on webhooks. Please get in touch with us to request more webhook types.

| Type               | Description                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| transaction        | Activates when a transaction is seen/confirmed on any receive address of a wallet                 |
| transactionRemoved | Activates when a transaction is removed from a user's wallet                                      |
| transactionExpire  | Activates when a transaction is about to expire                                                   |
| pendingapproval    | Activates when a pending approval pertaining to a user's wallet is created, approved, or rejected |
| block              | Activates when a new block is seen on the Bitcoin network                                         |

## List Wallet Webhooks

```shell
curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/webhooks
```

```javascript
bitgo.wallets().get({ "id": walletId }, function(err, wallet) {
    wallet.listWebhooks({}, function callback(err, result) {
        console.dir(result);
    });
});
```

> Example Response

```json
[
    {
      "walletId": "2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu",
      "type": "transaction",
      "url": "http://www.yoursite.com/partner/webhooks"
    },
    {
      "walletId": "2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu",
      "type": "transaction",
      "url": "http://www.company2.com/api/webhooks",
      "numConfirmations": 3
    }
]
```

Gets list of webhooks set up on the wallet. Currently, the only types of webhooks that can be attached to a wallet are transaction and pendingapproval notifications.

### HTTP Request

`GET /api/v1/wallet/:walletId/webhooks`

### Response

An array of Webhook objects

| Field    | Description                          |
| -------- | ------------------------------------ |
| walletId | id of the wallet                     |
| type     | type of Webhook, e.g. transaction    |
| url      | http/https url for callback requests |

## Add Wallet Webhooks

```shell
WALLETID=2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu
URL='http://www.yoursite.com/partner/webhooks'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"url\": \"$URL\", \"type\": \"transaction\" }" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/webhooks
```

```javascript
bitgo.wallets().get({ "id": walletId }, function(err, wallet) {
    wallet.addWebhook({ url: url, type: 'transaction' }, function callback(err, result) {
        console.dir(result);
    });
});
```

> Example Response

```json
{
  "walletId": "2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu",
  "type": "transaction",
  "url": "http://www.yoursite.com/partner/webhooks"
}
```

Adds a Webhook that will result in a HTTP callback at the specified URL from BitGo when events are triggered. There is a limit of 10 Webhooks of each type per wallet.

There are 2 types of wallet webhooks available: 1. Transaction webhooks will fire on any transaction on the wallet. 2. Pending approval webhooks will fire when an outgoing transaction has triggered policy on the wallet.

### HTTP Request

`POST /api/v1/wallet/:walletId/webhooks`

### Parameters

| Parameter        | Type    | Required | Description                                                                                                                                                                                                        |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type             | string  | YES      | type of Webhook, e.g. transaction or pendingapproval                                                                                                                                                               |
| url              | string  | YES      | valid http/https url for callback requests                                                                                                                                                                         |
| numConfirmations | integer | NO       | number of confirmations before triggering the transaction webhook. If 0 or unspecified, requests will be sent to the callback endpoint will be called when the transaction is first seen and when it is confirmed. |

### Response

| Field    | Description                          |
| -------- | ------------------------------------ |
| walletId | id of the wallet                     |
| type     | type of Webhook, e.g. transaction    |
| url      | http/https url for callback requests |

## Remove Wallet Webhooks

```shell
WALLETID=2NGJP7z9DZwyVjtY32YSoPqgU6cG2QXpjHu
URL='http://www.yoursite.com/partner/webhooks'

curl -X DELETE \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"url\": \"$URL\", \"type\": \"transaction\" }" \
https://test.bitgo.com/api/v1/wallet/$WALLETID/webhooks
```

```javascript
bitgo.wallets().get({ "id": walletId }, function(err, wallet) {
    wallet.removeWebhook({ url: url, type: 'transaction' }, function callback(err, result) {
        console.dir(result);
    });
});
```

> Example Response

```json
{
  "removed": 1
}
```

Removing a Webhook will cause new events of the specified type to no longer trigger HTTP callbacks to your URLs.

### HTTP Request

`DELETE /api/v1/wallet/:walletId/webhooks`

### Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| type      | string | YES      | type of Webhook, e.g. transaction                        |
| url       | string | YES      | valid http/https url for callback requests to be made at |

### Response

| Field    | Description                          |
| -------- | ------------------------------------ |
| walletId | id of the wallet                     |
| type     | type of Webhook, e.g. transaction    |
| url      | http/https url for callback requests |

## List User Webhooks

```shell
curl -X GET \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
https://test.bitgo.com/api/v1/webhooks
```

```javascript
bitgo.listWebhooks({}, function callback(err, result) {
  console.dir(result);
});
```

> Example Response

```json
[
    {
        "type": "block",
        "coin": "bitcoin",
        "url": "https://303fe960.ngrok.com"
    },
    {
        "type": "block",
        "coin": "bitcoin",
        "url": "http://www.yoursite.com/partner/webhooks"
    }
]
```

Gets list of webhooks attached to the user. Currently, the only type of webhook that can be attached to a user is a block notification.

### HTTP Request

`GET /api/v1/webhooks`

### Response

An array of Webhook objects

| Field | Description                                                                   |
| ----- | ----------------------------------------------------------------------------- |
| type  | type of Webhook, e.g. block                                                   |
| coin  | string | NO | the network token e.g. "bitcoin" or "eth" (defaults to bitcoin) |
| url   | http/https url for callback requests                                          |

## Add User Webhooks

```shell
URL='https://303fe960.ngrok.com'

curl -X POST \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"url\": \"$URL\", \"type\": \"block\", \"coin\": \"bitcoin\" }" \
https://test.bitgo.com/api/v1/webhooks
```

```javascript
bitgo.addWebhook({ url: url, type: 'block', coin: 'bitcoin' }, function callback(err, result) {
        console.dir(result);
    });
});
```

> Example Response

```json
{
  "type": "block",
  "type": "bitcoin",
  "url": "http://www.yoursite.com/partner/webhooks"
}
```

Adds a webhook that will result in a HTTP callback at the specified URL from BitGo when events are triggered. The webhook record is attached to the user account.

### HTTP Request

`POST /api/v1/webhooks`

### Parameters

| Parameter | Type   | Required | Description                                                     |
| --------- | ------ | -------- | --------------------------------------------------------------- |
| type      | string | YES      | type of Webhook, e.g. block                                     |
| coin      | string | NO       | the network token e.g. "bitcoin" or "eth" (defaults to bitcoin) |
| url       | string | YES      | valid http/https url for callback requests                      |

### Response

| Field | Description                          |
| ----- | ------------------------------------ |
| type  | type of Webhook, e.g. block          |
| url   | http/https url for callback requests |

## Remove User Webhooks

```shell
URL='http://www.yoursite.com/partner/webhooks'

curl -X DELETE \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $ACCESS_TOKEN" \
-d "{ \"url\": \"$URL\", \"type\": \"block\" }" \
https://test.bitgo.com/api/v1/webhooks
```

```javascript
bitgo.removeWebhook({ url: url, type: 'block' }, function callback(err, result) {
  console.dir(result);
});
```

> Example Response

```json
{
  "removed": 1
}
```

Removing a Webhook will cause new events of the specified type to no longer trigger HTTP callbacks to your URLs.

### HTTP Request

`DELETE /api/v1/webhooks`

### Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| type      | string | YES      | type of Webhook, e.g. transaction                        |
| url       | string | YES      | valid http/https url for callback requests to be made at |

### Response

| Field | Description                          |
| ----- | ------------------------------------ |
| type  | type of Webhook, e.g. transaction    |
| url   | http/https url for callback requests |

# Utilities

This section describes utility services provided as part of the BitGo API.

## Decrypt

```javascript
var encryptedString = '{"iv":"n4zHXVTi/Go/riCP8fNs/A==","v":1,"iter":10000,"ks":256,"ts":64,"mode":"ccm","adata":"","cipher":"aes","salt":"zvLyve+4AJU=","ct":"gNMqheicMoD8ZmNzRwuQfWGAh+HA933l"}';
var decryptedString = bitgo.decrypt({ password: "password", input: encryptedString });
// decryptedString = "this is a secret"
```

```shell
Available only as a local method (BitGo Express)

PASSWORD='password'
INPUT='{\"iv\":\"n4zHXVTi/Go/riCP8fNs/A==\",\"v\":1,\"iter\":10000,\"ks\":256,\"ts\":64,\"mode\":\"ccm\",\"adata\":\"\",\"cipher\":\"aes\",\"salt\":\"zvLyve+4AJU=\",\"ct\":\"gNMqheicMoD8ZmNzRwuQfWGAh+HA933l\"}'

curl -X POST \
-H "Content-Type: application/json" \
-d "{ \"password\": \"$PASSWORD\", \"input\": \"$INPUT\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/decrypt

{ "decrypted" : "this is a secret" }
```

Client-side function to decrypt an encrypted blob from the BitGo API.

## Encrypt

```javascript
var encryptedString = bitgo.encrypt({ password: "password", input: "this is a secret" });
// encyrptedString = "{\"iv\":\"n4zHXVTi/Go/riCP8fNs/A==\",\"v\":1,\"iter\":10000,\"ks\":256,\"ts\":64,\"mode\":\"ccm\",\"adata\":\"\",\"cipher\":\"aes\",\"salt\":\"zvLyve+4AJU=\",\"ct\":\"gNMqheicMoD8ZmNzRwuQfWGAh+HA933l\"}"
```

```shell
Available only as a local method (BitGo Express)

PASSWORD='password'
INPUT='this is a secret'

curl -X POST \
-H "Content-Type: application/json" \
-d "{ \"password\": \"$PASSWORD\", \"input\": \"$INPUT\" }" \
http://$BITGO_EXPRESS_HOST:3080/api/v1/encrypt

{ "encrypted" : "{\"iv\":\"U4uRz85ytmlCeTe2P3iOmg==\",\"v\":1,\"iter\":10000,\"ks\":256,\"ts\":64,\"mode\":\"ccm\",\"adata\":\"\",\"cipher\":\"aes\",\"salt\":\"zvLyve+4AJU=\",\"ct\":\"/tRnUh9LUyI7L5e5LPqpvnPR7RD1CdUi\"}" }
```

Client-side function to encrypt a string. All data stored with BitGo is encrypted using this API.

## Estimate Transaction Fees

```javascript
bitgo.estimateFee({ numBlocks: 6 }, function callback(err, res) {
  console.dir(res);
});
```

```shell
curl -k https://test.bitgo.com/api/v1/tx/fee?numBlocks=6
```

Returns the recommended fee rate per kilobyte to confirm a transaction within a target number of blocks. This can be used to construct transactions. Note: The estimation algorithm is only accurate for a minimum of 2 blocks ahead.

### HTTP Request

`GET /api/v1/tx/fee?numBlocks=6`

> Example response

```json
{ "feePerKb": 21949,
    "numBlocks": 6,
    "confidence": 95,
    "multiplier": 1,
    "feeByBlockTarget": {
        "1": 62246,
        "2": 47730,
        "3": 35130,
        "4": 32837,
        "5": 32837,
        "6": 21949,
        "7": 20017,
        "8": 18644,
        "9": 16545,
        "10": 16545
    }
}
```

### Response

| Field      | Description                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------- |
| confidence | confidence of the estimation as used by BitGo. do not use estimation if this value is less than 85. |
| feePerKb   | fee (in satoshis) per kilobyte to use                                                               |
| multiplier | multiplier amount used by BitGo to compute the feePerKb. informational only - do not use.           |
| numBlocks  | the target number of blocks requested for the estimate                                              |

## Market Price Data

```javascript
bitgo.markets().latest({}, function callback(err, market) {
  if (err) {
    // handle error
  }
  // etc
});
```

```shell
curl -k https://test.bitgo.com/api/v1/market/latest
```

Get information about the current market

### HTTP Request

`GET /api/v1/market/latest`

> Example Market Model response

```json
{
    "latest": {
        "blockchain": {
            "totalbc": 14648675,
            "transactions": 125632
        },
        "currencies": {
            "AUD": {
                "24h_avg": 337.58,
                "ask": 337.44,
                "bid": 337.18,
                "last": 337.44,
                "lastHourHigh": 337.59,
                "lastHourLow": 335.91,
                "monthlyHigh": 345.69,
                "monthlyLow": 322.85,
                "prevDayHigh": 345.69,
                "prevDayLow": 333.21,
                "timestamp": "Fri, 25 Sep 2015 06:31:31 -0000",
                "total_vol": 516.11
            },
            "USD": {
                "24h_avg": 234.34,
                "ask": 236.92,
                "bid": 236.59,
                "last": 236.39,
                "lastHourHigh": 236.2,
                "lastHourLow": 234.66,
                "monthlyHigh": 246.46,
                "monthlyLow": 227.08,
                "prevDayHigh": 236.3,
                "prevDayLow": 232.67,
                "timestamp": "Fri, 25 Sep 2015 06:31:31 -0000",
                "total_vol": 59877.2
            }
        },
        "updateTime": "2015-09-25T06:31:19.087Z"
    }
}
```

### Response

Returns a Market Model object. All prices are denominated in the user's set currency.

| Field       | Description                                                                   |
| ----------- | ----------------------------------------------------------------------------- |
| last        | Latest market price                                                           |
| bid         | Highest current bid price                                                     |
| ask         | Lowest current ask price                                                      |
| volume      | 24 hour volume of bitcoins exchanged                                          |
| high        | Highest market price today                                                    |
| low         | Lowest market price today                                                     |
| monthlyHigh | Highest market price this month                                               |
| monthlyLow  | Lowest market price this month                                                |
| marketcap   | Bitcoin market cap                                                            |
| updateTime  | Datetime of when this data was updated                                        |
| yesterday   | An object containing the prior listed fields, but for yesterday's market data |

## Malware Address List

```shell
curl https://www.bitgo.com/api/v1/malware/bitcoin
```

BitGo maintains a list of addresses known to be used by address-swapping malware. BitGo will refuse to co-sign transactions to these addresses. This API provides the list of addresses which are currently blocked by BitGo. We recommend periodically polling this list (once a day, perhaps), in order to maintain your own block list. Otherwise you may accidentally submit bad transactions to BitGo, if one of your customers is infected and requests a withdrawal.

### HTTP Request

`GET /api/v1/malware/bitcoin`

> Example response

```json
{
  "readme": "This is a list of addresses known by BitGo to be tied to bitcoin-stealing malware. BitGo will not co-sign transactions going to these addresses. It is recommended to periodically query this API to update your own internal list of bad addresses, in order to prevent transaction failures.",
  "addresses": [
    {
      "address": "19ZM2pjq6U4jVb283GZkCPNukjeyb2YZ2u"
    },
    {
      "address": "1EZV9ghJ4rCvwtVE27ZUxY6she4GzeEzCf"
    }
  ]
}
```

### Response

| Field     | Description                             |
| --------- | --------------------------------------- |
| readme    | Human-readable explanation of this list |
| addresses | List of { address: xxx } objects        |

## Verify Bitcoin Address

```javascript
var isValid = bitgo.verifyAddress({ address: "1AJbsFZ64EpEfS5UAjAfcUG8pH8Jn3rn1F" });
```

```shell
Available only as a local method (BitGo Express)

curl -X POST \
-H "Content-Type: application/json" \
-d '{ "address": "2NFasKP9nqHHUBvVHvEGkuHpNA3hjdkUhAV" }' \
http://$BITGO_EXPRESS_HOST:3080/api/v1/verifyaddress
```

Client-side function to verify that a given string is a valid Bitcoin Address. Supports both v1 addresses (e.g. "1...") and P2SH addresses (e.g. "3...").

Returns true if the address is valid.

## BitGo Client Version

```javascript
var version = bitgo.version();
```

Client-side function to get the version of this BitGo SDK.

Returns a string.

# Blockchain Data

BitGo provides a public API for getting blockchain data on addresses and transactions. These APIs do not relate to the concept of BitGo users or wallets. The purpose of this API endpoint is to allow API consumers to get data on non-BitGo addresses and transactions (similar to the concept of txindex and watchonly in the Satoshi client).

For most wallet use cases, developers will want to use the Wallet and Keychain APIs. They support many more operations, such as checking the combined balances of HD wallets, creating addresses, sending transactions, etc.<aside class="success"> The Blockchain Data API does not require authentication, since it returns only public blockchain data. It is still recommended to send the access token in the header to achieve a higher rate limit allowance from our service. </aside> 

## Get Address

Lookup an address with balance info.

```shell
ADDRESS=2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
curl https://test.bitgo.com/api/v1/address/$ADDRESS
```

```javascript
var address = "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD";
bitgo.blockchain().getAddress({ address: address }, function(err, response) {
  if (err) { console.log(err); process.exit(-1); }
  console.dir(response);
});
```

### HTTP Request

`GET /api/v1/address/:address`

### URL Parameters

| Parameter | Type                     | Required | Description         |
| --------- | ------------------------ | -------- | ------------------- |
| address   | bitcoin address (string) | YES      | The bitcoin address |

> Example response

```json
{
  "address": "2N76BgbTnLJz9WWbXw15gp6K9mE5wrP4JFb",
  "balance": 6900000,
  "confirmedBalance": 6900000
}
```

### Response

Returns address summary information.

| Field            | Description                                              |
| ---------------- | -------------------------------------------------------- |
| address          | The address                                              |
| balance          | the balance, including transactions with 0 confirmations |
| confirmedBalance | the confirmed balance                                    |

### Errors

| Response        | Description                                       |
| --------------- | ------------------------------------------------- |
| 400 Bad Request | The request parameters were missing or incorrect. |
| 404 Not Found   | The address was not found                         |

## Get Address Transactions

Get transactions for a given address, ordered by reverse block height.

```shell
ADDRESS=2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
curl https://test.bitgo.com/api/v1/address/$ADDRESS/tx
```

```javascript
var address = "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD";
bitgo.blockchain().getAddressTransactions({address: address}, function(err, response) {
  if (err) { console.log(err); process.exit(-1); }
  console.log(JSON.stringify(response, null, 4));
});
```

### HTTP Request

`GET /api/v1/address/:address/tx`

### URL Parameters

| Parameter | Type                     | Required | Description         |
| --------- | ------------------------ | -------- | ------------------- |
| address   | bitcoin address (string) | YES      | The bitcoin address |

### QUERY Parameters

| Parameter | Type   | Required | Description                                                            |
| --------- | ------ | -------- | ---------------------------------------------------------------------- |
| skip      | number | NO       | The starting index number to list from. Default is 0.                  |
| limit     | number | NO       | Max number of results to return in a single call (default=25, max=250) |

> Example response

```json
{
    "transactions": [
        {
            "id": "951aea423c6ba55ac4e6aba953c1dc08e4854bcdf07cb505c4c69447a3f9712e",
            "caption": "test sending",
            "date": "2014-11-13T01:47:10.000Z",
            "pending": false,
            "entries": [
                {
                    "account": "2MxrfrSPNhj1yVrC1CqyNRMFR8WtM7XxpS7",
                    "value": 1890000
                },
                {
                    "account": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
                    "value": -6900000
                },
                {
                    "account": "2N91XzUxLrSkfDMaRcwQhe9DauhZMhUoxGr",
                    "value": 5000000
                }
            ]
        },
        {
            "id": "c7e823fe39d1f0a4081bbefe53f67ebf117189b43a92da3225aa2e4247e35c68",
            "date": "2014-11-13T01:07:05.000Z",
            "pending": false,
            "entries": [
                {
                    "account": "muqrFWxvQiN6KTSZcUugxivMre2fCHnFnF",
                    "value": -104350000
                },
                {
                    "account": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
                    "value": 6900000
                },
                {
                    "account": "mrGtgBbfp5jnqK6c4QNcad6WdHYQr67W8N",
                    "value": 97440000
                }
            ]
        }
    ],
    "start": 0,
    "count": 2,
    "total": 2
}
```

### Response

Returns an array of Transaction objects. Each transaction contains summary information about how that transaction affected the net balance of any bitcoin address involved in the transaction.

### Errors

| Response        | Description                                       |
| --------------- | ------------------------------------------------- |
| 400 Bad Request | The request parameters were missing or incorrect. |

## Get Address Unspent Outputs

Get unspent outputs going into a given address. Ordered by descending block height (unconfirmed transactions first).

```shell
ADDRESS=2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
curl https://test.bitgo.com/api/v1/address/$ADDRESS/unspents
```

```javascript
var address = "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD";
bitgo.blockchain().getAddressUnspents({address: address}, function(err, response) {
  if (err) { console.log(err); process.exit(-1); }
  console.log(JSON.stringify(response, null, 4));
});
```

### HTTP Request

`GET /api/v1/address/:address/unspents`

### URL Parameters

| Parameter | Type                     | Required | Description         |
| --------- | ------------------------ | -------- | ------------------- |
| address   | bitcoin address (string) | YES      | The bitcoin address |

> Example response

```json
{
    "pendingTransactions": false,
    "unspents": [
        {
            "address": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
            "blockHeight": 308023,
            "confirmations": 85670,
            "date": "2015-04-15T23:31:34.918Z",
            "script": "a9147bd4d50e34791a6373bfa0175bf2cb9796a08f5587",
            "tx_hash": "777cb752b4ecaf84fc3716cc5790cd84d7481764b40d496d81d997e04112818d",
            "tx_output_n": 0,
            "value": 3200000
        },
        {
            "address": "2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD",
            "blockHeight": 308749,
            "confirmations": 84944,
            "date": "2015-04-15T23:31:23.919Z",
            "script": "a9147bd4d50e34791a6373bfa0175bf2cb9796a08f5587",
            "tx_hash": "6a75bda7016698abcd903b4b005b9ba306324a8d50bee932237ffde50a5a9eba",
            "tx_output_n": 0,
            "value": 100000
        }
    ]
}
```

### Response

Returns an array of unspent Transaction objects. Each transaction contains the following information

| Name          | Type   | Description                                          |
| ------------- | ------ | ---------------------------------------------------- |
| address       | string | The address with the unspent output                  |
| tx_hash       | number | Amount in satoshis of the output unspent             |
| tx_output_n | number | Amount in satoshis of the output unspent             |
| value         | number | Amount in satoshis of the output unspent             |
| blockheight   | number | The height in which the transaction was seen         |
| confirmations | number | The number of confirmations for this transaction     |
| date          | date   | The datetime the transaction was seen on the network |
| script        | string | The output bitcoin script in hex format              |

### Errors

| Response        | Description                                       |
| --------------- | ------------------------------------------------- |
| 400 Bad Request | The request parameters were missing or incorrect. |

## Get Transaction Details

```shell
TX=af867c86000da76df7ddb1054b273ca9e034e8c89d049b5b2795f9f590f67648
curl https://test.bitgo.com/api/v1/tx/$TX
```

```javascript
var txId = 'af867c86000da76df7ddb1054b273ca9e034e8c89d049b5b2795f9f590f67648';
bitgo.blockchain().getTransaction({id: txId}, function(err, response) {
  if (err) { console.log(err); process.exit(-1); }
  console.log(JSON.stringify(response, null, 4));
});
```

Gets details for a transaction hash

### HTTP Request

`GET /api/v1/tx/:txid`

### URL Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| txid      | string | YES      | The transaction ID (hash) |

> Example response

```json
{
  "blockhash": "000000009249e7d725cc087cb781ade1dbfaf2bd777822948d5fccd4044f8299",
  "confirmations": 16679,
  "date": "2014-11-06T02:22:55.000Z",
  "entries": [
    {
      "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
      "value": 84000000
    },
    {
      "account": "msj42CCGruhRsFrGATiUuh25dtxYtnpbTx",
      "value": -85900000
    },
    {
      "account": "mwahoJcaVuy2TiMtGDZV9PaujFeD9z1a1q",
      "value": 1890000
    }
  ],
  "fee": 10000,
  "height": 306695,
  "hex": "0100000001b6e8b36132d351b3d66b5452d8f4601e2271a7bb52b644397db956a4ffe2a053000000006a4730440220127c4adc1cf985cd884c383e69440ce4d48a0c4fdce6bf9d70faa0ee8092acb80220632cb6c99ded7f261814e602fc8fa8e7fe8cb6a95d45c497846b8624f7d19b3c012103df001c8b58ac42b6cbfc2223b8efaa7e9a1911e529bd2c8b7f90140079034e75ffffffff0200bd01050000000017a914c449a7fafb3b13b2952e064f2c3c58e851bb943087d0d61c00000000001976a914b0379374df5eab8be9a21ee96711712bdb781a9588ac00000000",
  "id": "af867c86000da76df7ddb1054b273ca9e034e8c89d049b5b2795f9f590f67648",
  "inputs": [
    {
      "previousHash": "53a0e2ffa456b97d3944b652bba771221e60f4d852546bd6b351d33261b3e8b6",
      "previousOutputIndex": 0
    }
  ],
  "outputs": [
    {
      "account": "2NB96fbwy8eoHttuZTtbwvvhEYrBwz494ov",
      "value": 84000000,
      "vout": 0
    },
    {
      "account": "mwahoJcaVuy2TiMtGDZV9PaujFeD9z1a1q",
      "value": 1890000,
      "vout": 1
    }
  ],
  "pending": false
}
```

### Response

Returns detailed information on a transaction, including net effects on all bitcoin addresses involved in the transaction.

## Get Block

```shell
BLOCK=00000000000000066fff8a67fbb6fac31e9c4ce5b1eabc279ce53218106aa26a
curl https://test.bitgo.com/api/v1/block/$BLOCK
```

```javascript
var blockId = '00000000000000066fff8a67fbb6fac31e9c4ce5b1eabc279ce53218106aa26a';
bitgo.blockchain().getBlock({id: blockId}, function(err, response) {
  if (err) { console.log(err); process.exit(-1); }
  console.log(JSON.stringify(response, null, 4));
});
```

Gets a Bitcoin block and the transactions within it. You can use 'latest' to get the latest block on the bitcoin network.

### HTTP Request

`GET /api/v1/block/latest`

`GET /api/v1/block/:blockHeight`

`GET /api/v1/block/:blockHash`

### URL Parameters

| Parameter | Type     | Required | Description                                                               |
| --------- | -------- | -------- | ------------------------------------------------------------------------- |
| id        | variable | YES      | The block hash (string), height (number) or 'latest' for the latest block |
| extended  | boolean  | NO       | Set to true to return details on each transaction within the block        |

> Example response

```json
{
    "chainWork": "60359949399610308617",
    "date": "2015-04-15T23:05:50.139Z",
    "height": 326945,
    "id": "00000000000000066fff8a67fbb6fac31e9c4ce5b1eabc279ce53218106aa26a",
    "previous": "00000000eecd159babde9b094c6dbf1f4f63028ba100f6f092cacb65f04afc46",
    "transactions": [
        "e393422e5a0b4c011f511cf3c5911e9c09defdcadbcf16ceb12a47a80e257aaa",
        "fe429dd68ef56613a038238e81b19e2158ef3ad9d9535d1127018bb78ff83537",
        "e1ee8183626b7854c80563d809f85acc6c7cd878de599bee291d0f759a7b2264",
        "dee33ab621e5409c65948d330f01e1adadb533b0f726d80d48e20bb434b4b542",
        "7d83ee2d59e06bc7efe49c647289d98842a7e9ec3b90c3cc965a81df08c08c8f"
    ]
}
```

### Response

| Name         | Type     | Description                                                 |
| ------------ | -------- | ----------------------------------------------------------- |
| date         | datetime | The timestamp the block was seen on the network             |
| id           | string   | Hash of the block                                           |
| previous     | string   | Hash of the previous block in the chain                     |
| transactions | array    | Array of transaction hashes (strings) that are in the block |

# BitGo Instant

BitGo Instant allows sending on-chain transactions which can be credited instantly by recipients, due to a financial guarantee by BitGo against double-spending. Anyone can receive BitGo Instant transactions. In order to send BitGo Instant transactions, you will need either a BitGo KRS wallet, or will need to arrange a collateral agreement with BitGo.

## Receiving

In order to credit BitGo Instant transactions instantly, you will need to respect the **instant: true** property on the transaction objects returned from the [List Wallet Transactions](#list-wallet-transactions) and [Get Wallet Transaction](#get-wallet-transaction) APIs. Instant transactions will also have a field **instantId** which can be used to [Get the Instant Guarantee](#get-instant-guarantee) on a transaction.

## Sending

You will first need a BitGo Instant-compatible wallet. This can be done by creating a KRS-enabled wallet in the web interface, or using the [Create Wallet API](#create-wallet-with-keychains) with a **backupXpubProvider** specified. If you have an existing non-KRS wallet, it can be upgraded to BitGo Instant-capable by arranging a collateral agreement with BitGo.

In order to send a BitGo Instant transaction, use the **instant: true** flag on any of the transaction APIs, such as [Send Coins to Address](#send-coins-to-address) or [Create Transaction](#create-transaction). BitGoD also has the capability to send BitGo Instant transactions through its JSON interface.

BitGo Instant transactions have stricter requirements about the depths of the inputs being spent. This means that the balance of a wallet available for sending a BitGo Instant transaction may be less than the total balance of the wallet. The **instantBalance** property on the wallet object returned by the [Get Wallet API](#get-wallet) will tell you the available balance for sending a BitGo Instant transaction.

When sending a BitGo Instant transaction, the transaction may fail if you do not have enough confirmed unspents in your wallet, or if the transaction would cause you to exceed the risk limits supported for your wallet. The risk limit is determined by the amount of collateral pledged, or by a risk limit BitGo applies to all wallets served by a particular KRS. You will need to handle potential failures when sending a BitGo Instant transaction, and possibly retry as a standard transaction.

BitGo Instant transactions are provided at no additional cost to any customer on our standard transactional pricing plans, including volume discount plans.

# Partner OAuth

BitGo partners may utilize our OAuth endpoints to obtain authorized access and perform actions on behalf of 3rd party BitGo accounts. BitGo complies with the OAuth standard to allow secure access to customer accounts while keeping their passwords safe.

To begin, partners should obtain OAuth application parameters by getting in touch with us. The OAuth flow typically goes as follows:

  1. You redirect users to log into BitGo via our OAuth gateway at `https://www.bitgo.com/oauth/authorize`. In the parameters of this request, you specify the client id, redirect uri and scope.
  2. The user reaches the BitGo OAuth gateway. We ask them if it's ok for you to gain access to the requested scope. They log in with their password and 2FA to confirm.
  3. We redirect the user back to your redirect Uri, with a code parameter. This authorization code is valid for use by your client ID only.
  4. You send the authorization code back to your servers and create a request to BitGo servers with the code, client id and secret. We exchange this for an access token.
  5. You use the access token in the Authorization header to make API calls on behalf of the BitGo user.

### OAuth Variables

| Name          | Description                                                                                                                                                          |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Client Id     | A string (name) of the OAuth application seeking access to 3rd party accounts. This will be public.                                                                  |
| Client Secret | A secret string, stored on the server of the OAuth consumer, used to convert authorization codes for the client id to access tokens.                                 |
| Redirect Uris | A list of acceptable redirect URIs. When you send users to our OAuth gateway for authorization, we send them back to a Uri on your site with the authorization code. |
| Scope         | A list of OAuth scopes. These are the scopes that your application will be allowed to request for from the user.                                                     |

### Scope Values

The scope values define the allowed operations within an OAuth session. These scopes should be provided to the BitGo OAuth gateway such that BitGo may inform the user of your intent and assign you an appropriate authentication code with the requested scope.

Please specify scopes separated using spaces, e.g. "openid profile wallet_view_enterprise wallet_spend_enterprise".

Note that more powerful scopes do not encompass basic ones, ie. wallet_spend does not encompass wallet_view, so you should request for both.

| OAuth Scope Value          | Description of Allowed Actions                                                           |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| openid                     | Verify the user is logged in and get their User ID                                       |
| profile                    | Get the user's profile, including email and phone number                                 |
| wallet_create              | Create wallets on behalf of the user                                                     |
| wallet_view_enterprise   | View wallets created under their enterprise                                              |
| wallet_spend_enterprise  | Spend Bitcoin from wallets created under their enterprise                                |
| wallet_manage_enterprise | Manage and modify settings from wallets created under their enterprise                   |
| wallet_view:#WALLETID      | View a wallet's transactions and addresses                                               |
| wallet_spend:#WALLETID     | Spend Bitcoin from the specific wallet                                                   |
| wallet_manage:#WALLETID    | Manage and modify settings on the specific wallet                                        |
| wallet_view_all          | View all the transactions and addresses for all wallets the user has access to           |
| wallet_freeze_#WALLETID  | Freeze all spend activity on a specific wallet for a given duration (defaults to 1 hour) |
| wallet_freeze_all        | Freeze all spend activity on all of the user's wallets                                   |

## 3rd Party BitGo Login

> Example OAuth gateway redirect to send your users to BitGo OAuth

    https://test.bitgo.com/oauth/authorize?client_id=FBExchange&redirect_uri=https%3A%2F%2Ffbexchange.com%2Foauth_redirect&scope=openid%20profile%20wallet_view_enterprise&email=test@bitgo.com&signup=false
    

The first step in the OAuth flow is to redirect your users to the BitGo OAuth gateway with the Client ID, Scope and Redirect Uri parameters.

* Test Endpoint: https://test.bitgo.com/oauth/authorize
* Production Endpoint: https://www.bitgo.com/oauth/authorize

Parameters may be sent via GET or POST.

### OAuth Request Parameters

> Example redirect URL from BitGo sending users back to your site

    https://fbexchange.com/oauth_redirect?code=440261e26512877b7ebe86e2740da3030d81e88e
    

| Parameter    | Required | Description                                                                                                                  |
| ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| client_id    | YES      | Name of the OAuth application seeking access to 3rd party accounts                                                           |
| redirect_uri | YES      | Redirect Uri for BitGo to send users back to your site after they have authenticated on BitGo                                |
| scope        | YES      | List of requested OAuth scopes, separated by spaces. Your access to the user's information will be dependent on these scopes |
| state        | NO       | opaque string that you contain any custom information you wish to provide. Send back as a parameter in the redirect Uri      |
| signup       | NO       | boolean value to be used to control if the user defaults to login or sign up when they land on the OAuth gateway at BitGo    |
| email        | NO       | string value of the email username, used to pre-populate the value                                                           |
| force_email  | NO       | if set to true, the email field (set above) will be readonly on the user's client                                            |

### Our server will redirect

After the user has authorized your application, we will redirect back them to your URL. The redirect will contain the URL parameters:

| Parameter | Description                                                                                           |
| --------- | ----------------------------------------------------------------------------------------------------- |
| code      | Authorizing code string which you can use (together with your secret) to exchange for an access token |

## Obtaining Access Tokens

```javascript
var bitgo = new BitGoJS.BitGo({clientId:clientId, clientSecret:clientSecret});
var authorizationCode = '440261e26512877b7ebe86e2740da3030d81e88e';
bitgo.authenticateWithAuthCode({ authCode: authorizationCode }, function(err, result) {
if (err) {
    console.dir(err);
    throw new Error("Could not auth!");
  }
  console.dir(result);
  bitgo.me({}, function(err, response) {
    if (err) {
      console.dir(err);
      throw new Error("Could not get user!");
    }
    console.dir(response);
  });
});
```

```shell
AUTHORIZATION_CODE='440261e26512877b7ebe86e2740da3030d81e88e'
CLIENT_ID='FBExchange'
CLIENT_SECRET='testclientsecret'
curl -X POST https://test.bitgo.com/oauth/token \
-H "Content-Type: application/json" \
-d  "{ \"client_id\": \"$CLIENT_ID\",
       \"client_secret\": \"$CLIENT_SECRET\",
       \"grant_type\": \"authorization_code\",
       \"code\":\"$AUTHORIZATION_CODE\"
    }"
```

When your user receives the authorization code, send it to the service backend you wish to use to perform actions on behalf of the user.

You then need to exchange the authentication code for an access token that you can use as you would the rest of the api.

* Test Endpoint: https://test.bitgo.com/oauth/token
* Production Endpoint: https://www.bitgo.com/oauth/token

### OAuth Token Request Parameters

| Parameter     | Description                                                                            |
| ------------- | -------------------------------------------------------------------------------------- |
| client_id     | A string (name) of the OAuth application seeking access to 3rd party accounts          |
| client_secret | A secret string, stored on the server of the OAuth application, issued to you by BitGo |
| grant_type    | should be 'authorization_code'                                                         |
| code          | The authentication code you received in the redirect from the above user login step    |

> Example response

```json
{
    "token_type":"bearer",
    "access_token":"2dba5167e70d4c18679a9775c57b184951a4fa07",
    "expires_in":3600,
    "expires_at":1418059789,
    "refresh_token":"3f8aa90479b4e3f0ea4544ed302e3bfe91968581",
    "id_token":"eyJ0eXAiOiJIUzI1NiJ96js46Ngvk-uC10YjYcEa4CqIAe-1hX2hYgXq6my...."
}
```

### OAuth Response

Our server will return an access token for use with the API.

The token must be added as a HTTP header to all API calls in the HTTP "Authorization" header:

`Authorization: Bearer <your token goes here>`

| Parameter     | Description                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| token_type    | The type of token e.g. 'bearer'                                                                               |
| access_token  | The token to be used in the Authorization header for subsequent authorized API requests on behalf of the user |
| expires_in    | Number of seconds the token is valid                                                                          |
| expires_at    | Time which the token will expire, in seconds since 1970.                                                      |
| refresh_token | Can be used to obtain another access token, if your session is due to expire                                  |
| id_token      | openid jwt token containing user profile information, if requested as a scope                                 |

## OpenID JSON Web Token

If the partner authentication request had an openid profile scope, a id_token in JSON Web Token (JWT) base64-encoded format will be returned in the response to the OAuth access token request above. You should validate the JSON web token is signed with the HS256 algorithm using your client secret (to prove it came from BitGo).

This token will contain user profile information. You should take care to store this information securely so as not to expose your users to scammers or phishing.

> Example decrypted id_token (from the above access token response)

```json
{
    "phone_number": "+14085551234",
    "sub": "54583af457fd213c2d00000532cc8e44e6bdf943559be179e9475cfe",
    "phone_number_verified": true,
    "iss": "http://dev-accounts.bitgo.com",
    "email_verified": false,
    "zoneinfo": "US/Pacific",
    "access_token": "09d3dc7a41c86ceff09e383acda4840840bf056d",
    "exp": 1416882424,
    "iat": 1416878824,
    "email": "tester@something.com",
    "aud": "TESTCLIENTID"
}
```

### ID Token Claims

| Parameter               | Format            | Description                                                                |
| ----------------------- | ----------------- | -------------------------------------------------------------------------- |
| iat                     | seconds since utc | time this token was created, in seconds since 1970.                        |
| exp                     | seconds since utc | expiry time of the token (when to accept it until), in seconds since 1970. |
| aud                     | string            | the client id                                                              |
| iss                     | uri string        | the issue identifier                                                       |
| sub                     | string            | BitGo unique user id                                                       |
| access_token            | string            | access token you received from this request, for verification purposes     |
| email                   | string            | user email                                                                 |
| email_verified          | boolean           | if the user email has been verified                                        |
| phone_number            | string            | user phone number                                                          |
| phone_number_verified | boolean           | if the phone number has been verified                                      |
| zoneinfo                | TZ                | time zone information, e.g. US/pacific                                     |

## Refreshing Access Tokens

```javascript
// var refreshToken = 'undefined'; // if unset, uses the refresh token saved from a previous authentication request.
var refreshToken = '3f8aa90479b4e3f0ea4544ed302e3bfe91968581' // from above auth request
bitgo.refreshToken({ refreshToken: refreshToken }, function(err, result) {
  if (err) {
    console.dir(err);
    throw new Error("Could not refresh!");
  }
  console.dir(result);
});
```

```shell
REFRESH_TOKEN='3f8aa90479b4e3f0ea4544ed302e3bfe91968581' #from above request
CLIENT_ID='FBExchange'
CLIENT_SECRET='testclientsecret'
curl -X POST https://test.bitgo.com/oauth/token \
-H "Content-Type: application/json" \
-d  "{ \"client_id\": \"$CLIENT_ID\",
       \"client_secret\": \"$CLIENT_SECRET\",
       \"grant_type\": \"refresh_token\",
       \"refresh_token\":\"$REFRESH_TOKEN\"
    }"
```

The access token obtained in the previous step is typically good for an hour.

To extend a user session, you can request another access token using the request token also sent in the previous step.

> Example response

```json
{
    "token_type":"bearer",
    "access_token":"2dba5167e70d4c18679a9775c57b184951a4fa07",
    "expires_in":3600,
    "expires_at":1418059789,
    "refresh_token":"3f8aa90479b4e3f0ea4544ed302e3bfe91968581"
}
```

* Test Endpoint: https://test.bitgo.com/oauth/token
* Production Endpoint: https://www.bitgo.com/oauth/token

Refresh tokens have a lifetime of 2 weeks from creation and are valid for a *single use only*. A new refresh token is assigned to you each time one is used.

### OAuth Token Request Parameters

| Parameter     | Description                                                                               |
| ------------- | ----------------------------------------------------------------------------------------- |
| client_id     | A string (name) of the OAuth application seeking access to 3rd party accounts             |
| client_secret | A secret string, stored on the server of the OAuth application, issued to you by BitGo    |
| grant_type    | should be 'refresh_token'                                                                 |
| refresh_token | The refresh token received when you exchanged the authorization code for the access token |

# Examples

BitGo has provided examples of how to perform several common wallet operations using our SDK. The more important ones are covered here.<aside class="info"> Our SDK and examples default to the BitGo test environment which is connected to the Bitcoin TestNet. Do refer to the \[Test Environments\](#bitgo-api-endpoints) section for further details. </aside> 

The examples below (and more!) can be found in the `BitGoJS/examples` directory in our SDK repository. Please report problems with the examples via email or Git issues.

### Obtaining the Wallet ID

When you create your wallet on the BitGo test website, the wallet id is the first receiving address. It is also in the URI when you click on it from the main menu.

## Get Wallet Balance

> Code Snippet

```javascript
var bitgo = new BitGoJS.BitGo();

// First, Authenticate
bitgo.authenticate({ username: user, password: password, otp: otp }, function(err, result) {
  console.log("Logged in!" );

  // Get the Balance
  bitgo.wallets().get({type: 'bitcoin', id: id}, function(err, wallet) {
    console.log("Balance is: " + (wallet.balance() / 1e8).toFixed(4));
  });
});
```

    $ node getWalletBalance.js tester@bitgo.com superhardseypassphrase 0000000 2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
    

> Example output

    Logged in!
    Balance is: 0.6274
    

This simple example shows how to authenticate and get the wallet. The balance in bitcoins can be found on the wallet model.

### Usage

`node getWalletBalance.js <user> <pass> <otp> <walletId>`

### Parameters

| Name     | Type   | Required | Description                                                         |
| -------- | ------ | -------- | ------------------------------------------------------------------- |
| user     | string | YES      | username (your email on the test environment)                       |
| pass     | string | YES      | password on BitGo                                                   |
| otp      | number | YES      | the one-time-password (you can use 0000000 in the test environment) |
| walletId | string | YES      | id of the wallet (also the first receiving address)                 |

## List Wallet Transactions

> Code Snippet

```javascript
bitgo.authenticate({ username: user, password: password, otp: otp }, function(err, result) {
  console.log("Logged in!" );

  // Get the wallet
  bitgo.wallets().get({type: 'bitcoin', id: id}, function(err, wallet) {
    console.log("Balance is: " + (wallet.balance() / 1e8).toFixed(4));

    // Get the transactions
    wallet.transactions({}, function(err, result) {
      for (var index = 0; index < result.transactions.length; ++index) {
        var tx = result.transactions[index];
        var value = 0;
        for (var entriesIndex = 0; entriesIndex < tx.entries.length; ++entriesIndex) {
          if (tx.entries[entriesIndex].account === wallet.id()) {
            value += tx.entries[entriesIndex].value;
          }
        }
        var verb = (value > 0) ? 'Received' : 'Sent';
        console.log(tx.id + ': ' + verb + ' ' + (value / 1e8).toFixed(8) + 'BTC on ' + tx.date);
      }
    });
  });
});
```

    $ node listWalletTransactions.js tester@bitgo.com superhardseypassphrase 0000000 2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
    

> Example output

    Logged in!
    Balance is: 0.6274
    b9573e0e1d8f22fbfe314760b9abd0b6942132cfb2bd7f9fee9713b545a62689: Received 0.56000000BTC on 2014-11-17T21:20:59.000Z
    b3bd8ac76de2340c1159337acdfcaabf08a9470e8157870bd1d84631a0acc67b: Received 0.00010000BTC on 2014-11-17T21:00:58.000Z
    7f5d6a27ea832b2d0a9b63ecdbccecaadbd582b5be41d5bdeabcce72243366a3: Received 0.00010000BTC on 2014-11-17T19:57:18.000Z
    690b8a83e1685869f6138d9a74776f5f868ffc1121fa22f2086e65400f14ef78: Received 0.00010000BTC on 2014-11-17T19:57:18.000Z
    

This example shows how to get the list of transactions on a wallet. This may be useful for verifying received transaction IDs.

### Usage

`node listWalletTransactions.js <user> <pass> <otp> <walletId>`

### Parameters

| Name     | Type                     | Required | Description                                                         |
| -------- | ------------------------ | -------- | ------------------------------------------------------------------- |
| user     | string                   | YES      | username (your email on the test environment)                       |
| pass     | string                   | YES      | password on BitGo                                                   |
| otp      | number                   | YES      | the one-time-password (you can use 0000000 in the test environment) |
| walletId | bitcoin address (string) | YES      | id of the wallet (also the first receiving address)                 |

## Address Labels

    $ node addressLabels.js tester@bitgo.com superhardseypassphrase 0000000
    

> Example output

    Enter the wallet ID: 2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD
    Which label action do you wish to perform? [list, set, delete]: list
     muYhgJrYZHffyGUmuzETjMcBQZJqFo9Vkg    Secret Stash
     2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD   another label
     2MzjET9tYPPZQtsahFvabhaaPWuDdKn3Dh6   descriptive text
     n1wbb1HeZULZwvtcYMLeFz8J9QD7HkG1pa    watch only address
    

This example shows how to list, set, and delete labels on addresses.

### Usage

`node addressLabels.js <user> <pass> <otp>`

### Parameters

| Name | Type   | Required | Description                                                         |
| ---- | ------ | -------- | ------------------------------------------------------------------- |
| user | string | YES      | username (your email on the test environment)                       |
| pass | string | YES      | password on BitGo                                                   |
| otp  | number | YES      | the one-time-password (you can use 0000000 in the test environment) |

## Create Wallet

> Code Snippet

```javascript
  bitgo.wallets().createWalletWithKeychains({"passphrase": password, "label": label, "backupXpubProvider": "keyternal"}, function(err, result) {
    if (err) { console.dir(err); throw new Error("Could not create wallet!"); }
    console.log("New Wallet: " + result.wallet.id());
    console.dir(result.wallet.wallet);

    console.log("BACK THIS UP: ");
    console.log("User keychain encrypted xPrv: " + result.userKeychain.encryptedXprv);
    console.log("Backup keychain xPub: " + result.backupKeychain.xpub);
});
```

    $ node createWallet.js tester@bitgo.com superhardseypassphrase 0000000 'My API wallet'
    

> Example output

    New Wallet: 2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf
    

```json
{ "id": "2NAGz3TDs5HmBU2SEodtWyks9n5KXVCzBTf",
 "label": "testwallet",
 "isActive": true,
 "type": "safehd",
 "private": { "keychains": [{}] },
 "permissions": "admin,spend,view",
 "admin": {},
 "spendingAccount": true,
 "confirmedBalance": 0,
 "balance": 0,
 "pendingApprovals": [] }
```

    BACK THIS UP:
    User keychain encrypted xPrv: {"iv":"v2aVEG5A8VwnI+ewS..."}
    Backup keychain xPub: xpub6GiRC55CRuv2Fx3ihR9EPCr7gauoJcHqvvdSgQkmMmqMQmvQ1KNSBmKPReryBv8E3qJWkHmCx3cWmLGvbDRzCAoV7HUf8A5LUdRhV46u9h5
    

Creates a wallet on BitGo. The example performs the following steps:

  1. Authenticates with BitGo
  2. Unlocks the account
  3. Creates the user keychain on the client, encrypts it with the password and sends it to the server.
  4. Creates the backup keychain on the client.
  5. Creates the BitGo keychain on the BitGo server.
  6. Creates the wallet with the corresponding public keys to the keychains above.<aside class="warning"> It is **VERY IMPORTANT** to have the user print out / back up their user and backup keys. Failure to do so can result in the loss of funds! </aside> 

### Usage

`node createWallet.js <user> <pass> <otp> <label>`

### Parameters

| Name  | Type   | Required | Description                                                         |
| ----- | ------ | -------- | ------------------------------------------------------------------- |
| user  | string | YES      | username (your email on the test environment)                       |
| pass  | string | YES      | password on BitGo                                                   |
| otp   | number | YES      | the one-time-password (you can use 0000000 in the test environment) |
| label | string | YES      | the wallet name as shown in the BitGo UI                            |

## Send Bitcoins to an Address

> Code Snippet

```javascript
var sendBitcoin = function() {
  // Get the wallet
  bitgo.wallets().get({id: walletId}, function(err, wallet) {
    // Send coins
    wallet.sendCoins({ address: destinationAddress, amount: amountSatoshis, walletPassphrase: walletPassphrase }, function(err, result) {
      console.dir(result);
    });
  });
};

// Authenticate and unlock account to enable sending
bitgo.authenticate({ username: user, password: password, otp: otp }, function(err, result) {
  bitgo.unlock({otp: otp}, function(err) {
    sendBitcoin();
  });
});
```

    $ node sendBitcoin tester@bitgo.com superhardseypassphrase 0000000 2N91XzUxLrSkfDMaRcwQhe9DauhZMhUoxGr superhardseypassphrase 2N4Xz4itCdKKUREiySS7oBzoXUKnuxP4nRD 10000
    

> Example output

    { "tx": "01000000017bc6aca03146d8d10b875781...",
      "hash": "101f1f0f2218b0a0ac9aea1c054fbba7d2e75e09fbeeae7acea0254baa9505b7",
      "fee": 10000 }
    

Sends Bitcoin to another bitcoin address. The example uses the following steps:

  1. Authenticates with BitGo
  2. Unlocks the account to make it possible to spend coins
  3. Gets the wallet from the server by the provided walletId.
  4. Calls the wallet.sendCoins method, which finds the user key, decrypts it, creates and signs the transaction and sends it to BitGo for signing.

### Usage

`node sendBitcoin <user> <pass> <otp> <walletId> <walletPassphrase> <destinationAddress> <amountSatoshis>`

### Parameters

| Name               | Type                     | Required | Description                                                         |
| ------------------ | ------------------------ | -------- | ------------------------------------------------------------------- |
| user               | string                   | YES      | username (your email on the test environment)                       |
| pass               | string                   | YES      | password on BitGo                                                   |
| otp                | number                   | YES      | the one-time-password (you can use 0000000 in the test environment) |
| walletId           | bitcoin address (string) | YES      | the wallet name as shown in the BitGo UI                            |
| walletPassphrase   | string                   | YES      | the passphrase used to encrypt the user's private key               |
| destinationAddress | bitcoin address (string) | YES      | the destination address of the wallet                               |
| amountSatoshis     | string                   | YES      | the number of satoshis to send, e.g. 0.1*1e8 for 0.1 bitcoin        |

## Webhook Oracle Policy

> Code Snippet

```javascript
var setUpPolicyAndSendBitcoin = function() {
  console.log("Getting wallet..");
  bitgo.wallets().get({id: walletId}, function(err, wallet) {
    if (err) { console.log("Error getting wallet!"); console.dir(err); return process.exit(-1); }
    console.log("Balance is: " + (wallet.balance() / 1e8).toFixed(4));
    // Sets the policy
    var rule = {
      id: "webhookRule1",
      type: "webhook",
      action: { type: "deny" },
      condition: { "url": url }
    };
    console.dir(rule);
    console.log("Setting webhook policy rule.. ");
    wallet.setPolicyRule(rule, function callback(err, walletAfterPolicyChange) {
      if (err) { throw err; }
      console.log("New policy: ");
      console.dir(walletAfterPolicyChange.admin.policy);
      wallet.sendCoins({ address: destinationAddress, amount: amountSatoshis, walletPassphrase: walletPassphrase },
      function(err, result) {
        // removing the rule
        wallet.removePolicyRule({id: "webhookRule1"}, function(err, res) { console.log("Removed policy rule"); });
        if (err) { console.log("Error sending coins!"); console.dir(err); }
        if (result) {
          console.dir(result);
        }
      });
    });
  });
};

// Authenticate and unlock account to enable sending
bitgo.authenticate({ username: user, password: password, otp: otp }, function(err, result) {
  bitgo.unlock({otp: otp}, function(err) {
    setUpPolicyAndSendBitcoin();
  });
});
```

```shell
$ node addPolicyWebhookAndSendCoins bencxr@fragnetics.com nicehardpassword 0000000 2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2 https://486d7844.ngrok.com/ walletpasw0rd 2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe 1380000
```

> Example output

    Getting wallet..
    Balance is: 1.2582
    { id: 'webhookRule1',
      type: 'webhook',
      action: { type: 'deny' },
      condition: { url: 'https://486d7844.ngrok.com/' } }
    Setting webhook policy rule..
    New policy:
    { id: '5632aa4039cccac60913ea0ed44276c0',
      rules:
       [ { id: 'webhookRule1',
           type: 'webhook',
           action: [Object],
           condition: [Object] } ] }
    { status: 'accepted',
      tx: '0100000001a89f36c0b714878cdd4cddf5...' }
    Removed policy rule
    

> Example webhook callback (sent to the URL provided, any non-200 response will trigger the policy denial)

```json
{
    "approvalCount": 0,
    "outputs": [
        {
            "outputAddress": "2N9kNR8iS46WuwekvQVaTUa74w2fbvAXHQn",
            "outputWallet": "2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2",
            "value": 24885327
        },
        {
            "outputAddress": "2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe",
            "outputWallet": "2N8ryDAob6Qn8uCsWvkkQDhyeCQTqybGUFe",
            "value": 10000000
        }
    ],
    "ruleId": "webhookRule1",
    "spendAmount": 10009060,
    "type": "webhook",
    "unsignedRawTx": "0100000001f8de6273285b13f20b59195c4...",
    "walletId": "2MufYDkh6iwNDtyREBeAXcrRDDAopG1RNc2"
}
```

This example demonstrates how to set up a webhook policy on a wallet capable of executing any custom external logic via a URL endpoint (potentially a script or other program) on the Internet. This allows external state to be mapped into a contract where various users share a single wallet (the URL acts as the "oracle").

When a transaction is made, the URL provided is hit. If it returns a 200 status, then the transaction will be allowed. If not, then the policy rule is fired and the transaction denied.

  1. Authenticates with BitGo
  2. Unlocks the account to make it possible to spend coins and set policy
  3. Gets the wallet from the server by the provided walletId
  4. Sets up the policy which will cause the URL provided to be hit.
  5. Calls the wallet.sendCoins method, which finds the user key, decrypts it, creates and signs the transaction and sends it to BitGo for signing.
  6. Removes the policy and returns the result.

When testing locally, one can create a URL by first setting up a local server (express or any http server will work), and then running a tool such as ngrok to get a public facing url.

### Usage

`node addPolicyWebhookAndSendCoins <user> <pass> <otp> <walletId> <url> <walletPassphrase> <destinationAddress> <amountSatoshis>`

### Parameters

| Name               | Type                     | Required | Description                                                         |
| ------------------ | ------------------------ | -------- | ------------------------------------------------------------------- |
| user               | string                   | YES      | username (your email on the test environment)                       |
| pass               | string                   | YES      | password on BitGo                                                   |
| otp                | number                   | YES      | the one-time-password (you can use 0000000 in the test environment) |
| walletId           | bitcoin address (string) | YES      | the wallet name as shown in the BitGo UI                            |
| url                | http endpoint (string)   | YES      | the URL to set up the policy with                                   |
| walletPassphrase   | string                   | YES      | the passphrase used to encrypt the user's private key               |
| destinationAddress | bitcoin address (string) | YES      | the destination address of the wallet                               |
| amountSatoshis     | string                   | YES      | the number of satoshis to send, e.g. 0.1*1e8 for 0.1 bitcoin        |

## Recover Wallet

RecoverWallet is a tool that can recover funds from a BitGo wallet without using the BitGo service. It is provided for demonstration purposes and to prove that BitGo wallets are 100% recoverable even if the BitGo Service is unavailable. It is not expected that this tool would be used for production environments.

Given only the information on a wallet's keycard, the tool constructs a transaction which moves all of the funds within that wallet to a new account of the user's choice. This is done without using any BitGo Service APIs.

The RecoverWallet tool is interactive or command line driven.

### Usage

node recoverwallet.js --userKey <userkey from keycard> --backupKey <backupkey from keycard> --bitgoKey <bitgo public key from keycard>

### Parameters

| Name        | Meaning                                                                         |
| ----------- | ------------------------------------------------------------------------------- |
| userKey     | The user extended private key for the wallet. (Box A from the Wallet KeyCard)   |
| backupKey   | The backup extended private key for the wallet. (Box B from the Wallet KeyCard) |
| bitgoKey    | The bitgo extended public key for the wallet. (Box C from the Wallet KeyCard)   |
| testnet     | Flag to use testnet instead of the production bitcoin network                   |
| nosend      | Flag to create the transaction but not send it                                  |
| password    | The password to use to decrypt the userKey and backupKey                        |
| destination | The bitcoin address to which you want to send the recovered funds               |