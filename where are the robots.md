# where are the robots
Author: zaratec/Danny

### URL
https://play.picoctf.org/practice/challenge/4

### Description
Can you find the robots? https://jupiter.challenges.picoctf.org/problem/60915/ ([link](https://jupiter.challenges.picoctf.org/problem/60915/)) or http://jupiter.challenges.picoctf.org:60915

<br>
<br>

## 解答の過程

問題文に記載されているリンクからページを開くと、シンプルなWelcomeページが表示される。  
そのページには、「Where are the robots?」と記述されている。  
robotsを見つければ、フラグが得られると推測できる。  
<br>
robotsは、ブラウザーのURL欄に、robots.txtを入力することで参照できる。  
<br>
ここでは、https://jupiter.challenges.picoctf.org/problem/60915/にアクセスしたので、https://jupiter.challenges.picoctf.org/problem/60915/robots.txtと記述することでrobots.txtの内容を確認できる。  
<br>
robots.txtには、以下のように記述されていた。  
```
User-agent: *
Disallow: /8028f.html
```
8028f.htmlがDisallowと記述されているので、8028f.htmlのページが見られたくないのだろう。  
<br>
そこで、8028f.htmlのページにアクセスしてみる。URLは次のようになる。https://jupiter.challenges.picoctf.org/problem/60915/8028f.html  
<br>
アクセスしたページに、フラグが記載されていた。
<br>
<br>
<br>
<br>

## フラグ
```
picoCTF{ca1cu1at1ng_Mach1n3s_8028f}
```
<br>
<br>

## この問題は
robots.txtについて学ぶ問題になる。  

参考：「robots.txt の概要」、https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=ja  
