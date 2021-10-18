---
title: "picoCTF Transformation writeup"
emoji: "📝"
type: "tech"
topics: ["ctf","picoCTF"]
published: true
---

# 問題
『これは本当何なのかしら…』という文言と共に`enc`というファイルと 
pythonのコードと思しきコードが与えられます。

```python
''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
```

`enc`の中身は湯呑みたいになってます。
```
灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彤㔲挶戹㍽
```

# 解くためにやったこと
まず読む気がしないpythonのコードが何をしてるかを見ます。  
恐らく平文を二文字ずつbitとして結合させてるんですかね。  
フラグの形式から予測すると最初の`灩`は`p`と`i`の合体ロボでしょう。
試しにこの３文字をbitに変換してみました。
```
julia> bitstring(Int16('灩'))
"0111000001101001"

julia> bitstring(Int8('p'))
"01110000"

julia> bitstring(Int8('i'))
"01101001"
```

この通り単純に上位8bitと下位8bitを別々に文字として出力すればFLAGが取れそうです。

### ソルバ
```julia
c = "灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彤㔲挶戹㍽"

for i in c
    print(Char(Int(i) >> 8))  #上位8bitは右シフト演算で取得
    print(Char(Int(i) & 255)) #下位8bitは255とのAND演算で取得
end
```