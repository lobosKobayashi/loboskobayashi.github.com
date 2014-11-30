---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [pysf, tutorial]
title : PythonSf を使って project Euler 92 問題を解く
---
{% include JB/setup %}

##■■ 概要
PythonSf は計算に特化した scripting language です。最小の手間で計算できるように作ってあります。多くの計算が one-liner で実行できてしまいます。

One-liner は一行だけで完結しています。このような one-liner は、抽象的な数学的思考・検討の積み重ねを強力にバックアップします。誤った筋道に入り込むことを、具体例での確認で防ぎます。一行の copy and past で動いてしまう one-liner は、似た考えを繰り返す試行錯誤の過程を強力に支えてくれます。この意味で PythonSf の one-liners は数学的な考察・検討を補助するツールとも言えます。

このような PythonSf one-liners を Project Euler 92 に適用してみます。この過程で PythonSf one-liner が考察・検討の補助ツールとして働く様子を見てください。

なお以下の計算は PythonSf version 0.91b / Python 2.7.2 で動作確認しています。


##■■ PythonSf て何？ 何ができるの？

PythonSf は最小の手間で計算し、またグラフを描かせるためのソフトウェア・ツールです。数学のための scripting language とも言えます。

計算させる・グラフを描かせる元になる式を「PythonSf 式」と呼んでいます。PythonSf 式は Python 文法に upper compatible であり、既存の膨大な Python package 群を利用できます。同時に、エディタで書ける数式メモに近い PythonSf 式記述もできます。エディタで書ける数式メモが最小の手間で数式を入力する方法だからです。例えば数式で頻出するギリシャ文字:α,β,γ, ... , ω, Α,Β,Γ, ... , Ω を、 PythonSf 式では漢字で記述できるようにしています。また Python の名前空間を拡張して、変数名の前後に backquote を追加できるようにして名前空間を広げています。これにより短いけれど既存の Python 変数と衝突しない数学向けの PythonSf 変数を使えるようにしています。あと numpy パッケージは頻繁に利用されるので、import 済みにしてあり np の名前を割り当ててあります。

例えば \\(arccosh(\pi/2)\\) の値は下のように計算させられます。
数学では円周率 \\(\pi\\):np.pi 変数の使用頻度は高いので global 変数空間に pi の別名を与えています。

    np.arccosh(pi/2)
    ===============================
    1.02322747855

また \\(\sin(x^2+2x+3)\\) 関数の \\(\pi/4\\) での値は下のように計算させられます。基本数値関数 exp, sin, cos, tan, sinh, cosh, tanh, arcsin, arccos, arctan, log10, sqrt は使用頻度が高いので、numpy 名前空間から global な名前空間に移しています。そのとき、これらの基本関数を関数のままで加減乗除べき乗算と関数合成が可能な、より高機能なものに修飾し直しています。また `X も 加減乗除べき乗算と関数合成が可能な高等関数なので、下のように実際に計算可能な数式メモに近い PythonSf 式で記述できます。

    x=`X; sin(x^2+2x+3)(pi/4)
    ===============================
    -0.889174871907

PythonSf 自体は CUI:コマンド・ライン・ツールですが、エディタに組み込んで使います。そして計算の元になる PythonSf 式文字列はエディタから与えます。これにより思考過程での考察のメモ書きと数式のメモ書きを、一つのエディタ上で完結させられます。

具体的な PythonSf での計算の様子を [PythonSf introduction 1/3](http://www.youtube.com/watch?v=rdo-46WafyQ), [PythonSf introduction 2/3](http://www.youtube.com/watch?v=O_0gW0ti0Ek), [PythonSf introduction 3/3](http://www.youtube.com/watch?v=s4FwqLcmHWM) で確認ください。また詳細説明が[こちら](http://loboskobayashi.github.io/pysf/manual/one-liners.htm)にあります。

<center><b>Open 判と commercial 判</b></center>
>PythonSf には Open 判と commercial 判があります。無償で配布している PythonSf には commercial 判も入っています。その機能は全て有償版と同様に働きます。ただし実行の開始時に 5 秒のディレーが含まれます。Open 判ならば 5 秒のディレーは入りません。ただしプリプロセッサ機能が貧弱なため、省略された積演算子のを補完できません。逆に言えば Python 文法の範囲内で使っている分には Open 判で十分です。詳細な commercial 判と open 判の違いは、[ここを](http://loboskobayashi.github.io/pysf/manual/one-liners.htm#%E2%96%A0%E2%96%A0%20PythonSf%20Open) 参照願います。

    # Open 判での PythonSf 式
    x=`X; sin(x^2+2*x+3)(pi/4)
    ===============================
    -0.889174871907


以下、このような PythonSf を Project Euler の問題 92 に適用してみます。そこでの PythonSf one-liner の思考ツールとしての働きを見てやってください。

##■■ PythonSf を Project Euler 92 問題で試す

[Project Euler 95](http://projecteuler.net/problem=92)の問題を下に示します。

<center><b>Project Euler 92 の問題</b></center>
>A number chain is created by continuously adding the square of the digits in a number to form a new number until it has been seen before.
>
>For example,
>
>   44 → 32 → 13 → 10 → 1 → 1
>
>   85 → 89 → 145 → 42 → 20 → 4 → 16 → 37 → 58 → 89
>
>Therefore any chain that arrives at 1 or 89 will become stuck in an endless loop. What is most amazing is that EVERY starting number will eventually arrive at 1 or 89.
>
>
>How many starting numbers below ten million will arrive at 89?

要は下のことを行えという問題です。

1. 数値を 10進数表記した各桁の自乗和を取れ
2. 上の操作を繰返して数列を作れ
3. その数列必ず 1 になるか、または [89,145,42,20,4,16,37,58] の繰り返しになる
3. 10^7 までの数値のうち、数列が [89,145,42,20,4,16,37,58] の繰り返しになるものの個数を求めよ

二種類の結果にしかならないのが意外です。

###■ Project Euler 92 問題の把握

まず最初に行うことは問題の把握です。それには具体例を作って数値実験をしてみるのが最適です。PythonSf なら簡単です。下のような具合です。

"数値を 10進数表記した各桁の自乗和を取れ" は下の PythonSf 式で計算できます。

    inAt=44; sum(int(ch)^2 for ch in str(inAt))
    ===============================
    32

10進数の各桁の値を取り出すのに str(inAt) と文字列に変換しています。イタレータ内包表記で その文字列を ch に取り出し、int(ch)^2 と自乗値を作っています。自然言語での "数値を 10進数表記した各桁の自乗和を取れ" をコンピュータで実行可能な、あいまい性の全くない sum(int(ch)^2 for ch in str(inAt)) に置き換えています。Python 文法になれた方ならば、PythonSf 式のほうが分かりやすいと思います。いかがでしょうか。

なお 上の PythonSf 式では自乗演算子に ^ を使っています。もちろん Python 文法に従った ** べき乗演算子を使うことも可能です。Python 文法では ^ は bit xor の意味なのは承知しています。数学の世界では ^ がべき乗演算子であることを優先しました。PythonSf 式で bit xor 演算を行うためには \^ 記号を使います。

    3 \^ 4
    ===============================
    7

"上の操作を繰返して数列を作れ" を下のように行えます

    inAt=32; sum(int(ch)^2 for ch in str(inAt))
    ===============================
    13

    inAt=13; sum(int(ch)^2 for ch in str(inAt))
    ===============================
    10

    inAt=10; sum(int(ch)^2 for ch in str(inAt))
    ===============================
    1

でも、上の計算は冗長すぎます。一度に数列を出してしまう PythonSf 式が欲しくなります。下のように記述できます。

<center><b>Project Euler 92 の問題を計算する PythonSf 式</b></center>
    N,ls=10,[44]; for _ in range(N):ls.append(sum(int(ch)^2 for ch in str(ls[-1]))); ls
    ===============================
    [44, 32, 13, 10, 1, 1, 1, 1, 1, 1, 1]

    N,ls=10,[85]; for _ in range(N):ls.append(sum(int(ch)^2 for ch in str(ls[-1]))); ls
    ===============================
    [85, 89, 145, 42, 20, 4, 16, 37, 58, 89, 145]

Python 文法では、一つの式 または 一つの文だけなら for ループの後に改行することなく続けて記述できます。逆に one-liner で記述するためには for loop の後には一つの式か文だけしか書けません。そのために上のような global な ls 変数と ls.append(..) を使ったコードを使います。上のコードの意味だけならば one-liner を使わずに下のブロック・コードのように書くべきです。でも one-liner の良さ:copy and paste の容易さを享受するためには、上のようなコードを書くべきです。この後でのように、数列を作って それをまた利用するコードを書くのならば、上のコードも悪くないでしょう。可読性の悪化も少しで済んでいます。他人にも読んでもらえる可読性を保っています。我々も Perl でのような可読性の悪い one-liners を使うぐらいなら、下のようなブロック・コードを書くべきだと主張します。

    //@@
    N = 10

    inAt = 44

    for _ in range(N):
        print(inAt),
        inAt = sum(int(ch)**2 for ch in str(inAt))
    //@@@
    44 32 13 10 1 1 1 1 1 1

上のコードで N==10 の数列の長さを天下りで与えてしまうのが気に入りません。1 から 89,.. の繰り返しどちらかになるまでの回数は分かっていないのですから N=10 では小さすぎるかもしれません。なので 1 か 89 がでるまでの数列を見つけ出すようなコードが欲しくなります。Python には、itertools があり、そのような目的で使えます。下のような具合です。

    ls=[44]; f=λ _:ls.append(sum(int(ch)^2 for ch in str(ls[-1]))) or (not ls[-1] in (1,89)); tn.takewhile(f,tn.count())[:1000]; ls
    ===============================
    [44, 32, 13, 10, 1]

PythonSf では itertools パッケージの関数を tlRcGn.py モジュール内で呼び出してあります。tlRcGn モジュールは tn の名前で import 済みになっており、上のような PythonSf 式が記述可能になっています。あと itertools の count(.) や takewhile(..) などは generator を返すだけなのですが、tlRcGn.py の中で それらに \__getitem__(..) method を追加して [..] によるインデックス操作を可能にしています。

また上の PythonSf 式で lambda 関数に λ:ギリシャ文字の漢字を使えていることも注目ください。もちろん Python 文法での lambda も使えるのですが、漢字の λ のほうが可読性が高まります。pysf.vim/pysf.el でのようにギリシャ文字への変換マクロを作ってやれば、漢字 λ の入力にともなう IME on/off の煩わしさもありません。

上の PythonSf 式まで到達できたならば、10^7-100 から 10^7 までの数値に対して 1 または 89 に到達するまでの数列をプリントするような PythonSf 式も容易に導けます。下の PythonSf 式では、PythonSf で宣言済みのグローバルな Bf バッファ変数を使っています。リスト内包表記では一つの式しか書けません。一方で Python には let 文がありません。リスプでの progn(...) のような逐次処理の構文もありません。それの対策として、グローバルな Bf クラス・インスタンスのキーワード引数つき呼び出しによって let 文と同様なことを可能にしています。 また Bf(..) let 文と タプル (, ..., ) の組合せによって progn(...) のような逐次処理を可能にしています。

    f=λ _:Bf.ls.append(sum(int(ch)^2 for ch in str(Bf.ls[-1]))) or (not Bf.ls[-1] in (1,89)); `print([(Bf(ls=[k]), tn.takewhile(f,tn.count())[:1000],Bf.ls)[-1] for k in range(10^7-100,10^7)])
    [[9999900, 405, 41, 17, 50, 25, 29, 85, 89],
     [9999901, 406, 52, 29, 85, 89],
     [9999902, 409, 97, 130, 10, 1],
     [9999903, 414, 33, 18, 65, 61, 37, 58, 89],
     [9999904, 421, 21, 5, 25, 29, 85, 89],
     [9999905, 430, 25, 29, 85, 89],
     [9999906, 441, 33, 18, 65, 61, 37, 58, 89],
     [9999907, 454, 57, 74, 65, 61, 37, 58, 89],
     [9999908, 469, 133, 19, 82, 68, 100, 1],
     [9999909, 486, 116, 38, 73, 58, 89],
     [9999910, 406, 52, 29, 85, 89],
     [9999911, 407, 65, 61, 37, 58, 89],
     [9999912, 410, 17, 50, 25, 29, 85, 89],
     [9999913, 415, 42, 20, 4, 16, 37, 58, 89],
     [9999914, 422, 24, 20, 4, 16, 37, 58, 89],
     [9999915, 431, 26, 40, 16, 37, 58, 89],
     [9999916, 442, 36, 45, 41, 17, 50, 25, 29, 85, 89],
     [9999917, 455, 66, 72, 53, 34, 25, 29, 85, 89],
     [9999918, 470, 65, 61, 37, 58, 89],
     [9999919, 487, 129, 86, 100, 1],
     [9999920, 409, 97, 130, 10, 1],
     [9999921, 410, 17, 50, 25, 29, 85, 89],
     [9999922, 413, 26, 40, 16, 37, 58, 89],
     [9999923, 418, 81, 65, 61, 37, 58, 89],
     [9999924, 425, 45, 41, 17, 50, 25, 29, 85, 89],
     [9999925, 434, 41, 17, 50, 25, 29, 85, 89],
     [9999926, 445, 57, 74, 65, 61, 37, 58, 89],
     [9999927, 458, 105, 26, 40, 16, 37, 58, 89],
     [9999928, 473, 74, 65, 61, 37, 58, 89],
     [9999929, 490, 97, 130, 10, 1],
     [9999930, 414, 33, 18, 65, 61, 37, 58, 89],
     [9999931, 415, 42, 20, 4, 16, 37, 58, 89],
     [9999932, 418, 81, 65, 61, 37, 58, 89],
     [9999933, 423, 29, 85, 89],
     [9999934, 430, 25, 29, 85, 89],
     [9999935, 439, 106, 37, 58, 89],
     [9999936, 450, 41, 17, 50, 25, 29, 85, 89],
     [9999937, 463, 61, 37, 58, 89],
     [9999938, 478, 129, 86, 100, 1],
     [9999939, 495, 122, 9, 81, 65, 61, 37, 58, 89],
     [9999940, 421, 21, 5, 25, 29, 85, 89],
     [9999941, 422, 24, 20, 4, 16, 37, 58, 89],
     [9999942, 425, 45, 41, 17, 50, 25, 29, 85, 89],
     [9999943, 430, 25, 29, 85, 89],
     [9999944, 437, 74, 65, 61, 37, 58, 89],
     [9999945, 446, 68, 100, 1],
     [9999946, 457, 90, 81, 65, 61, 37, 58, 89],
     [9999947, 470, 65, 61, 37, 58, 89],
     [9999948, 485, 105, 26, 40, 16, 37, 58, 89],
     [9999949, 502, 29, 85, 89],
     [9999950, 430, 25, 29, 85, 89],
     [9999951, 431, 26, 40, 16, 37, 58, 89],
     [9999952, 434, 41, 17, 50, 25, 29, 85, 89],
     [9999953, 439, 106, 37, 58, 89],
     [9999954, 446, 68, 100, 1],
     [9999955, 455, 66, 72, 53, 34, 25, 29, 85, 89],
     [9999956, 466, 88, 128, 69, 117, 51, 26, 40, 16, 37, 58, 89],
     [9999957, 479, 146, 53, 34, 25, 29, 85, 89],
     [9999958, 494, 113, 11, 2, 4, 16, 37, 58, 89],
     [9999959, 511, 27, 53, 34, 25, 29, 85, 89],
     [9999960, 441, 33, 18, 65, 61, 37, 58, 89],
     [9999961, 442, 36, 45, 41, 17, 50, 25, 29, 85, 89],
     [9999962, 445, 57, 74, 65, 61, 37, 58, 89],
     [9999963, 450, 41, 17, 50, 25, 29, 85, 89],
     [9999964, 457, 90, 81, 65, 61, 37, 58, 89],
     [9999965, 466, 88, 128, 69, 117, 51, 26, 40, 16, 37, 58, 89],
     [9999966, 477, 114, 18, 65, 61, 37, 58, 89],
     [9999967, 490, 97, 130, 10, 1],
     [9999968, 505, 50, 25, 29, 85, 89],
     [9999969, 522, 33, 18, 65, 61, 37, 58, 89],
     [9999970, 454, 57, 74, 65, 61, 37, 58, 89],
     [9999971, 455, 66, 72, 53, 34, 25, 29, 85, 89],
     [9999972, 458, 105, 26, 40, 16, 37, 58, 89],
     [9999973, 463, 61, 37, 58, 89],
     [9999974, 470, 65, 61, 37, 58, 89],
     [9999975, 479, 146, 53, 34, 25, 29, 85, 89],
     [9999976, 490, 97, 130, 10, 1],
     [9999977, 503, 34, 25, 29, 85, 89],
     [9999978, 518, 90, 81, 65, 61, 37, 58, 89],
     [9999979, 535, 59, 106, 37, 58, 89],
     [9999980, 469, 133, 19, 82, 68, 100, 1],
     [9999981, 470, 65, 61, 37, 58, 89],
     [9999982, 473, 74, 65, 61, 37, 58, 89],
     [9999983, 478, 129, 86, 100, 1],
     [9999984, 485, 105, 26, 40, 16, 37, 58, 89],
     [9999985, 494, 113, 11, 2, 4, 16, 37, 58, 89],
     [9999986, 505, 50, 25, 29, 85, 89],
     [9999987, 518, 90, 81, 65, 61, 37, 58, 89],
     [9999988, 533, 43, 25, 29, 85, 89],
     [9999989, 550, 50, 25, 29, 85, 89],
     [9999990, 486, 116, 38, 73, 58, 89],
     [9999991, 487, 129, 86, 100, 1],
     [9999992, 490, 97, 130, 10, 1],
     [9999993, 495, 122, 9, 81, 65, 61, 37, 58, 89],
     [9999994, 502, 29, 85, 89],
     [9999995, 511, 27, 53, 34, 25, 29, 85, 89],
     [9999996, 522, 33, 18, 65, 61, 37, 58, 89],
     [9999997, 535, 59, 106, 37, 58, 89],
     [9999998, 550, 50, 25, 29, 85, 89],
     [9999999, 567, 110, 2, 4, 16, 37, 58, 89]]
    ===============================
    None

上の 10^7-100, 10^7 までの数値に対する数列を眺めていると色々なことが分かります。
1. 確かに全ての数列が 1 または 89.. からの繰り返しになりそうです。1,89 どちらにも到達しない数値があったとき、上の PythonSf 式は無限ループに陥ります。(実際には tlRcGn モジュール内の __getitem__() method の中で 10^6 回以上の繰り返しになったとき IndexError 例外を発生させます。)
1. 数列が 1 になるのは途中で 10 や 100 などの値になったときです。確かに 10^n の値になった後、その各桁の値の自乗和は 1 になります。
3. 9999999 のような大きな値でも、その各桁の値の自乗和は 567 と小さな値になります。逆に 567 までの値について、その数列が 1 出終わる数値を選び出してリストを作ってやれば、10^7 までの数値について、数列が 89 になる数値の個数を高速に数え上げられそうです。

<center><b>Bf についての詳細な説明</b></center>
>先の Bf の説明だけでは、分からない部分が残った方も多いと思います。そのようなときは info(type(Bf)), source(type(Bf)) Pythn 式を実行してみることを勧めます。( ここで info,source は np.info, np.source 関数と同じ物です。これらは使用頻度が高いので global 変数に昇格させてあります。) この二つから findstr /n ClBuffer pysf\\*.py と grep を行うことも容易に思いつくでしょう。
>
>PythonSf は commercial 判でも大部分のソースを、Open 判では全部のソースを公開しています。一方で Python は可読性の高い言語です。下手な説明を読んでいるよりソースを直接読んだほうが、その働きを理解することが簡単なことが珍しくありません。Bf についても、それが成り立つと思います。

    info(type(Bf))
     ClBuffer(**dctAg)

    # date:2014/05/25 Constructor holds read-only variable and __call__ holds
    #   instantanious variable. We implemented this class for one-liners to hold
    #   temporal variables.
     
    Methods:

    ===============================
    None


    source(type(Bf))
    In file: pysf\customize.py

    class ClBuffer(object):
        """' date:2014/05/25 Constructor holds read-only variable and __call__ holds
            instantanious variable. We implemented this class for one-liners to hold
            temporal variables.
        '"""
        def __init__(self, **dctAg):
            if dctAg == {}:
                self.__dict__["m_dctReadOnly"] = {}
            else:
                self.__dict__["m_dctReadOnly"] = dctAg

            #self.m_dctBuffer={}
            self.__dict__["m_dctBuffer"] = {}

        def __call__(self, **dctAg):
            for k in dctAg:
                if k!='_' and self.m_dctReadOnly != {}:
                    assert False, ("You assigned a keyward argument:"+str({k:dctAg[k]})
                                  +" for a read only ClBuffer instance") 

                else:
                    for k in dctAg:
                        self.m_dctBuffer[k] = dctAg[k]

                return self.m_dctBuffer[sorted(dctAg)[-1]]

        def __getattr__(self, name):
            #print "debug--name:" + str(name)
            if name in self.m_dctReadOnly:
                return self.m_dctReadOnly[name]
            # yet implemented for _._. .. read only hierarchy
            elif name in self.m_dctBuffer:
                return self.m_dctBuffer[name]
            else:
                assert False, "In ClBuffer, there is no variable:" + name
            return self.__dict__[name]

        def __setattr__(self, name, value):

            assert False , ("Don't assign for ClBuffer members. " + name
                          + " = " + str(value) )

    ===============================
    None

    findstr /n ClBuffer pysf\*.py
    pysf\customize.py:1286:class ClBuffer(object):
    pysf\customize.py:1310:                              +" for a read only ClBuffer instance") 
    pysf\customize.py:1332:            assert False, "In ClBuffer, there is no variable:" + name
    pysf\customize.py:1337:        assert False , ("Don't assign for ClBuffer members. " + name
    pysf\customize.py:1342:Bf = ClBuffer() 

###■ Project Euler 92 問題の解
10^7 までの整数は一回目の各桁自乗和で 567 以下の小さな整数に収まります。ですから、567 までで自乗和が 1 に収束する整数全てのリストを作り出してやれば、自乗和がそのリストに含まれないことで、 89 以下の繰り返し数列に辿り着く整数と判定できます。以下の PythonSf 式で、そのようなリストを生成できます。

    Bf(ls=[1]); f=λ _:Bf.ls.append(sum(int(ch)^2 for ch in str(Bf.ls[-1]))) or (not Bf.ls[-1] in (1,89)); lsLs=[(Bf(ls=[k]), tn.takewhile(f,tn.count())[:1000],Bf.ls)[-1] for k in range(1,567+1)]; lsEnd1:=[ ls[0] for ls in lsLs if ls[-1]==1]
    ===============================
    [1, 7, 10, 13, 19, 23, 28, 31, 32, 44, 49, 68, 70, 79, 82, 86, 91, 94, 97, 100, 103, 109, 129, 130, 133, 139, 167, 176, 188, 190, 192, 193, 203, 208, 219, 226, 230, 236, 239, 262, 263, 280, 291, 293, 301, 302, 310, 313, 319, 320, 326, 329, 331, 338, 356, 362, 365, 367, 368, 376, 379, 383, 386, 391, 392, 397, 404, 409, 440, 446, 464, 469, 478, 487, 490, 496, 536, 556, 563, 565, 566]

<center><b>ファイル変数</b></center>
>上の PythonSf 式でファイル変数を生成する := 演算子を使っています。この演算子を使うことでカレント・ディレクトリにピックルス・ファイルを生成します。逆にファイル変数を読み出すのには =: lsEnd1 のように =: 演算子を使います。ファイル変数を使うことで、上の式のように長い PythonSf 式をファイル変数読み出し操作のように短くできます。
>
>残念ながら今のところ Open 判では :=, =: 演算子を使えません。上の PythonSf 式は commercial 判でしか働きません。無償 commertial 判は 5 秒のディレーを耐えるか、長い PythonSf 式を使うかのどちからを選択しなければなりません。

lsEndl ファイル変数を使えば、1000, 10000 までの整数で 89以下の繰り返しに辿り着く整数の個数は以下の PythonSf 式で計算できます。

    N=10^3; =:lsEnd1; f=λ n:sum(int(ch)^2 for ch in str(n)); f(10^7-1); sum(1 for n in range(1,N) if not(f(n) in lsEnd1))
    ===============================
    857
    N=10^4; =:lsEnd1; f=λ n:sum(int(ch)^2 for ch in str(n)); f(10^7-1); sum(1 for n in range(1,N) if not(f(n) in lsEnd1))
    ===============================
    8558

上の PythonSf 式は正しく動いています。ならば N の値を 10^7 にするだけで 10,000,000 までの整数で 89.. 以下の繰り返しになる整数の数を下の PythonSf 式で計算できるはずです。

    N=10^7; =:lsEnd1; f=λ n:sum(int(ch)^2 for ch in str(n)); sum(1 for n in range(1,N) if not(f(n) in lsEnd1))
    ===============================
    8581146

上の答えをえるための計算時間は ThinkPad E420 Inter Core i7 で 80秒ぐらいで済みました。int(ch)^2 for ch in str(n) 部分に高速化する余地がありそうです。でも そこまで高速化に拘る気になれません。

なお上と同じ計算を行う Open 判での PythonSf 式は下のようになります。

    Bf(ls=[1]); f=λ _:Bf.ls.append(sum(int(ch)^2 for ch in str(Bf.ls[-1]))) or (not Bf.ls[-1] in (1,89)); lsLs=[(Bf(ls=[k]), tn.takewhile(f,tn.count())[:1000],Bf.ls)[-1] for k in range(1,567+1)]; lsEnd1=[ ls[0] for ls in lsLs if ls[-1]==1]; N=10^7; f=λ n:sum(int(ch)^2 for ch in str(n)); sum(1 for n in range(1,N) if not(f(n) in lsEnd1))


###■ 思考ツーとしての PythonSf one-liners

Project Euler 92 問題の回答に到達するまでに、下の PythonSf 式を経過しています。これらの順序を経なければ、最後の回答にまで到達できません。この過程は思考過程そのものです。理系の問題の考察の多くは数式で行われます。自然言語では曖昧すぎ･冗長すぎるからです。式の間では自然言語で考えいますが、要の部分は PythonSf 式で考えています。

    inAt=44; sum(int(ch)^2 for ch in str(inAt))

    inAt=32; sum(int(ch)^2 for ch in str(inAt))

    N,ls=10,[44]; for _ in range(N):ls.append(sum(int(ch)^2 for ch in str(ls[-1]))); ls

    N,ls=10,[85]; for _ in range(N):ls.append(sum(int(ch)^2 for ch in str(ls[-1]))); ls

    f=λ _:Bf.ls.append(sum(int(ch)^2 for ch in str(Bf.ls[-1]))) or (not Bf.ls[-1] in (1,89)); `print([(Bf(ls=[k]), tn.takewhile(f,tn.count())[:1000],Bf.ls)[-1] for k in range(10^7-100,10^7)])

    Bf(ls=[1]); f=λ _:Bf.ls.append(sum(int(ch)^2 for ch in str(Bf.ls[-1]))) or (not Bf.ls[-1] in (1,89)); lsLs=[(Bf(ls=[k]), tn.takewhile(f,tn.count())[:1000],Bf.ls)[-1] for k in range(1,567+1)]; lsEnd1:=[ ls[0] for ls in lsLs if ls[-1]==1]
    N=10^7; =:lsEnd1; f=λ n:sum(int(ch)^2 for ch in str(n)); sum(1 for n in range(1,N) if not(f(n) in lsEnd1))

この記事の元になった日常ノートには、Project Euler 92 を検討するために書いた、上の何倍もの PythonSf 式が残っています。この記事の下書きを書くとき、日常ノートから重要な式だけを copy and paste しています。One-liner は、その一行で完結しているので、一行だけの抜き出しコピーでも動作することを保障できます。だからこそ、少しばかり可読性の悪化があるにもかかわらず、one-liner での記述をしてきたわけです。

PythonSf の one-liners を強力に活用するためには、分野に合わせたカスタマイズが有効です。PythonSf は from sfCrrnt import * を実行した後に、 PythonSf 式を前処理によって作られた Python 式を実行します。 すなわち sfCrrntIni.py という名前のカレント・ディレクトリを記述した関数やクラス全てを import 済みとして PythonSf 式を記述できます。

標準配布の PythonSf には圏論向けの機能を実装してあります。[次の]() 記事で、この sfCrrntIni を前提に、圏論の入門的な事柄を論じます。ここで PythonSf one-liner が思考ツールである、より強力な例を示します。

