---
title: "見習いセキュリティエンジニアでもMINI Hardening出れんの？"
emoji: "📝"
type: "ideas"
topics: ["hardening","Security"]
published: true
---

# 初めに
この記事はセキュリティエンジニアとして働き始めたばかりの筆者が  
2022/04/23に開催された***MINI Hardening***というイベントに参加した感想を述べ、
私と同じようにこのイベントを通じてセキュリティに興味を持ってもらえればいいなという記事です。

# MINI Hardeningとは？
Hardening(固める)という意味から察しがつきますが、システムの脆弱性を減らすことでセキュリティをより堅牢にすること、また、それを競うイベントのことを指します。

今回参加したMINI Hardeningは言葉の通り、Hardeningをちょっと小さい規模で体験できるようなイベントで  
公式の方曰く、『セキュリティインシデントをカジュアルに体験』できるというコンセプトだそうです。
詳細な情報は都合上公開できないのですが公表できる資料のURLを載せておきます。
https://speakerdeck.com/delphinz/mini-hardneing4-introduction

過去19回開催されていて筆者は記念すべき20回目の参加者でした。

# 今回の仮想シチュエーション
参加者に与えられたロールは生半可な覚悟でリリースされたソシャゲの運用をいきなり丸投げされたエンジニアとして、  
各システムをセキュアな状態で稼働させろというシチュエーションです。

参加者は4~5?人で1チームとなり、チームでイベントに臨みます。

与えられたミッションは以下の三つです。
- システムをサイバー攻撃から守る。
- ゲームシステムをチート行為から守る。
- 社長からの無茶振りに答える。

この三点について、どれだけ有効な対処ができたかがチームの得点になります。

# 筆者のスキル
筆者は情シスの経験がちょっとと、セキュリティエンジニアとして最近働き始めたくらいの奴です。
もちろんサービスの運用なんかしたことありませんし、大規模な開発も未経験です。
持っている資格はFEとLPIC Level1くらいです。
正直当日まではチームメンバーの足引っ張りまくってなんにも役に立たないまま終わるんじゃないかと思ってました。

# 当日まで
基本的にはslackでやりとりを行いました。
イベントの1週間前くらいにはチームメンバーが公表され、API仕様書などが公開されていたと思います。
ただ、チームメンバーとのコミュニケーションをとるのが遅く前日に軽く挨拶するくらいで作戦会議等は行いませんでした。
今思うと事前のコミュニケーションと、ある程度の作戦会議は重要だったかなと思います。
仕様書も余り読まないままイベント当日を向かえてました。

# 当日の流れ
当日は詳細説明の後、競技が開始されました。
最初は環境へのアクセスやなんやかんやに戸惑っていましたが特に大きいインシデント等は無く、
なごやかな雰囲気でここが脆弱じゃないか？とかここはこうした方がいいみたいな議論が割とできていました。

と思っていたのも束の間で、中盤からは攻撃、チートの嵐でした。それらの対応に追われる中、  
社長からの無茶振りが飛んできたときはどこか懐かしい苛立ちを覚えました。

終盤には新たな攻撃が確認されても対応する気がなくなり、『へー、こんな攻撃くるんだー』と  
ロールプレイングを放棄していました。

今回はカジュアルに体験するイベントだったからよかったものの、  
実際にセキュリティインシデントの対応をしている方のメンタルへの負担を考えると胃がキリキリします。

競技終了後は実際に行われた攻撃やチートの簡単な解説が行われました。

解説を聞きながら、チームの答え合わせをすることができます。
- 攻撃に至る前に気づき、防げていたもの
- 攻撃を予防することはできなかったが、対策を打てたもの
- 攻撃が行われていたことに気づけなかったもの

ボコボコに攻撃されていたとしても、解説を聞き、自分が気にするべきだった点や  
気にしていて正解だった点が見えて次につながるとても有意義なものでした。

# 教訓
具体的な攻撃手法については述べることはできませんが、  
MINI Hardeningで筆者が得るべき教訓は

- 脆弱なパスワードは使うな。
- 不要なポートは開けるな。
- CMSのバージョンは常に最新にしろ。
- ログは有用な情報が取得できる形式で保存しろ。

の4点です。

以上を徹底するだけでもっといいスコアを取れた気がします。  
いずれもセキュリティの世界では常識中の常識ですが、  
常識だからこそできてないとどうなるの？ということが、実際に攻撃されて学べるのはとてもためになります。

# 持っているとより楽しめる知識
- Webサーバーの知識
- CMSの知識
- Linuxコマンド(ps,tcpdump,cronとか)
- Webアプリケーションの脆弱性
- 最近のセキュリティニュース

# 最後に
今回筆者は初めての参加でしかも、特に有効なスキルもない状態でした。
右も左も分からない状態で一緒に戦ってくれたチームメンバーや、
このようなイベントを開催していただいた運営の方々は本当に尊敬します。

というように、なんのスキルもない私でも楽しめたMINI Hardening、
終わってみれば失うものは何も無く得るものしかなかったので  
筆者のような方々に参加していただければこういったイベントがますます盛り上がるのではないでしょうか。

リベンジしたいな。


