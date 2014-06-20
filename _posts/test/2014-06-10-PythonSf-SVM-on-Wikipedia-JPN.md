---
layout: post
category : Wikipedia
tagline: "Supporting tagline"
tags : [pysf, machine_learning]
---
{% include JB/setup %}
<script>
MathJax.Hub.Config({
    jax: ["input/TeX","output/HTML-CSS"],
    displayAlign: "left"
});
</script>
##■■ 概要
対象読者:大学理系数学の素養を持つ方たち N次元空間での平行・直交についての直感が働くレベルでの線形代数の知識を前提とする

PRML を読んでられない人に役立つと思う。

kernel magic は素晴らしい。こんなことを よく思いついたものだ。妬ましい気持ちにさえなる。

実際に電卓でのように具体例を動かせるのは、SVM の理解を容易にしてくれる

使い方ではなく原理
sklern の使い方は、私も まだよくわかっていない

lagrangian における極値の操作を理解している

未定係数法を代数的に理解している
↑ 未定係数法を使った手続きを暗記しているだけではない

y[i](w x[i] + b) - 1 の式を味わうには PythonSf が最適な道具
↑w x[i] ~== 1 for ∃i が(本質？眼目？ key point)

arg min(w,b) max(α>0){1/2 w^2 - Σα[k][y[k](w x[k]-b)-1]} の式も味がある
↑ この式を最初に発表した方は、「どうでい」と思って：自慢に思っているはず
    ↑ 多数の学者にとって一生のうち数回しか作り出せない式だろう

実際に数値実験できるツールがあることは、自分の理解による予想が確認できるので、理解の速度を格段に高めるのに

一度取り去った sinusoid:非線形制限を 再度 kernel magic の形式で取り込みなおしている

##■■ 前提
以下での計算は PythonSf がインストールされており、Support Vector Machine のための専用ディレクトリが用意されており、そこに下の sfCrrntIni.py ファイルが作られていることを前提とします。
<center><b>sfCrrntIni.py の中身</b></center>
    type sfCrrntIni.py
    """'
    '"""
    import sklearn.datasets as sds
    import sklearn.linear_model as slm
    import sklearn.tree as skt
    import sklearn.svm as svm
    import  sklearn.decomposition as sdc
    import sklearn.cross_validation as scv

    """'
    import pylab as plb
    '"""
    def mpl():
        import matplotlib as mpl
        import matplotlib.pyplot as plt
        import matplotlib.finance as mpf
        import pysf.sfFnctns as sf
        sf.__getDctGlobals().update(locals())

これにより、sklearn にある様々の機能を PythonSf one-liner で計算できてしまうようになります。svm.SVC(..) とかくだけで、Support Vector Machien を動かせてしまいます。 import sklearn.svm as svm を実行済みとして実行できてしまいます。

sklearn を学んだり検討したりするための計算の大部分が one-liner でできてしまう。

御自分向けに sfCrrntIni.py を変更・追加して使ってほしい。そして より高機能な sfCrrntIni.py を公開してもらえることを希望しています。

##■■ Machine Learning の薦め
たんなる流行以上のもの
後々まで実用的な道具として広く使われる

### PythonSf one-liner
sklearn モジュールを使った計算を電卓なみの手軽さで実行できる

##■■ Support Vector Machine て何？

- リスト・テスト
- リスト・テストothersMemoV.txt という私の日常メモ・ファイルに追加エディットの最中に突然書き込みできなくなり “E513: write error, c

##■■ Support vector machine -- Wikipedia
http://en.wikipedia.org/wiki/Support_vector_machine
↑ 日本語は情報が少なすぎ;;http://ja.wikipedia.org/wiki/サポートベクターマシン


##■■ 分離超平面: \\(w \cdot x - b = \pm 1\\) 

大部分の方は norm(w)==1 での考え方になれてしまっており \\(w \cdot x - b = \pm 1\\) の式に対して数学的な直感が働かないと思います。

この式は SVM の世界では何度でも出てきます。

分離具合が 1 に正規化されています。 w x[i_arg max] == 1 となります。

w,b,N=~[0.3,0],-0.5,  20; seed(0); ar=20 (rand(N,2)-0.5); lsDt:=[x for x in ar if (w x + b) > 1]+[x for x in ar if (w x + b) <-1]
===============================
[ClTensor([ 9.27325521, -2.33116962])
, ClTensor([ 5.83450076,  0.5778984 ])
, ClTensor([ 5.56313502,  7.40024296])
, ClTensor([ 9.57236684,  5.98317128])
, ClTensor([ 8.87496157,  3.63640598])
, ClTensor([-8.57927884, -8.25741401])
, ClTensor([-9.59563205,  6.65239691])
, ClTensor([-7.63451148,  2.79842043])
, ClTensor([-7.13293425,  8.89337834])
, ClTensor([-4.70888776,  5.48467379])
, ClTensor([-9.62420399,  2.35270994])
]

点Ａ（ ｘ0 ， ｙ0 ） から、直線 Ｌ ：　ａｘ＋ｂｙ＋ｃ＝０　に下ろした垂線の長さ ｄ は、
(a x0 + b y0 + c)/sqrt(a^2+b^2)

date:2014/06/03 (火) time:05:29
-----------------------------------
N=10; w,b=~[1,0], 1; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[1,0])]; f,g =λ b:min([y (w x + b) for x,y in lsData if y==1]),λ b:min([y (w x + b) for x,y in lsData if y==-1]); plotGr(f,-3,3); plotGr(g, -3,3, color=red)


試し \\(w \cdot x - b = \pm 1\\) です

\\[
\vec{w} \cdot x - b = \pm 1 \tag{1}
\\]

svm.LinearSVC(..)
svv.SVC(.)

> Neuro machine の w:weight, b:bias でしょう。
> Neuron simulation をやっていると、sinusoidal function の縛りを取り去ってリニアな世界での直感が働くようにしたくなることがよくあります。(sinusoidal function で back-propagation の計算を簡単にできるのは承知しているのですが)
> SVM は、Neuro machine での sinusoidal 制限を取り除いて出てきたものと解釈しています。Neuro 回路の閾値が±1 のところにある線形弁別機です

##■■ svm.SVC(..), svm.LinearSVC(..)

### svm.SVC(..) による support vector の計算
↑ マイナス符号と /2 が気に入らん
    ↑b:bias は？
        ↑ _intercept_ 
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2, clf._intercept_
===============================
(array([  2.49949115e-01,  -2.03541624e-04]), array([ 0.49983038]))

#### svm.SVC(..) support vector poiints

    =:lsDt;  clf=svm.SVC(degree=2,kernel='linear'); clf.fit(lsDt, [1]*5+[-1]*6); clf.support_vectors_
    ===============================
    [[-4.70888776  5.48467379]
     [ 5.83450076  0.5778984 ]
     [ 5.56313502  7.40024296]]

    =:lsDt; plotPt(lsDt[:5]); plotPt(lsDt[5:],color=red)

##### soft margin の有無による違いがあるか？

    dt,trgt=[(1,0),          (-1,0),(-1,1)        ], (1,    -1,-1   ); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;`print(clf.__dict__)
    {'C': 1.0,
     '_gamma': 0.0,
     '_impl': 'c_svc',
     '_intercept_': array([ 0.]),
     '_label': array([0, 1]),
     '_sparse': False,
     'cache_size': 200,
     'class_weight': None,
     'class_weight_': array([ 1.,  1.]),
     'classes_': array([-1,  1]),
     'coef0': 0.0,
     'degree': 3,
     'dual_coef_': array([[ 0.5, -0.5]]),
     'epsilon': 0.0,
     'fit_status_': 0,
     'gamma': 0.0,
     'intercept_': array([-0.]),
     'kernel': 'linear',
     'max_iter': -1,
     'n_support_': array([1, 1]),
     'nu': 0.0,
     'probA_': array([], dtype=float64),
     'probB_': array([], dtype=float64),
     'probability': False,
     'random_state': None,
     'shape_fit_': (3, 2),
     'shrinking': True,
     'support_': array([1, 0]),
     'support_vectors_': array([[-1.,  0.],
           [ 1.,  0.]]),
     'tol': 0.001,
     'verbose': False}
    ===============================
    None

    dt,trgt=[(1,0),(-0.1,0), (-1,0),(-1,1),(0.1,0)], (1,1,  -1,-1,-1); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;`print(clf.__dict__)
    {'C': 1.0,
     '_gamma': 0.0,
     '_impl': 'c_svc',
     '_intercept_': array([ 0.]),
     '_label': array([0, 1]),
     '_sparse': False,
     'cache_size': 200,
     'class_weight': None,
     'class_weight_': array([ 1.,  1.]),
     'classes_': array([-1,  1]),
     'coef0': 0.0,
     'degree': 3,
     'dual_coef_': array([[ 0.6,  1. , -0.6, -1. ]]),
     'epsilon': 0.0,
     'fit_status_': 0,
     'gamma': 0.0,
     'intercept_': array([-0.]),
     'kernel': 'linear',
     'max_iter': -1,
     'n_support_': array([2, 2]),
     'nu': 0.0,
     'probA_': array([], dtype=float64),
     'probB_': array([], dtype=float64),
     'probability': False,
     'random_state': None,
     'shape_fit_': (5, 2),
     'shrinking': True,
     'support_': array([2, 4, 0, 1]),
     'support_vectors_': array([[-1. ,  0. ],
           [ 0.1,  0. ],
           [ 1. ,  0. ],
           [-0.1,  0. ]]),
     'tol': 0.001,
     'verbose': False}
    ===============================
    None
    ↑ support_ 属性は support_vectors_ を指し示す整数インデックス
        ↑ β[i] や ξ[i] に相当する量はなさそう
            ↑ その量があっても、あまりうれしくなさそう
    dt,trgt=[(1,0),(-0.2,0), (-1,0),(-1,1),(0.1,0)], (1,1,  -1,-1,-1); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2, clf._intercept_
    ===============================
    (array([  4.99792002e-01,  -4.16000001e-04]), array([-0.00013867]))

<center><b>`print(..)</b></center>
>customize.py/customizeOp.py の中で prety print に割り振ってあります。 
>単なる print だと __???___ の属性も打ち出してしまい、ゴミだらけになってしまいます。
>
### svm.LinearSVC(..) 

    seed(0); dt,trgt=[(1,0), (-1,0),(-1,1)], (1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
    ===============================
    [[ 0.82105564 -0.10527819 -0.02105564]]
    ===============================
    [[ 0.82105564 -0.10527819 -0.02105564]]
    ↑ .fit(..) method は np.random を利用している

数を増やしてやると、それらしい値になる

    dt,trgt=[(3,0)      ]*9+[(-1,0),(-1,1)]*9, [1  ]*9+[-1,-1]*9; clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
    ===============================
    [[ 0.49357316 -0.02420283 -0.48087984]]

##■■ Soft margin
arg min {1/2 ||w||^2 + C Σξ[i]} で C はユーザーが適当だと定める givine constant
    w,ξ,b
         ↑ b によって w,ξの値は変わる

pysf.vim

digit data に適用すると、64次元空間に十個の超平面と intercept を設け、0,..,9 のデータを分類する

## ■■ 参考 URL

[1: scikit.learn手法徹底比較！ SVM編
        ](http://d.hatena.ne.jp/saket/20130212/1360656405)<a name="1">

## ■■ 付録・実際の私の計算
上で説明していることは、[Wikipedea の Support Vector Machine の説明](http://en.wikipedia.org/wiki/Support_vector_machine) を読み解いて格闘した結果です。ここの説明は密度が高く読み解くのは困難です。svm.SVC(..) 関数という実際に動作する関数が存在していなかったら、それを使って具体例での結果を数値実験できなかったら、ここの説明を理解できなかったと思います。

また svm.SVC(..) をワン･ライナーで計算できる PythonSf がなかったら、エディタ上での one-liner による考察の積み上げがなかったら、やはり この Wikipedea の説明を理解できなかったでしょう。

エディタ上での思考過程を・私の頭の悪さを曝け出すことにもなるのですが、その格闘のようす、日常メートに書いた Support Vector Machine に関連する部分を全て以下に示します。

普通の方は これ位の数値実験・試行錯誤を行わないと、この Wikipedea の説明は理解できないと、そして PythonSf one-liner がないと試行錯誤さえできないと思うのですが、いかがでしょうか。

<div id="div_1">
<p><input type="button" value="show the remaing part" style="WIDTH:150px"
   onClick="document.getElementById('div_2').style.display='block';
            document.getElementById('div_1').style.display='none'"></p>
</div>
<div id="div_2" style="display:none">
<p><input type="button" value="hide the remaining part" style="WIDTH:150px"
   onClick="document.getElementById('div_2').style.display='none';
            document.getElementById('div_1').style.display='block'"></p>
<pre>
-----------------------------------
* date:2014/06/02 (月) time:08:09
-----------------------------------
time:09:43

info(np.logspace)
np.logspace(0, 9,10)
===============================
[  1.00000000e+00   1.00000000e+01   1.00000000e+02   1.00000000e+03
   1.00000000e+04   1.00000000e+05   1.00000000e+06   1.00000000e+07
   1.00000000e+08   1.00000000e+09]

=:digits; X,y=digits.data, digits.target; cv=scv.ShuffleSplit(X.shape[0],n_iter=3,test_size=0.1,random_state=0); [(Bf(svc=svm.SVC(kernel='rbf',C=100, gamma=0.001).fit(X[train],y[train])),Bf.svc.score(X[train],y[train]), Bf.svc.score(X[test],y[test]),len(y[test]))[1:] for train,test in cv]
===============================
[(1.0, 0.98888888888888893, 180), (1.0, 0.99444444444444446, 180), (1.0, 0.99444444444444446, 180)]

=:digits; X,y=digits.data, digits.target; cv=scv.ShuffleSplit(X.shape[0],n_iter=3,test_size=0.1,random_state=0); [(Bf(svc=svm.SVC(kernel='rbf',C=100, gamma=0.001).fit(X[train],y[train])),Bf.svc.score(X[train],y[train]), Bf.svc.score(X[test],y[test]),len(y[train]),len(y[test]))[1:] for train,test in cv]
===============================
[(1.0, 0.98888888888888893, 1617, 180), (1.0, 0.99444444444444446, 1617, 180), (1.0, 0.99444444444444446, 1617, 180)]

=:digits; X,y=digits.data, digits.target; X_train, X_test, y_train, y_test = scv.train_test_split(X,y,test_size=25,random_state=0); len(y_train), len(y_test)
===============================
(1772, 25)

=:digits; X,y=digits.data, digits.target; X_train, X_test, y_train, y_test = scv.train_test_split(X,y,test_size=180,random_state=0); cv=scv.ShuffleSplit(X.shape[0],n_iter=3,test_size=0.1,random_state=0); train,test=list(cv)[0]; y_test==test
↑ 180 個の選び出された値は異なる
=:digits; X,y=digits.data, digits.target; X_train, X_test, y_train, y_test = scv.train_test_split(X,y,test_size=180,random_state=0); cv=scv.ShuffleSplit(X.shape[0],n_iter=3,test_size=0.1,random_state=0); train,test=list(cv)[0]; y_test, test
===============================
(array([2, 8, 2, 6, 6, 7, 1, 9, 8, 5, 2, 8, 6, 6, 6, 6, 1, 0, 5, 8, 8, 7, 8,
       4, 7, 5, 4, 9, 2, 9, 4, 7, 6, 8, 9, 4, 3, 1, 0, 1, 8, 6, 7, 7, 1, 0,
       7, 6, 2, 1, 9, 6, 7, 9, 0, 0, 5, 1, 6, 3, 0, 2, 3, 4, 1, 9, 2, 6, 9,
       1, 8, 3, 5, 1, 2, 8, 2, 2, 9, 7, 2, 3, 6, 0, 5, 3, 7, 5, 1, 2, 9, 9,
       3, 1, 7, 7, 4, 8, 5, 8, 5, 5, 2, 5, 9, 0, 7, 1, 4, 7, 3, 4, 8, 9, 7,
       9, 8, 2, 6, 5, 2, 5, 8, 4, 8, 7, 0, 6, 1, 5, 9, 9, 9, 5, 9, 9, 5, 7,
       5, 6, 2, 8, 6, 9, 6, 1, 5, 1, 5, 9, 9, 1, 5, 3, 6, 1, 8, 9, 8, 7, 6,
       7, 6, 5, 6, 0, 8, 8, 9, 8, 6, 1, 0, 4, 1, 6, 3, 8, 6, 7]), array([1081, 1707,  927,  713,  262,  182,  303,  895,  933, 1266,  788,
       1410, 1239,    6,  223,  156, 1168,  458, 1061,  722,  513,  438,
       1015, 1567, 1135, 1320, 1661,  934, 1232,  971, 1181, 1719, 1480,
         18, 1360, 1001,  347,  745,  276,  107, 1363, 1431, 1368,  342,
       1522,  526, 1728,  402, 1270,  479,  425, 1382,  995,  459,  957,
        516,  457, 1556,  322,  175,  487,  567,  619,  124,  517, 1520,
        891,  412,  254, 1757, 1286,  649, 1182,   80, 1465,  249,  333,
        518, 1194,  118,  461, 1042,   34, 1002,  521,  489, 1779, 1259,
        485,  278, 1412,  381, 1498,   47,  191, 1200, 1777,   53, 1319,
       1664,  135, 1738,  152, 1203,  621, 1703,  467, 1000, 1708, 1595,
        217, 1281,  148, 1444,  597,  361,   76, 1593, 1551, 1010,  668,
        587,  899,   14,  905, 1459,  666, 1223,  861, 1147,   37,  161,
        233, 1333, 1616,  220,  811, 1145,  938,  921,  187, 1538, 1375,
       1572, 1449,  141,  376,  657, 1064, 1027,  511, 1343, 1787, 1260,
       1224,  688,  414,  665, 1666,  299, 1063,  828,  984, 1643,  842,
       1555, 1154, 1409,   39,  378, 1629, 1213, 1722,  530,  991,  452,
        799, 1433,  542, 1467]))

=:digits; X,y=digits.data, digits.target; X_train, X_test, y_train, y_test = scv.train_test_split(X,y,test_size=180,random_state=0); cv=scv.ShuffleSplit(X.shape[0],n_iter=3,test_size=0.1,random_state=0); train,test=list(cv)[0]; y_test == y[test]
===============================
[ True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True
  True  True  True  True  True  True  True  True  True  True  True  True]

time:22:05
tα:=3
tα.pvl
↑PythonSfOpen にファイル変数を設けるべき？

The hyperplanes in the higher-dimensional space are defined as the set of points whose dot product with a vector in that space is constant. 

Σi αi K(xi,x) == constant

N=50; seed(0); lsData=[(x,1) for x in (randn(N,2)-[2,0])] + [(x,1) for x in (randn(N,2)+[3,0])]; plotPt([x for x,y in lsData])
N=10; seed(0); lsData=[(x,1) for x in (randn(N,2)-[2,0])] + [(x,1) for x in (randn(N,2)+[3,0])]; plotPt([x for x,y in lsData])

# w,b=~[1,0], 0;
N=10; w,b=~[1,0], 0; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[3,0])]; min([y (w x + b) for x,y in lsData if y== 1])
===============================
0.447010184166

N=10; w,b=~[1,0], 0; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[3,0])]; min([y (w x + b) for x,y in lsData if y==-1])
===============================
0.13244200985

# w,b=~[1,0], 1;
N=10; w,b=~[1,0], 1; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[3,0])]; min([y (w x + b) for x,y in lsData if y== 1])
===============================
1.44701018417

N=10; w,b=~[1,0], 1; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[3,0])]; min([y (w x + b) for x,y in lsData if y==-1])
===============================
-0.86755799015

N=10; w,b=~[1,0], 1; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[3,0])];    ([y (w x + b) for x,y in lsData if y==-1])
===============================
[-0.76405234596766403, 0.021262015894260688, -0.86755799014996748, 0.049911582474410743, 1.1032188517935579, 0.85595642883912193, 0.23896227485300647, 0.55613676725457428, -0.49407907315760613, 0.68693229834909864]


N=10; w,b=~[1,0], 1; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[3,0])]; f,g =λ b:min([y (w x + b) for x,y in lsData if y==1]),λ b:min([y (w x + b) for x,y in lsData if y==-1]); plotGr(f,-3,3); plotGr(g, -3,3, color=red)

N=10; w,b=~[1,0], 1; seed(0); lsData=[(x,-1) for x in (randn(N,2)-[2,0])] + [ (x,1) for x in (randn(N,2)+[5,0])]; f,g =λ b:min([y (w x + b) for x,y in lsData if y==1]),λ b:min([y (w x + b) for x,y in lsData if y==-1]); plotGr(f,-3,3); plotGr(g, -3,3, color=red)

time:11:21
g= w x + b のてき x0 から g(x)=0 を満たす直線までの距離は g(x0)/norm(W)
直線 点 距離
d=(a x0 + b y0 + c)/(a^2+b^2)^0.5
↑(a/norm(w), b/norm(w)) は norm 1 の方向ベクトル
   c/norm(w) は norm 1 に対するオフセット

w x - b == 1 の意味

time:12:45
w x - b >= 1 を作ってみよ for randn(N,2)
time:23:20
w,b,N=~[0.9,0], 0.5, 900; seed(0); ar=randn(N,2); plotPt([x for x in ar if (w x + b) > 1]); plotPt([x for x in ar if (w x + b) <-1], color=red)
_tmC.py
-----------------------------------
* date:2014/06/06 (金) time:04:51
-----------------------------------
w,b,N=~[0.3,0],-0.5, 900; seed(0); ar=randn(N,2); plotPt([x for x in ar if (w x + b) > 1]); plotPt([x for x in ar if (w x + b) <-1], color=red)
Traceback (most recent call last):
  File "C:\Python27\lib\runpy.py", line 162, in _run_module_as_main
    "__main__", fname, loader, pkg_name)
  File "C:\Python27\lib\runpy.py", line 72, in _run_code
    exec code in run_globals
  File "D:\my\vc7\mtCmBkup\commercial\sfPP.py", line 30, in <module>
    pysf.sfPPrcssr.start()
  File "pysf\sfPPrcssr.py", line 3033, in start
    __execLine( lstLineAt[0].strip() )
  File "pysf\sfPPrcssr.py", line 2476, in __execLine
    exec(strAt, globals())
  File "<string>", line 11, in <module>
  File "pysf\vsGraph.py", line 1797, in plotPt
    assert hasattr(sqAg[0], '__len__'),(
IndexError: list index out of range
↑ 下と同じエラー
plotPt([])

info(rand)
# 10 は list index out of range, 20 はだめ
w,b,N=~[0.3,0],-0.5, 900; seed(0); ar=10 (rand(N,2)-0.5); plotPt([x for x in ar if (w x + b) > 1]); plotPt([x for x in ar if (w x + b) <-1], color=red)

w,b,N=~[0.3,0],-0.5, 900; seed(0); ar=10 (rand(N,2)-0.5);        [x for x in ar if (w x + b) > 1]
===============================
[]
w,b,N=~[0.3,0],-0.5, 900; seed(0);       x=~[5.1,0];                               (w x + b) > 1
===============================
True

w,b,N=~[0.3,0],-0.5, 900; seed(0);       x=~[5.1,0];                               (w x + b)
bd!
:===============================
1.03
w,b,N=~[0.3,0],-0.5, 900; seed(0);       x=~[5.1,0];                               x , (1-b)/norm(w)
===============================
(ClTensor([ 5.1,  0. ])
, 5.0)

w,b,N=~[0.3,0],-0.5,5900; seed(0); ar=12 (rand(N,2)-0.5); plotPt([x for x in ar if (w x + b) > 1]); plotPt([x for x in ar if (w x + b) <-1], color=red)
w,b,N=~[0.3,0],-0.5, 900; seed(0); ar=15 (rand(N,2)-0.5); plotPt([x for x in ar if (w x + b) > 1]); plotPt([x for x in ar if (w x + b) <-1], color=red)
w,b,N=~[0.3,0],-0.5, 900; seed(0); ar=20 (rand(N,2)-0.5); plotPt([x for x in ar if (w x + b) > 1]); plotPt([x for x in ar if (w x + b) <-1], color=red)

-----------------------------------
* date:2014/06/09 (月) time:04:30
-----------------------------------
w,b,N=~[0.3,0],-0.5,  20; seed(0); ar=20 (rand(N,2)-0.5); lsDt:=[x for x in ar if (w x + b) > 1]+[x for x in ar if (w x + b) <-1]
===============================
[ClTensor([ 9.27325521, -2.33116962])
, ClTensor([ 5.83450076,  0.5778984 ])
, ClTensor([ 5.56313502,  7.40024296])
, ClTensor([ 9.57236684,  5.98317128])
, ClTensor([ 8.87496157,  3.63640598])
, ClTensor([-8.57927884, -8.25741401])
, ClTensor([-9.59563205,  6.65239691])
, ClTensor([-7.63451148,  2.79842043])
, ClTensor([-7.13293425,  8.89337834])
, ClTensor([-4.70888776,  5.48467379])
, ClTensor([-9.62420399,  2.35270994])
]

=:lsDt; len(lsDt)
===============================
11
=:lsDt; plotPt(lsDt)

, ClTensor([ 5.83450076,  0.5778984 ]):index 1
, ClTensor([-4.70888776,  5.48467379]):index 9
=:lsDt; lsDt[9]
===============================
[-4.70888776  5.48467379]
---- ClTensor ----
`σx
`σy/`i
    θ=pi/2; pp(~[ [cos(θ),sin(θ)],[-sin(θ),cos(θ)]])
    [[  0, 1]
    ,[ -1, 0]]
    -------- pp --
    ===============================
    None
    θ=pi/2; (~[ [cos(θ),sin(θ)],[-sin(θ),cos(θ)]]) [1,1]
    ===============================
    [ 1. -1.]
    ---- ClTensor ----

θ=-pi/2; (~[ [cos(θ),sin(θ)],[-sin(θ),cos(θ)]]) [1,1]
===============================
[-1.  1.]
---- ClTensor ----
θ=-pi/2; pp(~[ [cos(θ),sin(θ)],[-sin(θ),cos(θ)]])
[[ 0, -1]
,[ 1,  0]]
-------- pp --
===============================
None
pp(-`i `σy)
[[ 0, -1]
,[ 1,  0]]
-------- pp --
===============================
None
pp(1/`i `σy)
pp(`σy/`i)
[[ 0, -1]
,[ 1,  0]]
-------- pp --
===============================
None
pp((`σy/`i).real)
[[ 0, -1]
,[ 1,  0]]
-------- pp --
===============================
None

θ=pi/2; =:lsDt; wn=[cos(θ),sin(θ)]; wn lsDt[1]
===============================
0.577898395058

θ=pi/2; =:lsDt; wn=[cos(θ),sin(θ)]; wn lsDt[1]
θ=pi/3; =:lsDt; wn=[cos(θ),sin(θ)]; wn lsDt[1] + bn == 0
θ=pi/3; =:lsDt; wn=[cos(θ),sin(θ)]; bn= -wn lsDt[1]; f=λ x:wn[0]/wn[1]+bn/wn[1]; plotGr(f, -5,5)
bad operand type for unary -: 'list'
_tmC.py
θ=pi/3; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; f=λ x:wn[0]/wn[1] x+bn/wn[1]; plotGr(f, -7,7)
↑ x=5.8 で正でなければならない
θ=pi/3; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; f=λ x:wn[0]/wn[1] x+bn/wn[1]; f(lsDt[1][0])
===============================
-0.577898395058
↑ 符号が反対
θ=pi/3; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; f(lsDt[1][0])
===============================
0.577898395058
θ=pi/3; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; plotGr(f, -7,7)
↑ wn に直交して lsDt[1] を通る直線の意味
直線 距離 公式

点Ａ（ ｘ0 ， ｙ0 ） から、直線 Ｌ ：　ａｘ＋ｂｙ＋ｃ＝０　に下ろした垂線の長さ ｄ は、
(a x0 + b y0 + c)/sqrt(a^2+b^2)
　　　　　　　　　　　　
# lsDt[1] を通る直線の 原点(0,0) からの距離
θ=pi/3; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; bn/normSq(wn)
===============================
-3.41772507175
θ=pi/3; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; bn
===============================
-3.41772507175

=:lsDt; f=λ θ:(Bf(wn=~[cos(θ),sin(θ)]), absF(Bf.wn lsDt[1]) + absF(Bf.wn lsDt[9]))[-1]; plotGr(f,0,2pi)

sy(); info(so.brent)

sy(); =:lsDt; f=λ θ:(Bf(wn=~[cos(θ),sin(θ)]),-absF(Bf.wn lsDt[1]) - absF(Bf.wn lsDt[9]))[-1]; so.brent(f)
===============================
-0.435577352008

=:lsDt; f=λ θ:(Bf(wn=~[cos(θ),sin(θ)]), absF(Bf.wn lsDt[1]) + absF(Bf.wn lsDt[9]))[-1]; plotGr(f,-3,3)

-0.435577352008+pi
===============================
2.70601530158

sy(); =:lsDt; f=λ θ:(Bf(wn=~[cos(θ),sin(θ)]),-absF(Bf.wn lsDt[1]) - absF(Bf.wn lsDt[9]))[-1]; f(2.70601530158)
===============================
-11.6292513189

θ=11.6292513189; =:lsDt; wn=~[cos(θ),sin(θ)]; bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; plotGr(f, -7,7)
↑ lsDt[1] と lsDt[9] を通る平行な直線で最大距離となる直線
    ↑ 分離しない

sklearn cheat sheet
D:\my\sf\blog\linearPrediction\SVM\cheat_sheet.png

cd D:\my\sf\blog\linearPrediction\SVM
    誤り
    =:lsDt; import sklearn.svm as md; clf=md.LinearSVC(); clf.fit([(lsData[k], 1 if k<5 else -1) for k in len(lsDt)], iris.target); clf.coef_
    'int' object is not iterable
    _tmC.py

    =:lsDt; import sklearn.svm as md; clf=md.LinearSVC(); clf.fit(lsDt,[1 if k<5 else -1 for k in len(lsDt)]); clf.coef_
    =:lsDt; import sklearn.svm as md; clf=md.LinearSVC(); clf.fit(lsDt,~[1 if k<5 else -1 for k in len(lsDt)]); clf.coef_
    'int' object is not iterable
    _tmC.py
    Traceback (most recent call last):
      File "D:\my\sf\blog\linearPrediction\SVM\_tmC.py", line 11, in <module>
        clf.fit(lsDt,krry__(*[1 if k<5 else -1 for k in len(lsDt)]))
    TypeError: 'int' object is not iterable

    =:iris; import sklearn.svm as md; clf=md.LinearSVC(); clf.fit(iris.data, iris.target); clf.coef_
    ===============================
    [[ 0.18423971  0.45122859 -0.80794391 -0.45071181]
     [ 0.05166345 -0.89321526  0.40582911 -0.93951946]
     [-0.85065685 -0.98667517  1.38099162  1.86541762]]

    =:iris; import sklearn.svm as md; clf=md.LinearSVC();        (iris.data, iris.target)

=:lsDt; lsDt
===============================
[ClTensor([ 9.27325521, -2.33116962])
, ClTensor([ 5.83450076,  0.5778984 ])
, ClTensor([ 5.56313502,  7.40024296])
, ClTensor([ 9.57236684,  5.98317128])
, ClTensor([ 8.87496157,  3.63640598])
, ClTensor([-8.57927884, -8.25741401])
, ClTensor([-9.59563205,  6.65239691])
, ClTensor([-7.63451148,  2.79842043])
, ClTensor([-7.13293425,  8.89337834])
, ClTensor([-4.70888776,  5.48467379])
, ClTensor([-9.62420399,  2.35270994])
]
=:iris; import sklearn.svm as md; clf=md.LinearSVC(); clf.fit(~[iris.data], list(iris.target)); clf.coef_

=:lsDt; import sklearn.svm as md; clf=md.LinearSVC(); clf.fit(lsDt,~[1 if k<5 else -1 for k,_ in enumerate(lsDt)]); clf.coef_
===============================
[[ 0.19496393 -0.0114674 ]]
=:iris; clf=svm.LinearSVC(); clf.fit(~[iris.data], list(iris.target)); clf.coef_
===============================
[[ 0.18423874  0.45123008 -0.80794075 -0.45071545]
 [ 0.04492856 -0.87704272  0.41003205 -0.94855902]
 [-0.85068471 -0.98665302  1.38101499  1.86541215]]

=:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; plotGr(f, -7,7)
=:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; plotGr(f, -100,100)

=:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; f(lsDt[1][0])
===============================
0.577898395058
    誤り
    =:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; f(lsDt[1][0]), f(lsDt[9][0])
    ===============================
    (0.57789839505808516, -178.67637557205268)

=:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[9]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; f(lsDt[9][0])
===============================
5.48467378868
↑ svm.LinearSVC(..) は lsDt[2] について >1 が成り立たない
=:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; f(lsDt[2][0])
===============================
-4.0357482603
=:lsDt; wn=normalize([ 0.19496393,-0.0114674 ]); bn= -wn lsDt[1]; f=λ x:-wn[0]/wn[1] x-bn/wn[1]; f(lsDt[3][0])
===============================
64.1275313856

time:08:19
    ↑ predict は上手く働く
=:lsDt; clf=svm.LinearSVC(); clf.fit(lsDt,~[1 if k<5 else -1 for k,_ in enumerate(lsDt)]); clf.predict(lsDt)
===============================
[ 1.  1.  1.  1.  1. -1. -1. -1. -1. -1. -1.]

=:lsDt; clf=svm.LinearSVC(); clf.fit(lsDt,~[1 if k<5 else -1 for k,_ in enumerate(lsDt)]); dir(clf)
===============================
['C', '__abstractmethods__', '__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_abc_cache', '_abc_negative_cache', '_abc_negative_cache_version', '_abc_registry', '_enc', '_get_bias', '_get_param_names', '_get_solver_type', '_predict_proba_lr', '_solver_type_dict', 'class_weight', 'class_weight_', 'classes_', 'coef_', 'decision_function', 'densify', 'dual', 'fit', 'fit_intercept', 'fit_transform', 'get_params', 'intercept_', 'intercept_scaling', 'label_', 'loss', 'multi_class', 'penalty', 'predict', 'random_state', 'raw_coef_', 'score', 'set_params', 'sparsify', 'tol', 'transform', 'verbose']
=:lsDt; clf=svm.LinearSVC(); clf.fit(lsDt,~[1 if k<5 else -1 for k,_ in enumerate(lsDt)]); `print(clf.__dict__)
{'C': 1.0,
 '_enc': LabelEncoder(),
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'coef_': array([[ 0.19496368, -0.01146762]]),
 'dual': True,
 'fit_intercept': True,
 'intercept_': array([-0.00751638]),
 'intercept_scaling': 1,
 'loss': 'l2',
 'multi_class': 'ovr',
 'penalty': 'l2',
 'random_state': None,
 'raw_coef_': array([[ 0.19496368, -0.01146762, -0.00751638]]),
 'tol': 0.0001,
 'verbose': 0}
===============================
None
-0.00751638 が たぶん b
↑ たぶん、二つの分離線の真ん中の線
    ↑ intercept_ でもある

=:lsDt; w=~[0.19496368, -0.01146762]; b=-0.00751638; f=λ x:-w[0]/w[1] x-b/w[1]; plotGr(f,-5,5)

`σx.astype(np.int)
===============================
[[0 1]
 [1 0]]
---- ClTensor ----

`σx.astype(int)
===============================
[[0 1]
 [1 0]]
---- ClTensor ----
dir(`σx)
===============================
['T', '__abs__', '__add__', '__and__', '__array__', '__array_finalize__', '__array_interface__', '__array_prepare__', '__array_priority__', '__array_struct__', '__array_wrap__', '__class__', '__contains__', '__copy__', '__deepcopy__', '__delattr__', '__delitem__', '__delslice__', '__dict__', '__div__', '__divmod__', '__doc__', '__eq__', '__float__', '__floordiv__', '__format__', '__ge__', '__getattr__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__hex__', '__iadd__', '__iand__', '__idiv__', '__ifloordiv__', '__ilshift__', '__imod__', '__imul__', '__index__', '__init__', '__int__', '__invert__', '__ior__', '__ipow__', '__irshift__', '__isub__', '__iter__', '__itruediv__', '__ixor__', '__le__', '__len__', '__long__', '__lshift__', '__lt__', '__mod__', '__module__', '__mul__', '__ne__', '__neg__', '__new__', '__nonzero__', '__oct__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdiv__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__setitem__', '__setslice__', '__setstate__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__xor__', 'all', 'any', 'argmax', 'argmin', 'argsort', 'astype', 'base', 'byteswap', 'choose', 'clip', 'cntrct', 'compress', 'conj', 'conjugate', 'copy', 'ctypes', 'cumprod', 'cumsum', 'data', 'diagonal', 'dot', 'dtype', 'dump', 'dumps', 'fill', 'flags', 'flat', 'flatten', 'getfield', 'imag', 'inv', 'item', 'itemset', 'itemsize', 'max', 'mean', 'min', 'nbytes', 'ndim', 'newbyteorder', 'nonzero', 'prod', 'ptp', 'put', 'ravel', 'real', 'repeat', 'reshape', 'resize', 'round', 'searchsorted', 'setasflat', 'setfield', 'setflags', 'shape', 'size', 'sort', 'squeeze', 'std', 'strides', 'sum', 'swapaxes', 'take', 'tofile', 'tolist', 'tostring', 'trace', 'transpose', 'tuple_', 'var', 'view']

-----------------------------------
*date:2014/06/10 (火) time:05:50
-----------------------------------
dt,trgt=[(1,0), (-1,0),(-1,1)], (1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.coef_
===============================
[[ 0.82104136 -0.10527819]]
    invalid syntax (<string>, line 8)
    _tmC.py
dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.coef_
===============================
[[  8.88892165e-01   7.30820695e-06]]

    dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); dir(clf)
    ===============================
    ['C', '__abstractmethods__', '__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_abc_cache', '_abc_negative_cache', '_abc_negative_cache_version', '_abc_registry', '_enc', '_get_bias', '_get_param_names', '_get_solver_type', '_predict_proba_lr', '_solver_type_dict', 'class_weight', 'class_weight_', 'classes_', 'coef_', 'decision_function', 'densify', 'dual', 'fit', 'fit_intercept', 'fit_transform', 'get_params', 'intercept_', 'intercept_scaling', 'label_', 'loss', 'multi_class', 'penalty', 'predict', 'random_state', 'raw_coef_', 'score', 'set_params', 'sparsify', 'tol', 'transform', 'verbose']
dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); `print(clf.__dict__)
{'C': 1.0,
 '_enc': LabelEncoder(),
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'coef_': array([[  8.88889417e-01,  -1.49880024e-06]]),
 'dual': True,
 'fit_intercept': True,
 'intercept_': array([ -1.37480958e-05]),
 'intercept_scaling': 1,
 'loss': 'l2',
 'multi_class': 'ovr',
 'penalty': 'l2',
 'random_state': None,
 'raw_coef_': array([[  8.88889417e-01,  -1.49880024e-06,  -1.37480958e-05]]),
 'tol': 0.0001,
 'verbose': 0}
===============================
None

dt,trgt=[(1,0), (-1,0),(-1,1)], (1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
===============================
[[ 0.82104136 -0.10527819 -0.02104136]]
===============================
[[ 0.82105921 -0.10525439 -0.02105921]]

seed(0); dt,trgt=[(1,0), (-1,0),(-1,1)], (1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
===============================
[[ 0.82105564 -0.10527819 -0.02105564]]
===============================
[[ 0.82105564 -0.10527819 -0.02105564]]
↑ .fit(..) method は np.random を利用している

[[(1,0),(1,1)]]*3
===============================
[[(1, 0), (1, 1)], [(1, 0), (1, 1)], [(1, 0), (1, 1)]]
[(1,0),(1,1)]*3
===============================
[(1, 0), (1, 1), (1, 0), (1, 1), (1, 0), (1, 1)]
[1,0]*9
===============================
[1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0]
dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
===============================
[[  8.88877695e-01  -5.16710710e-06  -2.58178446e-06]]
===============================
[[  8.88888676e-01   3.13599310e-06   1.69033305e-05]]

dt,trgt=[(1,0),(1,1)]*9+[(-1,0),(-1,1)]*9, [(1,1)]*9+[(-1,-1)]*9; clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
bad input shape (18, 2)
dt,trgt=[(1,0),(1,1)]*9+[(-1,0),(-1,1)]*9, [(1,1)]*9+[(-1,-1)]*9; dt, trgt
dt,trgt=[(1,0),(1,1)]*9+[(-1,0),(-1,1)]*9, [1,1]*9+[-1,-1]*9; clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
===============================
[[  9.86298203e-01  -1.18345941e-05  -4.60032888e-06]]

dt,trgt=[(1,0)      ]*9+[(-1,0),(-1,1)]*9, [1  ]*9+[-1,-1]*9; clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
===============================
[[  9.73632355e-01  -2.43543012e-02  -6.53283729e-04]]

dt,trgt=[(3,0)      ]*9+[(-1,0),(-1,1)]*9, [1  ]*9+[-1,-1]*9; clf=svm.LinearSVC(); clf.fit(dt, trgt); clf.raw_coef_
===============================
[[ 0.49357316 -0.02420283 -0.48087984]]

    dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.SVC(); clf.fit(dt, trgt); `print(clf.__dict__)
        {'C': 1.0,
         '_gamma': 0.5,
         '_impl': 'c_svc',
         '_intercept_': array([ 0.]),
         '_label': array([0, 1]),
         '_sparse': False,
         'cache_size': 200,
         'class_weight': None,
         'class_weight_': array([ 1.,  1.]),
         'classes_': array([-1,  1]),
         'coef0': 0.0,
         'degree': 3,
         'dual_coef_': array([[ 0.71946406,  0.72057953, -0.71946406, -0.72057953]]),
         'epsilon': 0.0,
         'fit_status_': 0,
         'gamma': 0.0,
         'intercept_': array([-0.]),
         'kernel': 'rbf',
         'max_iter': -1,
         'n_support_': array([2, 2]),
         'nu': 0.0,
         'probA_': array([], dtype=float64),
         'probB_': array([], dtype=float64),
         'probability': False,
         'random_state': None,
         'shape_fit_': (4, 2),
         'shrinking': True,
         'support_': array([2, 3, 0, 1]),
         'support_vectors_': array([[-1.,  0.],
               [-1.,  1.],
               [ 1.,  0.],
               [ 1.,  1.]]),
         'tol': 0.001,
         'verbose': False}
        ===============================
        None

dt,trgt=[(1,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2); clf.fit(dt, trgt); `print(clf.__dict__)
    {'C': 1.0,
     '_gamma': 0.5,
     '_impl': 'c_svc',
     '_intercept_': array([ 0.30544481]),
     '_label': array([0, 1]),
     '_sparse': False,
     'cache_size': 200,
     'class_weight': None,
     'class_weight_': array([ 1.,  1.]),
     'classes_': array([-1,  1]),
     'coef0': 0.0,
     'degree': 2,
     'dual_coef_': array([[ 0.56766764,  0.43233236, -1.        ]]),
     'epsilon': 0.0,
     'fit_status_': 0,
     'gamma': 0.0,
     'intercept_': array([-0.30544481]),
     'kernel': 'rbf',
     'max_iter': -1,
     'n_support_': array([2, 1]),
     'nu': 0.0,
     'probA_': array([], dtype=float64),
     'probB_': array([], dtype=float64),
     'probability': False,
     'random_state': None,
     'shape_fit_': (3, 2),
     'shrinking': True,
     'support_': array([1, 2, 0]),
     'support_vectors_': array([[-1.,  0.],
           [-1.,  1.],
           [ 1.,  0.]]),
     'tol': 0.001,
     'verbose': False}
    ===============================
    None
dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.SVC(degree=2); clf.fit(dt, trgt); `print(clf.__dict__)
    {'C': 1.0,
     '_gamma': 0.5,
     '_impl': 'c_svc',
     '_intercept_': array([ 0.]),
     '_label': array([0, 1]),
     '_sparse': False,
     'cache_size': 200,
     'class_weight': None,
     'class_weight_': array([ 1.,  1.]),
     'classes_': array([-1,  1]),
     'coef0': 0.0,
     'degree': 2,
     'dual_coef_': array([[ 0.71946406,  0.72057953, -0.71946406, -0.72057953]]),
     'epsilon': 0.0,
     'fit_status_': 0,
     'gamma': 0.0,
     'intercept_': array([-0.]),
     'kernel': 'rbf',
     'max_iter': -1,
     'n_support_': array([2, 2]),
     'nu': 0.0,
     'probA_': array([], dtype=float64),
     'probB_': array([], dtype=float64),
     'probability': False,
     'random_state': None,
     'shape_fit_': (4, 2),
     'shrinking': True,
     'support_': array([2, 3, 0, 1]),
     'support_vectors_': array([[-1.,  0.],
           [-1.,  1.],
           [ 1.,  0.],
           [ 1.,  1.]]),
     'tol': 0.001,
     'verbose': False}
    ===============================
    None

dt,trgt=[(2,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.33326667]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 2,
 'dual_coef_': array([[  2.22000000e-01,   2.00000000e-04,  -2.22200000e-01]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.33326667]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([2, 1]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (3, 2),
 'shrinking': True,
 'support_': array([1, 2, 0]),
 'support_vectors_': array([[-1.,  0.],
       [-1.,  1.],
       [ 2.,  0.]]),
 'tol': 0.001,
 'verbose': False}
===============================
None
dt,trgt=[(1,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 2,
 'dual_coef_': array([[ 0.5, -0.5]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([1, 1]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (3, 2),
 'shrinking': True,
 'support_': array([1, 0]),
 'support_vectors_': array([[-1.,  0.],
       [ 1.,  0.]]),
 'tol': 0.001,
 'verbose': False}
===============================
None
dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 2,
 'dual_coef_': array([[ 0.5, -0.5]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([1, 1]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (4, 2),
 'shrinking': True,
 'support_': array([3, 1]),
 'support_vectors_': array([[-1.,  1.],
       [ 1.,  1.]]),
 'tol': 0.001,
 'verbose': False}
===============================
None

time:07:52
# SVC を三次元で動かしてみる
↑ α らしい parameter があるはず
dt,trgt=[(1,0,0),(1,1,0), (-1,0,0),(-1,1,0)], (1,1, -1,-1); clf=svm.SVC(degree=3,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 3,
 'dual_coef_': array([[ 0.5, -0.5]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([1, 1]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (4, 3),
 'shrinking': True,
 'support_': array([3, 1]),
 'support_vectors_': array([[-1.,  1.,  0.],
       [ 1.,  1.,  0.]]),
 'tol': 0.001,
 'verbose': False}
===============================
None

# SVC.support_vectors が w と b を表していそうだ。上で、三次元に変化したのはここだけだから
dt,trgt=[(1,0),(1,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)

dt,trgt=[(3,0),(3,1), (-1,0),(-1,1)], (1,1, -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.  1.]
 [ 3.  1.]]
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.  0.]
 [-1.  1.]
 [ 3.  0.]]
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_, clf.dual_coef_
===============================
(array([[-1.,  0.],
       [-1.,  1.],
       [ 3.,  0.]]),
 array([[ 0.12456747,  0.00040708, -0.12497456]]))

# svm.SVC(..) による support vector の計算
↑ マイナス符号と /2 が気に入らん
    ↑b:bias は？
        ↑ _intercept_ に間違いない
            
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2
===============================
[  2.49949115e-01  -2.03541624e-04]
    dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0];  α[0] x[0]+α[1] x[1]+α[2] x[2]
    ===============================
    [ -4.99898229e-01   4.07083249e-04]
    dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0];  np.dot(α,x)
    ===============================
    [ -4.99898229e-01   4.07083249e-04]

    dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -α[0] x[0]-α[1] x[1]+α[2] x[2]
    ===============================
    [-0.24994911 -0.00040708]
    dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -α[0]     -α[1]     +α[2]     
    ===============================
    -0.249949114594
↑ 0 にならん
    ↑ α に符号まで入れている
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0];  α
===============================
[ 0.12456747  0.00040708 -0.12497456]

    下は誤り
    dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_,α=clf.dual_coef_[0]; -α[0] x[0]-α[1] x[1]+α[2] x[2]
    too many values to unpack

    operands could not be broadcast together with shapes (3) (2) 
    _tmC.py
    dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x,α=clf.support_vectors_, clf.dual_coef_;  α[0],x[0]
    ===============================
    (array([ 0.12456747,  0.00040708, -0.12497456]), array([-1.,  0.]))
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); plotPt(dt)
↑ support_vectors_ が 3x2行列になる意味が理解できん
    ↑ support_vectors を定める点の位置ベクトル

## svm.SVC(..) の bias 属性を探す
↑ _intercept_ に間違いない
dt,trgt=~[(3.1,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)
↑ ClTensor 引数にしてみても、内部では np.array(..) に変えられている

dt,trgt=[(3.1,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); `print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.51204545]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 2,
 'dual_coef_': array([[ 0.11860171,  0.00035403, -0.11895574]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.51204545]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([2, 1]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (3, 2),
 'shrinking': True,
 'support_': array([1, 2, 0]),
 'support_vectors_': array([[-1. ,  0. ],
       [-1. ,  1. ],
       [ 3.1,  0. ]]),
 'tol': 0.001,
 'verbose': False}
===============================
None

## 三次元にしてみる
↑ svm.SVC(..).support_vectors_ は support points を返している
dt,trgt=[(3,0,0),(3,0.9,0),(3,2,0), (-1,0,0),(-1,1,0)], (1,1,1, -1,-1); clf=svm.SVC(degree=3,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.   0.   0. ]
 [-1.   1.   0. ]
 [ 3.   0.9  0. ]]

dt,trgt=[(3,0,0),(3,1,0),(3,2,0), (-1,0,0),(-1,1,0)], (1,1,1, -1,-1); clf=svm.SVC(degree=3,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.  1.  0.]
 [ 3.  1.  0.]]
dt,trgt=[(3,0,0),(3,1,0), (-1,0,0),(-1,1,0)], (1,1, -1,-1); clf=svm.SVC(degree=3,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.  1.  0.]
 [ 3.  1.  0.]]

dt,trgt=[(3,0,0),       (-1,0,0),(-1,1,0)], (1,   -1,-1); clf=svm.SVC(degree=3,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.  0.  0.]
 [-1.  1.  0.]
 [ 3.  0.  0.]]
dt,trgt=[(3,0,0),       (-1,0,0),(-1,1,0)], (1,   -1,-1); clf=svm.SVC(degree=3,kernel='linear'); clf.fit(dt, trgt); clf.dual_coef_
===============================
[[ 0.12456747  0.00040708 -0.12497456]]
↑α くさい

dt,trgt=[(3,0,0),       (-1,0,0),(-1,1,0)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[-1.  0.  0.]
 [-1.  1.  0.]
 [ 3.  0.  0.]]
dt,trgt=[(0,3,0),       (0,-1,0),(0,-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); clf.support_vectors_
===============================
[[ 0. -1.  0.]
 [ 0. -1.  1.]
 [ 0.  3.  0.]]
`
=:lsDt;  clf=svm.SVC(degree=2,kernel='linear'); clf.fit(lsDt, [1]*5+[-1]*6); clf.support_vectors_
===============================
[[-4.70888776  5.48467379]
 [ 5.83450076  0.5778984 ]
 [ 5.56313502  7.40024296]]
=:lsDt; plotPt(lsDt[:5]); plotPt(lsDt[5:],color=red)
invalid syntax (<string>, line 8)
_tmC.py

-----------------------------------
*date:2014/06/11 (水) time:07:53
-----------------------------------
arg min {1/2 ||w||^2 + C Σξ[i]} で C はユーザーが適当だと定める givine constant
    w,ξ,b
         ↑ b によって w,ξの値は変わる

cd D:\my\sf\blog\linearPrediction\SVM
=:lsDt; lsDt=lsDt
=:lsDt; plotPt(lsDt)
=:lsDt; lsDt
===============================
[ClTensor([ 9.27325521, -2.33116962])
, ClTensor([ 5.83450076,  0.5778984 ])
, ClTensor([ 5.56313502,  7.40024296])
, ClTensor([ 9.57236684,  5.98317128])
, ClTensor([ 8.87496157,  3.63640598])
, ClTensor([-8.57927884, -8.25741401])
, ClTensor([-9.59563205,  6.65239691])
, ClTensor([-7.63451148,  2.79842043])
, ClTensor([-7.13293425,  8.89337834])
, ClTensor([-4.70888776,  5.48467379])
, ClTensor([-9.62420399,  2.35270994])
]

time:11:05
w,b,N=~[0.3,0],-0.5,  20; seed(0); ar=20 (rand(N,2)-0.5); lsDt:=[x for x in ar if (w x + b) > 1]+[x for x in ar if (w x + b) <-1]

SVC(C=1.0, kernel='rbf', degree=3, gamma=0.0, coef0=0.0, shrinking=True,
     probability=False, tol=0.001, cache_size=200, class_weight=None,
     verbose=False, max_iter=-1, random_state=None)

dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(degree=2,kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2, clf._intercept_
dt,trgt=[(3,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(         kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2, clf._intercept_
===============================
(array([  2.49949115e-01,  -2.03541624e-04]), array([ 0.49983038]))
===============================
(array([  2.49949115e-01,  -2.03541624e-04]), array([ 0.49983038]))

dt,trgt=[(1,0),       (-1,0),(-1,1)], (1,   -1,-1); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2, clf._intercept_
===============================
(array([ 0.5, -0. ]), array([ 0.]))

dt,trgt=[(1,0),(-0.1,0), (-1,0),(-1,1),(0.1,0)], (1,1,  -1,-1,-1); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;α=clf.dual_coef_[0]; -np.dot(α,x)/2, clf._intercept_
===============================
(array([ 0.5, -0. ]), array([ 0.]))

# soft margin の有無による違いがあるか？
dt,trgt=[(1,0),          (-1,0),(-1,1)        ], (1,    -1,-1   ); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;`print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 3,
 'dual_coef_': array([[ 0.5, -0.5]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([1, 1]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (3, 2),
 'shrinking': True,
 'support_': array([1, 0]),
 'support_vectors_': array([[-1.,  0.],
       [ 1.,  0.]]),
 'tol': 0.001,
 'verbose': False}
===============================
None

dt,trgt=[(1,0),(-0.1,0), (-1,0),(-1,1),(0.1,0)], (1,1,  -1,-1,-1); clf=svm.SVC(kernel='linear'); clf.fit(dt, trgt); x=clf.support_vectors_;`print(clf.__dict__)
{'C': 1.0,
 '_gamma': 0.0,
 '_impl': 'c_svc',
 '_intercept_': array([ 0.]),
 '_label': array([0, 1]),
 '_sparse': False,
 'cache_size': 200,
 'class_weight': None,
 'class_weight_': array([ 1.,  1.]),
 'classes_': array([-1,  1]),
 'coef0': 0.0,
 'degree': 3,
 'dual_coef_': array([[ 0.6,  1. , -0.6, -1. ]]),
 'epsilon': 0.0,
 'fit_status_': 0,
 'gamma': 0.0,
 'intercept_': array([-0.]),
 'kernel': 'linear',
 'max_iter': -1,
 'n_support_': array([2, 2]),
 'nu': 0.0,
 'probA_': array([], dtype=float64),
 'probB_': array([], dtype=float64),
 'probability': False,
 'random_state': None,
 'shape_fit_': (5, 2),
 'shrinking': True,
 'support_': array([2, 4, 0, 1]),
 'support_vectors_': array([[-1. ,  0. ],
       [ 0.1,  0. ],
       [ 1. ,  0. ],
       [-0.1,  0. ]]),
 'tol': 0.001,
 'verbose': False}
===============================
None
↑ support_ 属性は support_vectors_ を指し示す整数インデックス
</pre>

<p><input type="button" value="hide the remaining part△" style="WIDTH:150px"
   onClick="document.getElementById('div_2').style.display='none';
            document.getElementById('div_1').style.display='block';
            document.location='#div_1'"></p>
</div>

