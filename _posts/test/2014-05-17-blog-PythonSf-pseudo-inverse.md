---
layout: post
category : wikipedia
tagline: "pysf wikipedia"
tags : [pysf, wikipedia]
---
{% include JB/setup %}

## 概要
numpy の pseudo inverse は昔読んだ generalized inverse とは少し異なっている。Rank の小さい行列、またゼロ行列さえも pseudo inverse が存在する。それらについての PythonSf 数値実験と検討を報告する。

## 擬似逆行列の定義と具体例
ここに [wikipedia](http://ja.wikipedia.org/wiki/擬似逆行列) に擬似逆行列の定義と解説が書かれてます。下に定義を再掲します。

<center><b>擬似逆行列の定義</b></center>

    行列 A に対し、A の随伴行列（複素共軛かつ転置行列）を A.d とするとき、以下の条件を満足する擬似逆行列 Apsd は常に存在し、また唯一つ定まる：

    条件 1: Apsd は広義可逆元である：
        A Apsd A == A,
        Apsd A Apsd == Apsd.

    条件 2: A Apsd および Apsd A はエルミート行列である：
        (A Apsd).d == A Apsd,
        (Apsd A).d == Apsd A.

大部分の方は上の定義を見せられても半分狐に摘まれたような気分でしょう。広義可逆元なんて初めて見る方が殆どでしょう。こういう時は具体例での数値実験で式の意味を明確にしましょう。幸い np.linalg.pinv(..) 関数が擬似逆行列を生成して返してくれます。なお np.linalg.pinv(..) 関数は np.ufunc ではないのですが、ClTensor インスタンスを引数に与えると ClTensor インスタンスの擬似逆行列を返してくれます。実質的に np.ufunc です。

<center><b>2x3 ランダム複素行列:A と その擬似逆行列:Apsd</b></center>
    seed(0); A=randn(3,2)+ `i randn(3,2); A
    ===============================
    [[ 1.76405235+0.95008842j  0.40015721-0.15135721j]
     [ 0.97873798-0.10321885j  2.24089320+0.4105985j ]
     [ 1.86755799+0.14404357j -0.97727788+1.45427351j]]
    ---- ClTensor ----

    seed(0); A=randn(3,2)+ `i randn(3,2); Apsd=np.linalg.pinv(A); Apsd
    ===============================
    [[ 0.23850346-0.14941741j  0.07430511-0.08088406j  0.20359156+0.05059478j]
     [ 0.06670345+0.11764575j  0.28298303-0.01310121j -0.15924128-0.11027123j]]
    ---- ClTensor ----

    seed(0); A=randn(3,2)+ `i randn(3,2); Apsd=np.linalg.pinv(A); Apsd A
    ===============================
    [[  1.00000000e+00 -4.16333634e-17j  -2.49800181e-16 +0.00000000e+00j]
     [  1.11022302e-16 +0.00000000e+00j   1.00000000e+00 -6.93889390e-17j]]
    ---- ClTensor ----

    # 上の三つの式の PythonSf Open 判コード
    seed(0); A=randn(3,2)+ `i*randn(3,2); A
    ===============================
    [[ 1.76405235+0.95008842j  0.40015721-0.15135721j]
     [ 0.97873798-0.10321885j  2.24089320+0.4105985j ]
     [ 1.86755799+0.14404357j -0.97727788+1.45427351j]]
    ---- ClTensor ----

    seed(0); A=randn(3,2)+ `i*randn(3,2); Apsd=np.linalg.pinv(A); Apsd
    ===============================
    [[ 0.23850346-0.14941741j  0.07430511-0.08088406j  0.20359156+0.05059478j]
     [ 0.06670345+0.11764575j  0.28298303-0.01310121j -0.15924128-0.11027123j]]
    ---- ClTensor ----

    seed(0); A=randn(3,2)+ `i*randn(3,2); Apsd=np.linalg.pinv(A); Apsd A
    ===============================
    [[  1.00000000e+00 -4.16333634e-17j  -2.49800181e-16 +0.00000000e+00j]
     [  1.11022302e-16 +0.00000000e+00j   1.00000000e+00 -6.93889390e-17j]]
    ---- ClTensor ----

3x2 行列の擬似逆行列は 2x3 行列です。Apsd A と A Apsd どちらの積演算も可能です。

Apsd A が「単位行列 [[1,0],[0,1]] になっていない」と言わないでください。実数のコンピュータ計算には誤差がつき物です。行列の prety print 関数 pp(..) を使えば、少し上の結果が見やすくなります。ただし六桁以下の小さな値は表示されません。また現在のところ pp(..) 関数は PythonSfOpen には移植できていません。

<center><b>pp(..) による Apsd A と A Apsc</b></center>
    seed(0); A=randn(3,2)+ `i randn(3,2); Apsd=np.linalg.pinv(A); pp(Apsd A);
    [[ 1, 0]
    ,[ 0, 1]]
    -------- pp --

    seed(0); A=randn(3,2)+ `i randn(3,2); Apsd=np.linalg.pinv(A); pp(A Apsd);
    [[           0.607191,  0.31918-0.120161j, 0.230665+0.262658j]
    ,[  0.31918+0.120161j,           0.703891, -0.10708-0.283986j]
    ,[ 0.230665-0.262658j, -0.10708+0.283986j,           0.688919]]
    -------- pp --

上の計算で A Apsd が Hermite になっていることに注意を向けておいてください。A Apsd を単位行列にできなくなった代償として特殊な Hermite 行列になります。

## 定義「条件 1」の確認
先の擬似逆行列の定義における「条件 1」が成り立つことを数値実験で確認してみましょう。

<center><b>定義「条件 1」の確認</b></center>
    seed(0); A=randn(3,2)+ `i randn(3,2); Apsd=np.linalg.pinv(A); (A Apsd A) ~== A
    ===============================
    True

    seed(0); A=randn(3,2)+ `i randn(3,2); Apsd=np.linalg.pinv(A); (Apsd A Apsd) ~== Apsd
    ===============================
    True

    # 上の二つの PythonSf Open 判コード
    seed(0); A=randn(3,2)+ `i*randn(3,2); Apsd=np.linalg.pinv(A); np.allclose(np.dot(A, np.dot(Apsd,A)), A)
    ===============================
    True
    seed(0); A=randn(3,2)+ `i*randn(3,2); Apsd=np.linalg.pinv(A); np.allclose(np.dot(Apsd, np.dot(A, Apsd)), Apsd)
    ===============================
    True

~== は PythonSf でのユーザー演算子拡張であり、customize.py の中で nearlyEq(..) 関数に割り振っています。現在のところプリプロセッサがユーザー演算子の優先順位を一律に高くしているので、丸括弧での優先順位指定が必須です。無駄に丸括弧が必要なデメリットよりも可読性を優先しています。

条件 1 の確認計算は PythonSf Open 判でも可能です。でも積演算子 * および np.dot(..) を追加し、 ~== ユーザー演算子のかわりに np.allclose(..) わねばなりません。この結果 PythonSf Open 判のコードは Python のコードと同じになってしまいました。でも可読性は悪化しています。

A Apsd または Apsd A が単位行列であれば (A Apsd A) ~== A と (Apsd A Apsd) ~== Apsd が成り立ちます。だから この二つの条件は擬似逆行列であるための必要条件とみなして良さそうです。

ちなみに擬似逆行列は行列式が 0 の逆行列が存在しない正方行列についても存在します。そのときにも (A Apsd A) ~== A と (Apsd A Apsd) ~== Apsd が成り立ちます。もちろん Apsd A, A Apsd は単位行列になりません。

<center><b>行列式が 0 のときの定義「条件 1」の確認</b></center>

    A =~[np.arange(3*3)].reshape(3,3); A
    ===============================
    [[ 0.  1.  2.]
     [ 3.  4.  5.]
     [ 6.  7.  8.]]
    ---- ClTensor ----

    # 行列式が 0
    A=~[np.arange(3*3).reshape(3,3)]; A.m_dtrm
    ===============================
    0.0

    # 擬似逆行列 Apsd
    A=~[np.arange(3*3).reshape(3,3)]; A.m_dtrm ; Apsd=np.linalg.pinv(A); pp(Apsd)
    [[  -0.555556, -0.166667,  0.222222]
    ,[ -0.0555556,         0, 0.0555556]
    ,[   0.444444,  0.166667, -0.111111]]
    -------- pp --
    ===============================
    None

    # Apsd A は単位行列にならない
    A=~[np.arange(3*3).reshape(3,3)]; A.m_dtrm ; Apsd=np.linalg.pinv(A); Apsd A
    ===============================
    [[ 0.83333333  0.33333333 -0.16666667]
     [ 0.33333333  0.33333333  0.33333333]
     [-0.16666667  0.33333333  0.83333333]]
    ---- ClTensor ----

    A=~[np.arange(3*3).reshape(3,3)]; A.m_dtrm ; Apsd=np.linalg.pinv(A); (A Apsd) ~== (Apsd A)
    ===============================
    True

    # でも  (A Apsd A) ~== A は成り立つ
    A=~[np.arange(3*3).reshape(3,3)]; A.m_dtrm ; Apsd=np.linalg.pinv(A); (A Apsd A) ~== A
    ===============================
    True

    # でも  (Apsd A Apsd) ~== Apsd も成り立つ
    A=~[np.arange(3*3).reshape(3,3)]; A.m_dtrm ; Apsd=np.linalg.pinv(A); (Apsd A Apsd) ~== Apsd
    ===============================
    True

## 定義「条件2」の確認
先の擬似行列の定義における「条件 2」が成り立つことも数値実験で確認してみましょう。

<center><b>定義「条件 2」の確認</b></center>
    seed(0); A=randn(2,3)+ `i randn(2,3); Apsd=np.linalg.pinv(A); (A Apsd).d ~== (A Apsd)
    ===============================
    True

    seed(0); A=randn(2,3)+ `i randn(2,3); Apsd=np.linalg.pinv(A); (Apsd A).d ~== (Apsd A)
    ===============================
    True

    # 上の二つの式の PythonSf Open 判コード
    seed(0); A=randn(2,3)+ `i*randn(2,3); Apsd=np.linalg.pinv(A); np.allclose(np.dot(A, Apsd).conj().T, np.dot(A, Apsd))
    ===============================
    True

    seed(0); A=randn(2,3)+ `i*randn(2,3); Apsd=np.linalg.pinv(A); np.allclose(np.dot(Apsd, A).conj().T, np.dot(Apsd, A))
    ===============================
    True

<center><b></b></center>
    # rank 2 の 3x2 行列のとき、pseudo inverse は (A.d A)^-1 A.d で作れる
    seed(0); A=randn(3,2)+ `i randn(3,2); (A.d A)^-1 A.d
    ===============================
    [[ 0.23850346-0.14941741j  0.07430511-0.08088406j  0.20359156+0.05059478j]
     [ 0.06670345+0.11764575j  0.28298303-0.01310121j -0.15924128-0.11027123j]]
    ---- ClTensor ----

    seed(0); A=randn(3,2)+ `i randn(3,2); (A.d (A A.d)^-1) ~== np.linalg.pinv(A)
    ===============================
    True

     
    # rank 2 の 3x2 行列のとき、pseudo inverse  以外にも Ax A == E となる Ax が存在する
    seed(0); A=randn(3,2)+ `i randn(3,2); (A.t A)^-1 A.t
    ===============================
    [[ 0.19083191+0.01484792j  0.06279178-0.22894183j  0.34142385-0.01398845j]
     [ 0.10843294-0.15421935j  0.33223968+0.22056979j -0.36553262+0.02146981j]]
    ---- ClTensor ----

    seed(0); A=randn(3,2)+ `i randn(3,2); ((A.t A)^-1 A.t) A
    ===============================
    [[  1.00000000e+00 +9.71445147e-17j   0.00000000e+00 +6.59194921e-17j]
     [  1.11022302e-16 +6.93889390e-17j   1.00000000e+00 -5.20417043e-17j]]
    ---- ClTensor ----

    # 正方行列のときは ((A.t A)^-1 A.t) == ((A.d A)^-1 A.d)
    seed(0); A=randn(2,2)+ `i randn(2,2); ((A.t A)^-1 A.t) ~== ((A.d A)^-1 A.d)
    ===============================
    True

## 正方ゼロ行列、行列式==0 の行列での擬似逆行列
全要素やが 0 の正方ゼロ行列や、行列式==0 の行列にも擬似逆行列は存在します。具体的に Python に計算させてみましょう。
<center><b>正方ゼロ行列、行列式==0 の行列での擬似逆行列</b></center>
    mt=kzrs(2,2); np.linalg.pinv(mt) 
    ===============================
    [[ 0.  0.]
     [ 0.  0.]]
    ---- ClTensor ----

    mt=kzrs(2,2); mt[0,1]=1; np.linalg.pinv(mt) 
    ===============================
    [[ 0.  0.]
     [ 1.  0.]]
    ---- ClTensor ----

    mt=kzrs(2,2); mt[0,1]=1; np.linalg.pinv(mt) mt
    ===============================
    [[ 0.  0.]
     [ 0.  1.]]
    ---- ClTensor ----

    # 上の三式の PythonSf Open 判コード
    mt=np.zeros([2,2]); np.linalg.pinv(mt) 
    ===============================
    [[ 0.  0.]
     [ 0.  0.]]

    mt=np.zeros([2,2]); mt[0,1]=1; np.linalg.pinv(mt) 
    ===============================
    [[ 0.  0.]
     [ 1.  0.]]
