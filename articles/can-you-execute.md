---
title: "CpawCTF level1 [Can you execute?] Writeup"
emoji: "👻"
type: "tech"
topics: ["ctf","crypto"]
published: true
---

# 問題
拡張子がないファイルを貰ってこのファイルを実行しろと言われたが、どうしたら実行出来るのだろうか。
この場合、UnixやLinuxのとあるコマンドを使ってファイルの種類を調べて、適切なOSで実行するのが一般的らしいが…

# Flagの入手方法
問題文と`exec_me`という拡張子の無いファイルが与えられます。
問題文にとあるコマンドを使うというヒントが書いてあります。

このとあるコマンドは`file`コマンドですかね…？

まず、このファイルをWindows環境で実行しようとしても
実行できません。

なので、言われた通り`file`コマンドを使ってみます。
今回は`file`コマンドを使うためにdockerコンテナを立てました。

`file`コマンドを実行してみると、ELFと表示されたので、
実行可能形式のファイルであることが分かりました。  
そして、`for GNU/Linux`とあるのでLinux環境であれば実行できるのでしょう。
```
$ file exec_me
exec_me: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=663a3e0e5a079fddd0de92474688cd6812d3b550, not stripped

```
dockerでLinux環境は構築していたのでそのまま実行してみるとフラグが取得できました。

```
$ ./exec_me
```
