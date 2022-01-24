---
title: "YubikeyでKali Linuxへのログインをカチカチにする"
emoji: "📝"  
type: "tech"
topics: ["security","Yubikey"]
published: true
---

# YubikeyでKali Linuxへのログインをカチカチにする

## やること
- ログイン時にYubikeyを使用するように設定する
- 画面ロック解除時にYubikeyを使用するように設定する
- sudo時にYubikeyを使用するように設定する

## 注意点
1. いきなりloginに設定を行って、ミスってログインできないなんてことが無いようにsudo => login => lightdm(画面ロック時) の順番で設定していく
2. **必ずパスワードとYubikeyを併用する。**  
Yubikeyはあくまで二要素認証のためのデバイスであるため、Yubikeyの接続のみで認証をするような設定にしないこと。  
Yubikeyが接続されたままのPCを紛失した場合、PCにパスワードが書かれた付箋を貼っているようなものになってしまう。

## 手順
FIDO U2F用のパッケージをインストールする
```
$ sudo apt install libpam-u2f
```


ターミナルを開く

設定情報格納用フォルダを作成する
```
$ mkdir -p ~/.config/Yubico
```

YubikeyをPCに接続する

下記のコマンドを実行すると入力待機状態になり、
Yubikeyが点滅するのでタッチセンサ部分にタッチする
```
$ pamu2ufg > ~/.config/Yubico/u2f_keys
```

入力待機状態が終了し、u2f_keysファイルに情報が出力される

バックアップデバイスを追加する場合は **-n** オプションを追加しリダイレクトを **>>** にすることで追加できる
```
$ pamu2ufg -n >> ~/.config/Yubico/u2f_keys
```

u2f_keysファイルを、読取にsudo権限が必要なディレクトリに移動し、よりセキュアにする
```
$ sudo mkdir /etc/Yubico
$ sudo mv ~/.config/Yubico/u2f_keys /etc/Yubico/u2f_keys
```

**/etc/pam.d/u2f-requisite**ファイルを作成し、以下の内容を書き込む
```
auth requisite pam_u2f.so debug_file=/var/log/pam_u2f.log

# auth          => アカウントの有効期限や認証の有効性に関するモジュールを指定
# requisite     => このモジュールが認証を継続するのに必須である事を表す。
# pam_u2f.so    => 実際のモジュールのファイル名
# debug_file=… => デバッグファイルの出力先
```

## 注意ポイント
上の内容では二項目目に**requisite**を指定しているが、この設定を**required**または、**requisite**以外に設定しない事。  
前述の二つのどちらかに設定することでYubikeyでの認証を必須にする。  
詳しい設定項目については以下を参照  
https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/system-level_authentication_guide/pam_configuration_files

**/etc/pam.d/sudo**ファイルを編集する
```diff
#%PAM-1.0

@include common-auth
+ @include u2f-requisite
@include common-account
@include common-session-noninteractive
```

## 注意ポイント
先述の**required**または**requisite**以外が設定されているモジュールが
**@include common-auth**の前の行に記述されているとYubikeyの認証が通った段階で認証OKになってしまい、二要素認証の意味をなさなくなるので必ず上記のような記述にすること。

作業中のターミナルは閉じずに新しいターミナルを開く。  
まず、Yubikeyを接続せずに**sudo echo aaaa**などのコマンドを適当に打つ。  
ここまでの設定が正しければ正しいパスワードを入力したのにも関わらず認証に失敗するはず。

次にYubikeyを接続した状態で新しいターミナルを開き、同じく**sudo echo aaaa**を実行してみる。  
パスワードが要求されるので正しいパスワードを入力すると、Yubikeyが点滅するはず。  
その状態でYubikeyのタッチセンサ部分に触れるとコマンドが実行される。

ここまで問題なく設定出来たら次は **/etc/pam.d/login**に対して同じ設定をしていく。

viで開いて
```
$ sudo vi /etc/pam.d/login
```
u2f-requisiteを使用するように記述
```diff
# Standard Un*x authentication. 
@include common-auth
+ @include u2f-requisite
```

再起動して、Yubikeyを接続している時のみ認証に通ることを確認する。

最後はlightdm(画面ロックからの復帰)の設定

viで開いて
```
$ sudo vi /etc/pam.d/lightdm
```
u2f-requisiteを使用するように記述
```diff
# Load environment from /etc/environment and ~/.pam_environment
session   required pam_env.so readenv=1
session   required pam_env.so readenv=1 envfile=/etc/default/locale

@include common-auth
+ @include u2f-requisite
```

設定後、一度画面ロックをして、Yubikeyが接続されている状態でのみロック解除ができることを確認する。

これでカチカチLinuxの完成。

References:  
https://support.yubico.com/hc/en-us/articles/360016649099-Ubuntu-Linux-Login-Guide-U2F)
https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/system-level_authentication_guide/pam_configuration_files
