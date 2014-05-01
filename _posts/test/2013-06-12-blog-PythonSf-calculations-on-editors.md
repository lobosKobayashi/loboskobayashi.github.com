---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [pysf, tutorial]
---
{% include JB/setup %}

## 概要
PythonSf では、petit simulation:one-liner の積み重ねによる数学・物理・工学などにおける様々の問題の検討の積み重ねを主張します。petit simulation:one-liner に拘るのは、数学や物理の問題の検討に集中するためです。そのる最中にプログラミング・デバッグ作業が入ってくると、本来の目的としていることとは異なった考察に値の回転をとられてしまうからです。

If then else 構文が入ってこない、数式を並べただけの one-liner 式ならば、プログラミング作業・デバッグ作業とは無縁です。数学・物理・工学などの問題の考察だけに集中し続けられます。また one-liner が一行だけで独立していることが、それまでの計算過程からの文脈から独立して計算できることも、考察の積み重ねを強く支えて切れます。


PythonSf を使えば vim や emacs といったエディタを Matlab や Mathematica 以上に有用な計算ツールに変貌させられます。

PythonSf は CUI プログラムです。GUI ではありません。これは計算プログラムにとって利点となります。コンピュータでの計算作業は計算式文字列を与えて、コンピュータに計算させ、その結果を待つことの繰り返しだからです。この計算式文字列を与える作業を最も効率的に行えるのはユーザーが使い慣れたエディタです。そして CUI プログラムである PythonSf は容易にエディタに組み込めます。PythonSf と使い慣れたエディタを使って効率的に計算作業をコンピュータに行わせられます。

    PythonSf ワンライナー
    # 数値の加減乗除べき乗算
    (1-2*3/4)^5
    ===============================
    -0.03125

    # sin(1/x) value at x=pi
    sin(1/`X)(pi)
    ===============================
    0.312961796208

    # 数値微分値: sin(1/x) value at x=pi
    ∂x( sin(2/`X) )(`π)
    ===============================
    -0.162946719226

    # シンボリック微分: sin(1/x)
    ts(); ∂x( ts.sin(1/`x) )
    ===============================
    -cos(1/x)/x**2

    # Groebner 基底をシンボリックに計算する
    ts();x,y,z=ts.symbols('x y z');f1,f2,f3=[x+y^2+3z^2-4, 4y^2-5,y^4-5z-10]; ts.groebner([f1,f2,f3], [x,y,z])
    ===============================
    [-5/4 + y**2, 1483/256 + x, 27/16 + z]

    # 行列とベクトルの積演算
    #  |1,2| * |5|
    #  |3,4|   |6]
    mt,vc=~[[1,2],[3,4]], ~[5,6]; mt vc
    ===============================
    [ 17.  39.]
    ---- ClTensor ----

    # W:ワット、A:アンペア単位付きの数値計算
    ts(); wattage,current=100W`,2.3A`; wattage/current^2
    ===============================
    18.9035916824197*V`/A`

    # Bool 体係数多項式の商と剰余
    (`P^7+1)/(`P^2+1)
    ===============================
    (Pl(`P^5+`P^3+`P), Pl(`P+1))

    x=`P; (x^7+1)%(x^2+1)
    ===============================
    `P+1

    # 楕円関数:第一種の不完全/Jacobi 楕円関数の計算
    φ,m=pi/3,0.6; sy(); u=ss.ellipkinc(φ,m); ss.ellipj(u,m)
    ===============================
    (0.8660254037844386, 0.50000000000000011, 0.74161984870956643, 1.0471975511965976)

    # Sn(4) 置換群における 置換集合:{Sb(2,3,0,1), Sb(1,0,3,2),Sb(1,3,2,0)} の左剰余類
    =:SS4; SS4/{Sb(2,3,0,1), Sb(1,0,3,2),Sb(1,3,2,0)}
    ===============================
    kfs([Sb(0,1,2,3), Sb(0,1,3,2), Sb(0,2,1,3), Sb(0,2,3,1), Sb(0,3,1,2), Sb(0,3,2,1)])

    # 数値 1234 と文字列 '1' の hash コードを確認する Python テスト・コード
    hash(1234), hash('1')
    ===============================
    (1234, 1977051568)

    # グラフ描画: sin(1/x) を [0.2, pi] 領域で
    plotGr( sin(1/`X), 0.2, pi )
    [1](#test name reference)

PythonSf ワンライナー;;http://localhost:4000/pysf/manual/one-liners.htm#kOde:%E5%B8%B8%E5%BE%AE%E5%88%86%E6%96%B9%E7%A8%8B%E5%BC%8F
    D=2;inV=[-0.97m`,0.243, 0.97,-0.243, 0,0, -0.466m`/s`,-0.432, -0.466,-0.432, 0.932,0.864]; N=len(inV)//(2D) ; getFV=λ v,i,k:(λ r=krry(v[D k:D (k+1)])-krry(v[D i:D (i+1)]):r/norm(r)^3 if norm(r)!=0 else ~[0,0])(); sumFc=λ v,j:sum([getFV(v,j,k) for k in range(N) if j!=k]); fnc= λ *v: np.r_[v[D N:],(~[sumFc(v,j) for j in range(N)]).r]; mt=kOde(fnc,inV, 2 s`,400); cl=ClCplxColor(); for k in range(N):plotTrajectory(mt[:,D k:D (k+1)],color=cl.GetFltColor(exp(`i 2pi k/N)))

:pysf.vim スクリプト関数・機能表

|関数名                          |機能
|:-------------------------------|:------------------------------------
|ExecPySf_1liner()               |PythonSf ワンライナー式の実行
|ExecPySfOp_1liner()             |PythonSf Open ワンライナー式の実行
|ExecPySf_start_1liner()         |PythonSf ワンライナー式のスタート実行
|ExecSf_Block()                  |PythonSf コード・ブロックの実行
|ExecSfOp_Block()                |PythonSf Open コード・ブロックの実行
|Exec_command()                  |OS コマンドの実行
|Exec_start()                    |OS コマンドの start 実行
|ExecPy_Block()                  |Python コード・ブロックの実行
|Exec_BlockCntn()                |任意コード・ブロックの生成と連続実行
|ConvertAlpbt2Greek()            |ASCII ギリシャ文字漢字変換
|GetDateTime()                   |現在年月日時刻文字列の取得
|StartGoogleSearchUnderCursor()  |Google 検索開始


## Vim での pysf.vim script 組み込み
Vim エディタで PythonSf 計算をさせるには pysf.vim script を Vim に組み込みます。その手順は簡単です。Git で配布している pysf.vim を Vim がインストールされているディレクトリの ..\vim73\plugin\に置くだけです。キー･バインドは pysf.vim にハード･コーディングされています。

![pysf.vim を Vim73\plugin へ](/images/2013/pysfVimToPlugin.png "pysf.vim のコピー")

<!--
//@@
    Git                  Vim73\vim\plugin\
    +------------+       +---------------+  
    |            | copy  |               |  
    |  pysf.vim  +---- ->|  pysf.vim     |  
    |            |       |               |  
    +------------+       +---------------+
                         
//@@@
//java -jar \utl\ditaa0_9.jar __tmp pysfVimToPlugin.png
-->

## Vim での計算例
Vim エディタで PythonSf 計算をさせるには pysf.vim script を使います。このスクリプトを組み込んだ Vim も Git での [PythonSfCp932](https://github.com/lobosKobayashi/PythonSfCp932) の配布ファイルに入っています。Windows で Vim を使える方は、評価のためには こちらを使われると便利です。ダウンロードした git working directory で下のコマンドを実行させるだけで Python Sf 計算を始められます。

    vim\gvim.exe

\\(\pi^{0.5}\\) を計算したいときの Vim エディタ操作例を見てみましょう。 Vim エディタで計算をするときは、 pi^0.5 式の文字列行を打ち込み、その行にカーソルを置いたまま Vim normal mode で  <strong>;j</strong> 操作を行います。

    pi^0.5

すると Vim 画面最下段のコマンド・エリアに下のように計算結果が表示されます

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

![エラー関数グラフ](/images/2013/pysfErrFnctn.png "エラー関数グラフ")

## C++/Boost プログラムのコンパイルと実行
pysf.vim マクロは PythonSf の計算以外でも活用できます。";e" で //@@ ... //@@@ ブロックのファイル化に続けてコマンドを連続実行します。 ";a" 操作でカーソル下の一行を OS コマンドとみなして実行します。 この二つの操作を活用することで、Vim エディタ上で任意のプログラムのテスト・コンパイルと実行が可能です。

何時でも再実行可能な形でノートに残せます。どのようにリンクして、どんな引数で実行するかまで残っています。

一行で完結している。それ以前の文脈の影響を受けない
import 済みのモジュールはディレクトリごとに sfCrrntIni.py で固定される
↑ if then else 構文の排除


kOde ← scipy.integrate.ode を one-liner で扱いやすいように修正したもの
↑ ode(..) は時刻引数を明示的に扱わねばならないなど、ワンライナーでは扱いにくい。

D=2;inV=[-0.97m`,0.243, 0.97,-0.243, 0,0, 1,1,  -0.466m`/s`,-0.432, -0.466,-0.432, 0.932,0.864, 0,0]; N=len(inV)//(2D) ; getFV=λ v,i,k:(λ r=krry(v[D k:D (k+1)])-krry(v[D i:D (i+1)]):r/norm(r)^3 if norm(r)!=0 else ~[0,0])(); sumFc=λ v,j:sum([getFV(v,j,k) for k in range(N) if j!=k]); fnc= λ *v: np.r_[v[D N:],(~[sumFc(v,j) for j in range(N)]).r]; mt=kOde(fnc,inV, 2 s`,400); cl=ClCplxColor(); for k in range(N):plotTrajectory(mt[:,D k:D (k+1)],color=cl.GetFltColor(exp(`i 2pi k/N)))
↑ if then else 構文が不必要

type _tmC.py
キーボードからマウスに手を移し変える、うざったい操作は一切ありません。
Eclips や VisualStudio といった統合環境では、小さなテスト・プログラムを大量に
###<name id="test name reference"></name> Examples

git push のような時間のかかる操作には ;a 操作を使うべきではありません。途中経過を表示できないからです。



