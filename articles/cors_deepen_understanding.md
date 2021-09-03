---
title: "CORSの理解を深める"
emoji: "📝"
type: "tech"
topics: ["web","security","http","javascript"]
published: true
---

# 初めに
AngularでWEBアプリケーションを初めて書いたときに出たCORS関連のエラーを解決方法だけ検索して解決したものの、仕組みがわからないままずっと引きずっていたので調べてまとめてみます。

# CORSとは
**C**loss **O**rigin **R**esource **S**haringの略  
日本語で表すと『**クロスオリジン間リソース共有**』となります。  
つまり、異なるオリジン間(クロスオリジン)でリソース共有をするためのセキュリティメカニズムです。  
例えば下の図のように`domain-a.com`のWEBページ内で使用する画像を  
`domain-b.com`から取得したい場合にCORSを使用します。

![](https://mdn.mozillademos.org/files/14295/CORS_principle.png)
> http://developer.mozilla.org/ja/docs/Web/HTTP/CORS


# Originとは
あるWEBコンテンツにアクセスするために使用されるURLの  
`プロトコル + ホスト + ポート`がそのWEBコンテンツの
オリジンとなります。  
例えばcontoso.comのport=8080で動作しているWEBコンテンツのオリジンは下記のようになります。
```
https://contoso.com:8080
```


# Closs Origin, Same Originとは
当たり前ですが、二つのOriginが同一ではない場合クロスオリジンとなります。
二つのOriginが同一の場合は同一オリジンとなります。  
同一性の定義は、比較するオリジンの`プロトコル、ホスト、ポートが全て一致するか`どうかです。  
<br>
以下に同一オリジンの例とクロスオリジンの例を挙げます。

## オリジン比較の例
|オリジン|同一？|解説|
|--|--|--|
|`http://contoso.com/v1/index.html`<br>`http://contoso.com/v2/index.html`|○|プロトコルとホストが同一なので同一オリジンです。<br>v1とv2でパスが異なっていますが、同一性にパスは含まれません。|
|`http://contoso.com:80`<br>`http://contoso.com`|○|サーバーは既定で80番ポートで HTTP コンテンツを配信するため、同一オリジンです。|
|`http://contoso.com/app1`<br>`https://contoso.com/app2`|×|httpとhttpsで使用するプロトコルが異なるためクロスオリジンです。|
|`http://contoso.com`<br>`http://www.contoso.com`|×|ホスト名が異なるためクロスオリジンです。|
|`http://contoso.com`<br>`http://contoso.com:8080`|×|ポートが異なるのでクロスオリジンです。|


# CORSの流れ
通常、ブラウザーはセキュリティの観点からアプリケーションが読み込まれたオリジンへのリクエストのみ許可します。  
クロスオリジンへのリクエストはデフォルトで許可されていません。  
この仕組みを[Same Origin Policy(同一オリジンポリシー)](https://developer.mozilla.org/ja/docs/Web/Security/Same-origin_policy)と言います。  
クロスオリジンへのリクエストの場合はリクエストのヘッダにCORSを使用するための設定がされている必要があります。


# Simple Requests(単純リクエスト)とは  
送信するリクエストが**以下の全ての条件を満たす場合**は、**単純リクエスト**となり、
後述する**プリフライトリクエスト**が発生しません。

- リクエストのHTTPメソッドが**GET**,**HEAD**,**POST**のいずれかである。
- 以下のヘッダのみが設定されている。
  - Connection
  - User-Agent
  - Accept
  - Accept-Language
  - Content-Language
  - Content-Type(**但し、下記の条件を満たすもの**)
- Content-Typeヘッダーが下記のいずれかである。
  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain
- リクエストに使用されるどの XMLHttpRequestUpload にもイベントリスナーが登録されていないこと
- リクエストに ReadableStream オブジェクトが使用されていないこと


# Preflighted Requests(プリフライトリクエスト)とは
実際のリクエストの送信前にOPTIONSメソッドによるHTTPリクエストをクロスオリジンに向けて送信し、  
実際のリクエストを送信しても安全かどうかを確かめる仕組みです。  
上記の単純リクエストの条件を満たさないリクエストの場合は全てプリフライトリクエストが飛びます。


# クロスオリジンリクエストの流れ
例として`example.com`から`contoso.com`にリクエストを行う場合を考えてみます。  
## 単純リクエストの場合
ヘッダは下記のようになるかと思います。  
`Origin`というヘッダがありますがこれはクロスオリジンリクエストが発生した場合、自動でブラウザが追加します。

```http
GET /app HTTP/1.1 
Host: contoso.com
Origin: https://example.com
```

アクセスが許可されていた場合、`contoso.com`からのレスポンスは下記のようになります。
```http
200 OK
Content-Type:text/html; charset=UTF-8
Access-Control-Allow-Origin: https://example.com
```

ブラウザはcontoso.comからのレスポンスの`Access-Control-Allow-Origin`を確認し、  
アクセス元(`example.com`)が含まれていれば成功、そうでなければエラーとします。

![](https://github.com/riko-teki/pictures/blob/main/simple-request.png?raw=true)

## プリフライトリクエストの場合
実際にプリフライトリクエストが発生する例を見てみます。
```js
const xhr = new XMLHttpRequest();
xhr.open('POST', 'https://bar.other/resources/post-here/');
xhr.setRequestHeader('X-PINGOTHER', 'pingpong');
xhr.setRequestHeader('Content-Type', 'application/xml');
xhr.onreadystatechange = handler;
xhr.send('<person><name>Arun</name></person>');
```
この例の場合、下記の理由からプリフライトリクエストが飛びます。
- リクエストヘッダーにカスタムヘッダが設定されている(`X-PINGOTHER`)
- `Content-Type`ヘッダーが次のいずれでもない
  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain

プリフライトリクエストからの処理の流れは下記の図のような形になります。  
![](https://mdn.mozillademos.org/files/17268/preflight_correct.png)
### 1回目のリクエスト  
- `Access-Control-Request-Method`で実際のリクエストでPOSTメソッドを使用することをサーバーに通知しています。
- `Access-Control-Request-Headers`で実際のリクエストで使用するヘッダーをサーバーに通知しています。

### 1回目のレスポンス
- `Access-Control-Request-Method`でPOSTメソッドが許可されていることをクライアントに通知しています。
- `Access-Control-Request-Headers`でリクエストヘッダーが許可されていることをクライアントに通知しています。

### 2回目のリクエスト
ここが実際に送信されるリクエストです。1回目のレスポンスで許可されていることを確認したメソッド、ヘッダーでリクエストを送信しています。

### 2回目のレスポンス
サーバーからの実際のレスポンスです。ここがクライアントが受け取りたい実際のレスポンスになります。

# CORSで資格情報を取り扱う
デフォルトではxhrやfetchはクロスオリジンへのリクエストにcockieを送信しませんが
XMLHttpRequestオブジェクト、またはRequestオブジェクトのコンストラクタに特定のフラグを設定することで送信が可能になります。
```js
const invocation = new XMLHttpRequest();
const url = 'http://bar.other/resources/credentialed-content/';

function callOtherDomain() {
  if (invocation) {
    invocation.open('GET', url, true);
    invocation.withCredentials = true; //ココ！
    invocation.onreadystatechange = handler;
    invocation.send();
  }
}
```
この場合、単純リクエストの条件に収まりますのでプリフライトリクエストは発行されません。  
サーバーからのレスポンスに`Access-Control-Allow-Credentials: true`が含まれていない場合、  
ブラウザがこのレスポンスを拒否し、WEBアプリケーションでは使用できなくなります。  
また、レスポンスの`Access-Control-Allow-Origin`がワイルドカードで指定されている場合もリクエストが失敗します。  
なので、サーバーは実際のオリジンを指定してレスポンスを作成する必要があります。
![](https://mdn.mozillademos.org/files/17213/cred-req-updated.png)

# まとめ
- CORSとは異なるオリジン間でのリソース共有をするためのセキュリティメカニズムである
- Originは`プロトコル + ホスト + ポート`で表される
- Simple Requestとは特定の条件下を満たしており、プリフライトリクエストが行われないリクエストのこと
- Preflight RequestとはSimple Requestではないリクエストにおいて実際のリクエストの前に送信されるリクエストのこと

# 参考
https://developer.mozilla.org/ja/docs/Web/HTTP/CORS#simple_requests