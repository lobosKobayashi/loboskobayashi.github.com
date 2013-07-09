---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

## 概要

PythonSf を使えば vim や emacs といったエディタを Matlab や Mathematica 以上に有用な計算ツールに変貌させられます。PythonSf と使い慣れたエディタを使って効率的に計算作業をコンピュータに行わせましょう。[1](#test name reference)

## Vim での計算例
Vim エディタで PythonSf 計算をさせるには pysf.vim script を使います。このスクリプトを組み込んだ Vim も Git での PythonSf の配布ファイルに入っています。Vim を使える方は、評価のためには こちらを使われると便利です。ダウンロードした git working directory で下のコマンドを実行させるだけで Python Sf 計算を始められます。

    vim\gvim.exe

\\(\pi^{0.5}\\) を計算したいときの Vim エディタ操作例を見てみましょう。(なお このための<M-F14> Vim script を組み込んだ Vim 自体も　Git PythonSf にもおいてあります。) この Vim エディタで計算をするときは、 pi^0.5 式の文字列行を打ち込み、その行にカーソルを置いたまま Vim normal mode で  **;j** 操作を行います。

    pi^0.5

すると Vim 画面最下段のコマンド・エリアに下の   に計算結果が表示されます

    ===============================
    1.77245385091

この計算結果文字列は Vim の「特殊レジスタ 0 と \*」にも残されています。エディット・テキストにも計算結果を残したいときは、Vim の paste コマンド *p* を実行します。するとエディット画面は下のようになり、後で計算式と計算結果を参照できるようになります。

    pi^0.5
    ===============================
    1.77245385091

「Matlab や Mathematica を超える」などと不遜なことをいうのですから、電卓計算以上のことも もちろん可能です。例えば \\(\int_{-\infty}^{+\infty} e^{-x^{2}} dx\\) 定積分値は下の PythonSf 式で計算できます。

    quadR(exp(-`X^2), -np.inf, np.inf)
    ===============================
    1.77245385091

[0,t] 領域の積分値として定まるエラー関数 \\(\\int_{0}^{t} e^{-x^{2}} dx\\) のグラフを下の PythonSf 式で描かせられます。

    plotGr(λ t:quadR(exp(-`X^2), 0, t), 0,5)

### What is Jekyll?

Jekyll is a parsing engine bundled as a ruby gem used to build static websites from
dynamic components such as templates, partials, liquid code, markdown, etc. Jekyll is known as "a simple, blog aware, static site generator".

###<a id="test name reference"></a> Examples

This website is created with Jekyll. [Other Jekyll websites](https://github.com/mojombo/jekyll/wiki/Sites).



