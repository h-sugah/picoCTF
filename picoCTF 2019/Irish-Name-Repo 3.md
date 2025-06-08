# Irish-Name-Repo 3
Author: Xingyang Pan

### URL
https://play.picoctf.org/practice/challenge/8

### Description
There is a secure website running at https://jupiter.challenges.picoctf.org/problem/40742/ (link) or http://jupiter.challenges.picoctf.org:40742. Try to see if you can login as admin!



## 解答の過程
linkをクリックして指定されたサイトにアクセスする。  

「List 'o the Irish!」のタイトルとともに、6名の人物の写真とコメントが記載されているページが表示される。  
ページ遷移のリンクがあるが、動作しない。  
画面左上にあるハンバーガーメニューを開くと、以下の3つのメニューが用意されている。  
1. Close Menu
1. Support
1. Admin Login

Close Menuはメニューを閉じるだけである。  
SupportはFAQのようなページが表示される。  
Admin Loginを選択するとAdmin Loginページに遷移する。

Admin Loginページではパスワードを入力する欄とLoginボタンがある。正しいパスワードを入力すると管理者としてログインできると思われる。このように入力する箇所が用意されているということは、この問題の攻略ポイントになると考えられる。  
パスワードを適当に入力してもログイン失敗になるので、パスワードのチェックを通過させるような文字列を入力する必要がある。  
まず、SQLインジェクションの脆弱性が存在しているか確かめるために、クエリーになる以下のような文字列を入力してみたところ、エラーページが表示された。

> ' or 1=1; --

次に、ブラウザーの開発者モードを使用し、ログイン試行時の通信内容を確認した。すると、POSTデータにパスワードとともにdebug=0というデータが付いているのを見つけた。  
![challenge-8--figure1.png](https://github.com/h-sugah/picoCTF/blob/main/picoCTF%202019/pictures/challenge-8--figure1.png)
ログインページのソースコードを見たところ、hidden属性でdebugのinput要素が記載されている。この値を操作することで何かしらの反応が得られると考えられる。  
そこで、Burp Suiteを利用し、hidden属性になっているinput要素のdebugの値を変更することとした。

### Burp Suiteの利用
Butp Suiteは、PortSwigger社が開発したWebアプリケーションのセキュリティ診断や侵入テストを実施するたツールである。Webブラウザーとサーバー間の通信を傍受し、リクエストとレスポンスの確認、編集などを行うことができる。
操作手順は以下のようになる。
- Burp Suiteを起動し、「Proxy」を選択する。
- 「Intercept」タグの画面内にある「Intercept 」スイッチをONに切り替え、「Open browser」ボタンでブランざーを開く。
- ブラウザーが起動したら、対象のサイト（https://jupiter.challenges.picoctf.org/problem/40742/）を開き、Admin Loginページまで遷移する。
- Password欄に「picoCTF」と入力し、「Login」ボタンを押すと、Burp Suiteがリクエストを補足してくれる。
- Burp Suiteが補足したリクエストの内容で、Debugの値を0から1に変更し「Forward」ボタンを押す。これで改変した内容をサイトに送信する。
![challenge-8--figure2.png](https://github.com/h-sugah/picoCTF/blob/main/picoCTF%202019/pictures/challenge-8--figure2.png)
![challenge-8--figure3.png](https://github.com/h-sugah/picoCTF/blob/main/picoCTF%202019/pictures/challenge-8--figure3.png)

操作した結果、サイトはログイン失敗となったが、入力したパスワードの内容とSQL文の内容が表示された。
![challenge-8--figure4.png](https://github.com/h-sugah/picoCTF/blob/main/picoCTF%202019/pictures/challenge-8--figure4.png)
```
password: picoCTF
SQL query: SELECT * FROM admin where password = 'cvpbPGS'
```
password「picoCTF」の文字列が「cvpbPGS」に変化しており、これがSQLインジェクション攻撃が動作しなかった原因なっていることが分かる。
CyberChefを利用し、password文字列の変化を検証してみたところ、ROT13で暗号化していることが分かった。

## パスワードチェックの通過
ROT13で暗号化された結果がSQL文になるように文字列を構成すれば良いため、以下のように文字列を構成し、ログインを試行した。

> ' be 1=1; --

ログインが成功し、以下のようなページが表示された。
「be」の文字がROT13で暗号化されて「or」になり、SQLインジェクションとしてパスワードチェックを通過させる内容になったことが分かる。
![[challenge-8--figure5.png]]




## フラグ
picoCTF{3v3n_m0r3_SQL_4424e7af}
