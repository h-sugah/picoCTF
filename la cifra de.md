# la cifra de
Author: Alex Fulton/Daniel Tunitis  

### URL
https://play.picoctf.org/practice/challenge/3  

### Description
I found this cipher in an old book. Can you figure out what it says? Connect with nc jupiter.challenges.picoctf.org 32411.  

<br>
<br>

## 解答の過程
問題文で指示されているサイトにncコマンドでアクセスすると、何かの文章のようなテキストが出力される。

```
> nc jupiter.challenges.picoctf.org 32411
Encrypted message:
Ne iy nytkwpsznyg nth it mtsztcy vjzprj zfzjy rkhpibj nrkitt ltc tnnygy ysee itd tte cxjltk

Ifrosr tnj noawde uk siyyzre, yse Bnretèwp Cousex mls hjpn xjtnbjytki xatd eisjd

Iz bls lfwskqj azycihzeej yz Brftsk ip Volpnèxj ls oy hay tcimnyarqj dkxnrogpd os 1553 my Mnzvgs Mazytszf Merqlsu ny hox moup Wa inqrg ipl. Ynr. Gotgat Gltzndtg Gplrfdo

Ltc tnj tmvqpmkseaznzn uk ehox nivmpr g ylbrj ts ltcmki my yqtdosr tnj wocjc hgqq ol fy oxitngwj arusahje fuw ln guaaxjytrd catizm tzxbkw zf vqlckx hizm ceyupcz yz tnj fpvjc hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3x7g996649}

Ehk ktryy herq-ooizxetypd jjdcxnatoty ol f aordllvmlbkytc inahkw socjgex, bls sfoe gwzuti 1467 my Rjzn Hfetoxea Gqmexyt.

Tnj Gimjyèrk Htpnjc iy ysexjqoxj dosjeisjd cgqwej yse Gqmexyt Doxn ox Fwbkwei Inahkw.

Tn 1508, Ptsatsps Zwttnjxiax tnbjytki ehk xz-cgqwej ylbaql rkhea (g rltxni ol xsilypd gqahggpty) ysaz bzuri wazjc bk f nroytcgq nosuznkse ol yse Bnretèwp Cousex.

Gplrfdo’y xpcuso butvlky lpvjlrki tn 1555 gx l cuseitzltoty ol yse lncsz. Yse rthex mllbjd ol yse gqahggpty fce tth snnqtki cemzwaxqj, bay ehk fwpnfmezx lnj yse osoed qptzjcs gwp mocpd hd xegsd ol f xnkrznoh vee usrgxp, wnnnh ify bk itfljcety hizm paim noxwpsvtydkse.
```

中央付近のテキストに、フラグのような文字列が記載されているのが分かる。  

```
hgqqpohzCZK{m311a50_0x_a1rn3x3_h1ah3x7g996649}
```

始めの4文字がくっついているのが分からないが、「pohzCZK」は「picoCTF」になるはずである。  
このような、フラグのフォーマットが見られる場合、換字式暗号を使っているものと推測できる。  
そこで、[CyberChef](https://gchq.github.io/CyberChef/)を利用して変換させてみる。  
ROT13のレシピでブルートフォースを試してみても、特にフラグになるような文字列が得られなかった。  
そのため、ヴィジュネル暗号と考えて変換させてみる。  

1文字目がpで、5文字目がCなので、4文字の周期性があるので、暗号キーは4文字だと判断できる。  

[CyberChef](https://gchq.github.io/CyberChef/)のVigenere Decodeを利用し、1文字ずつ入力して文字列の変化を確認していくと、キーが「agfl」で以下のような文字列になった。  

<br>
<br>

```
picoCTF{b311a50_0r_v1gn3r3_c1ph3r7b996649}
```

<br>
<br>

## フラグ

```
picoCTF{b311a50_0r_v1gn3r3_c1ph3r7b996649}
```

<br>
<br>

## この問題は

ヴィジュネル暗号について学ぶ問題になる。  
