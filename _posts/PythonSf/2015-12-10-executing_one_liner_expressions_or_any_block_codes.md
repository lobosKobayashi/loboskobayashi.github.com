---
layout: post
category : PythonSf
tagline: "one-liners"
tags : [PythonSf, one-liners]
title : カーソル下一行/ブロックの計算式・OS コマンド・任意プログラムのvim による実行--jpn
---
{% include JB/setup %}

# はじめに

1. [前回](http://loboskobayashi.github.io/pythonsf/2015/12-01/recomending_PythonSf_one-liners_for_general_python_codes/)計算または Python codes を one-liners でエディタ上で実行すること
2. pysf.vim/pysf.el エディタ・スクリプトで実装していること

を書きました。でも pysf.vim/pysf.el は Python または　PythonSf one-liners だけではなく Python または PythonSf の block codes も実行できます。Python にかぎらず C/C++ javascript haskell その他任意のブロック・コードもコンパイル・実行できます。さらには OS コマンドも実行可能です。カーソル下の文字列が URL だったりファイル名だったりするときは、それに関連付けられたアプリケーションを一緒に立ちあけられます。

これらが可能になると言うことは 

二階の反対称テンソル
\\[
\begin{align*}
f_{\mu\lambda}=
\begin{bmatrix}
 0    &  cB_z & -cB_y & -iE_x \\
-cB_z &  0    &  cB_x & -iE_y \\
 cB_y & -cB_x &  0    & -iE_z \\
 iE_x &  iE_y &  iE_z &  0
\end{bmatrix}
\end{align*}
\\]
になる．
emacs の shell mode,
vimsh.vim
株価 蝋燭チャート

url2str(u"http://www.amazon.co.jp/TARION%C2%AE-%E3%83%87%E3%82%B8%E3%82%BF%E3%83%AB%E4%B8%80%E7%9C%BC%E3%83%AC%E3%83%95%E3%82%AB%E3%83%A1%E3%83%A9-%E6%B6%B2%E6%99%B6%E3%83%A2%E3%83%8B%E3%82%BF%E3%83%BC%E3%82%84%E3%83%9E%E3%82%B8%E3%83%83%E3%82%AF%E3%82%A2%E3%83%BC%E3%83%A0%E3%81%AA%E3%81%A9%E3%81%AB-%E3%82%B9%E3%83%BC%E3%83%91%E3%83%BC%E3%82%AF%E3%83%A9%E3%83%B3%E3%83%97-%E3%82%AF%E3%83%A9%E3%83%B3%E3%83%97/dp/B00HTXB3O8/ref=pd_sim_sbs_421_2?ie=UTF8&dpID=31taQnVf1oL&dpSrc=sims&preST=_AC_UL160_SR160%2C160_&refRID=033WD5ESWTBBQ26X8NW8")
Traceback (most recent call last):
  File "pysf\sfPPrcssr.py", line 2504, in __execLine
  File "<string>", line 8, in <module>
  File "sfCrrntIni.py", line 893, in url2str
    return md.unquote(strAg).encode('raw_unicode_escape').decode('utf-8').encode('cp932')
UnicodeEncodeError: 'cp932' codec can't encode character u'\xae' in position 30: illegal multibyte sequence

'cp932' codec can't encode character u'\xae' in position 30: illegal multibyte sequence
↓
url2str(u"http://www.amazon.co.jp/TARION-%E3%83%87%E3%82%B8%E3%82%BF%E3%83%AB%E4%B8%80%E7%9C%BC%E3%83%AC%E3%83%95%E3%82%AB%E3%83%A1%E3%83%A9-%E6%B6%B2%E6%99%B6%E3%83%A2%E3%83%8B%E3%82%BF%E3%83%BC%E3%82%84%E3%83%9E%E3%82%B8%E3%83%83%E3%82%AF%E3%82%A2%E3%83%BC%E3%83%A0%E3%81%AA%E3%81%A9%E3%81%AB-%E3%82%B9%E3%83%BC%E3%83%91%E3%83%BC%E3%82%AF%E3%83%A9%E3%83%B3%E3%83%97-%E3%82%AF%E3%83%A9%E3%83%B3%E3%83%97/dp/B00HTXB3O8/ref=pd_sim_sbs_421_2?ie=UTF8&dpID=31taQnVf1oL&dpSrc=sims&preST=_AC_UL160_SR160%2C160_&refRID=033WD5ESWTBBQ26X8NW8")
===============================
http://www.amazon.co.jp/TARION-デジタル一眼レフカメラ-液晶モニターやマジックアームなどに-スーパークランプ-クランプ/dp/B00HTXB3O8/ref=pd_sim_sbs_421_2?ie=UTF8&dpID=31taQnVf1oL&dpSrc=sims&preST=_AC_UL160_SR160,160_&refRID=033WD5ESWTBBQ26X8NW8




