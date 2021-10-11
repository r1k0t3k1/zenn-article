---
title: "CpawCTF level1 Classical Cipher Writeup"
emoji: "👻"
type: "tech"
topics: ["ctf","crypto"]
published: true
---

# 問題
暗号には大きく分けて、古典暗号と現代暗号の2種類があります。  
特に古典暗号では、古代ローマの軍事的指導者ガイウス・ユリウス・カエサル（英語読みでシーザー）が初めて使ったことから、名称がついたシーザー暗号が有名です。  
これは3文字分アルファベットをずらすという単一換字式暗号の一つです。  
次の暗号文は、このシーザー暗号を用いて暗号化しました。暗号文を解読してフラグを手にいれましょう。

暗号文: fsdz{Fdhvdu_flskhu_lv_fodvvlfdo_flskhu}

# Flagの入手方法
問題文に書いてある通りです。
各文字を3文字ずらしていけばFlagが手に入ります。
D → A
E → B

# ソルバ

```julia
c = "fsdz{Fdhvdu_flskhu_lv_fodvvlfdo_flskhu}"

for s in c
  if (Int(s) >= 65 && Int(s) <= 90) || (Int(s) >= 97 && Int(s) <= 122)
    print(Char(Int(s)-3))
  else
    print(s)
  end
end

```