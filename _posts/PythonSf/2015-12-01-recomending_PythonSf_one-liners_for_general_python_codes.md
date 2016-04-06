---
layout: post
category : PythonSf
tagline: "one-liners"
tags : [PythonSf, one-liners]
title : 一般の PythonCode でも PythonSf one-liners を活用しましょう--jpn
---
{% include JB/setup %}

<!--
sub theme: construct your own global name space and utilize it.

カーソル下一行/ブロックの計算式・OS コマンド・任意プログラムのvim による実行
:PythonSf is a RITE tool too. rapid iterative test and evaluation tools

計算結果は 最下行のコマンド・ライン行に一時的に表示されると同時に yank レジスタに記録される。

<pre font-family="MS gothic" style="font-size:25px;">
┌→ 文字列作成 → computer 処理 → 処理結果の文字列 → 人間の判断  ─┐
│                                                                    │
└──────────────────────────────────┘
</pre>

emacs の eshell のようなソフトはありますが、shell:console であり、実行結果はコンソール画面の最下行に強制的に追加書き込みされてしまいます。

数式の入力は　エディタで行うのが一番合理的です。


小さいマクロですから、ご自分でも簡単に作れると思います。

できるだけ小さな code snipett で動くものを作ること。
↑それを元に少しだけ変形したコードを動かすこと

<pre class="box">
<code>
(1,2,3)==[1,2,3]
list((1,2,3))==[1,2,3]
{1,2,3}==[1,2,3]
frozenset([1,2,3])==[1,2,3]
frozenset([1,2,3])==(1,2,3)
a,b,c,d=1,1,2,1; a==b==d, a==b==c
a,b,c,d=1,1,2,1; a!=b!=d, a!=b!=c, a==b!=c!=d
id((1,2,3)) == id((1,2,3))
id('abc') == id('abc')
np.array([1,2,3])==[1,2,3]
np.array([1,2,3])==(1,2,3)
np.array([1,2,3])== np.array([1,2,3])
np.array([1,2,3]) != np.array([1,2,4])
np.array([1,2,3])== np.array([1,2,3]) != np.array([4,5,6])

a,b,c,d=1,1,2,1; a!=b!=d, a!=b!=c,
import dis; dis.dis(λ a,b,c,d:a==b!=c!=d)
import dis; dis.dis(λ a,b,c:a==b==c)
  9           0 LOAD_FAST                0 (a)
              3 LOAD_FAST                1 (b)
              6 DUP_TOP             
              7 ROT_THREE           
              8 COMPARE_OP               2 (==)
             11 JUMP_IF_FALSE_OR_POP    21
             14 LOAD_FAST                2 (c)
             17 COMPARE_OP               2 (==)
             20 RETURN_VALUE        
        >>   21 ROT_TWO             
             22 POP_TOP             
             23 RETURN_VALUE        
===============================
None

import dis; dis.dis(λ:id((1,2,3)) == id((1,2,3)))
import dis; dis.dis(λ:id('abc') == id('abc'))
f1,f2=λ:`print(id('abc')), λ:`print(id('abcd')); f1(),f2()
33147576
89796480
===============================
(None, None)
f1,f2=λ:`print(id('abc')), λ:`print(id('abcd')); f1();f2();
5163704
88125536
f1,f2=λ:`print(id('abc')), λ:`print(id('abc')); f1();f2(); (2,id('abc'))
</code>
</pre>

ブロック・コードでも確認できるが、縦に長くなって、比較の意味での可読性が悪くなる
//@@
print  """abc\nXYZ"""
//@@@
abc
XYZ

//@@
print r"""abc\nXYZ"""
//@@@
abc\nXYZ

# IPython など他のソフトとの併用も問題ありません。
学習コストは editor macro の組込み作業が一番大きい程度です。
暫くは one-liner python として使ってみてください。
ギリシャ文字を使うのは学習コストではないでしょう。

# html ファイル編集中に、html ファイル中の一行を、またはブロックを何時でも実行できる。

# one-liner を書いて ;j 操作をするだけで機能を確認できるのは便利です。
コードを少し書き足すごとに動作を確認できます。
↑ 誤りコード書き込んだタイミングでエラーを検出できる便利さはプログラマーならば了解してもらえるはずです。
    ↑書き込んだ 誤りコードは大部分の場合、エラー動作に気付くまで残ります。目視だけでバグを取り除けることは一パーセントもないでしょう。
    ↑この誤りは仕様に関連する根本的な誤りの可能性もあります。

http://qiita.com/advent-calendar/2015/vim-script
http://loboskobayashi.github.io/pythonsf/2015/12-10/executing_one_liner_expressions_or_any_block_codes/
vim ユーザーでしたら ctrl+W → gf, → gF 操作でファイルを開く便利な操作を多用していると思います。これを一般化して、カーソル下の文字列を特定の種類の文字列とみなして、その種類にあわせた動作をエディタにさせると多くのコンピュータ操作が楽になります。

拡張子 関連付け Linux;;アプリケーションとファイルの関連付けを設定するには
↑Linux でも関連付けできるとは知らんかった

Python プログラム開発でも one-liners で動作を確認します。
↑ debuger や ipython でのように import しなおす必要がありません。
    ↑source が変わっているのですから、Python が自動的にコンパイルしなおしてくれます。

# CUI 操作
## git 操作は vim 上で行い、その操作記録を残しています。

# 自信の無いコードの確認
import itertools as it; it
shape=(2,3); list(tn.it.product(range(shape[0]), range(shape[1])))
===============================
[(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2)]

# emacs screen などと異なり、一行ずつの実行にすぎませんから、エディタの最下行に勝手に移ることはしません。

# one-liner と名前空間の属人性
↑sed, perl 系統の黒魔術 one-liners は唾棄すべき代物だと主張します。

理系の人間にとって np:numpy module 名は global 変数であるべき。
np.angle(3+4j)
===============================
0.927295218002

np.info, np.source は Python user 全員にとって global 変数であるべき
↑dir, help 以上に使える


XyPic(r"小林\ar[r] & \phi")
LaTeX(r"$$\sum_{k=1}^{\infty} \frac{1}{k} = \infty$$")
↑;s で実行すると別プロセスで実行してくれるので、LaTeX 処理終了をまたなくても済む
↑実行結果はエディタに返されないけれど、
↑ファイルを介したコマンド・ラインでの定型処理一般に使えるテクニック

↓大部分の git 操作
コンピュータ操作自体の多くをエディタ内で、結果も含めて記録しながら行っていく

def ctx():
    import PyV8 as md
    ctx=md.JSContext()
    ctx.enter()
    sf.__getDctGlobals()['ctx']=ctx
    return 

import PyV8 as md; ctx=md.JSContext(); ctx.enter(); ctx.eval('var y=[1,2]'); ctx.locals['y']
===============================
1,2

ctx(); ctx.eval('var y=[1,2]'); ctx.locals['y']
===============================
1,2

# recursive loop に入り込んだとき、直ぐに検出できる可能性を大きく高められます。

# 動作確認 one-liner
↑ できているソフトの多くの箇所を通るコードにする。
↑ まだテストを通す段階ではない
ksv(); ar=ksv.AlndEclsAr(~[10mm*"ABCxyz", 10mm*'→', 10mm*'abc', object]); ar.tostring()

## one-liner の実行が printf デバッグの代わり
↑ 自分の作っているクラス・データメンバーは分かっており、それが予定どおりの値か否かの確認が大部分
ksv(); a=(10mm*"ABCxyz"); a.E, a.m_arInsert, a.m_arSize
===============================
(array([ 36.,  54.]), None, array([  36.,  108.]))
↑ m_arSize の x,y が逆転している

## デバッガを立ち上げることが殆ど無しで済ませられる。

## 業務ノートがデバッグ作業記録にもなる。
↑ 業務ノート の one-liner と その動作結果の間に、考察メモ・コード修正記録をメモしていく形になる
↑ 殆ど vim editor の上だけでの作業を続けることになる。
    ↑ web での検索がなければマウスをまず使わない。

ksv(); ar=ksv.AlndEclsAr(~[10mm*"ABCxyz", 10mm*u'→', 10mm*'abc', object]); ar.tostring()
u'→'
===============================
Traceback (most recent call last):
  File "D:\my\vc7\mtCmBkup\commercial\pysf\sfPPrcssr.py", line 2474, in __execLine
    exec(strAt, globals())
  File "<string>", line 12, in <module>
UnicodeEncodeError: 'ascii' codec can't encode character u'\u2192' in position 132: ordinal not in range(128)

'ascii' codec can't encode character u'\u2192' in position 132: ordinal not in range(128)

//@@
\documentclass{jarticle}
\begin{document}

$$\sum_{k=1}^{\infty} \frac{1}{k} = \infty$$

\end{document}
//@@@
//copy __tmp temp.tex /y
//platex temp.tex


//@@
\documentclass{jarticle}
\usepackage[all]{xy}
\begin{document}
\[\xymatrix{
& A & \\
& M \ar[u]_h & \\
M_i \ar[rr]_{\varphi_{ji}} \ar[ruu]^{f_i} \ar[ru]_{\varphi} & & M_j \ar[luu]_{f_i} \ar[lu]^{\varphi_j} \\
}\]
\end{document}
//@@@
//copy __tmp temp.tex /y
//platex temp.tex

XyPic(r"\\(\left(\begin{array}{cc} 1&0\\ \\ 0&1\\ \end{array}\right)\\)")

https://www.youtube.com/watch?v=rdo-46WafyQ
-->

## 始めに

PythonSf と名づけたエディタに組み込んで使う計算ソフトを開発しています。vim,emacs に pysf.vim, pysf.el といった小さな script を組み込んで使います。その PythonSf では one-liners での計算を多用します。通常の使用では 99% 以上が one-liners になると思います。を組み込んで計算させます。
[こんな具合に](https://www.youtube.com/watch?v=rdo-46WafyQ) カーソル下の計算式文字列を実行させます。

下の URL より PythonSf utf-8 版と shift-jis 版を入手できます。

1. [cps932版 PythonSf;;https://github.com/lobosKobayashi/PythonSfCp932](https://github.com/lobosKobayashi/PythonSfCp932)
1. [utf-8版 PythonSf;;https://github.com/lobosKobayashi/PythonSfUtf8](https://github.com/lobosKobayashi/PythonSfUtf8)

Windows 限定ですが、[ここから](https://drive.google.com/drive/folders/0BwB_czqjpb6XaXF6TEluWWg1Ulk) vim と Python も含んだポータブル版 zip ファイルを入手することもできます。bigCpPysf32_version97B.zip が cp932版であり、 bigUtPysf32_version97B.zip が utf-8 版です。Windows ではギリシャ漢字文字を含んだファイル変数を使うために cp932版を使うことを進めます。

この PythonSf one-liner 実行方法は計算以外にも使えます。PythonSf は Python に対して upper compatible です。ですから、カーソル下の one-liner python code も計算式と同様に実行できてしまいます。これは vim/emacs エディタを Python の RITE tool： rapid iterative test and evaluation tools にできてしまうことを意味します。

さらに嬉しいことに、Python 文法の範囲で使うだけならば、無償のオープン版 PythonSf でも十分です。当然全てのソースも公開されています。以下に述べることは PythonSf オープン版でも全て成立ちます。


まずは単純な例から見てみましょう。皆様は下の Python 式各行の返す値を全て正しく答えられますでしょうか。Python を何年も使ってきた方でも、全てを正しく答えられる方は少ないと思います。

<pre class="box">
<code>
    (1,2,3)==[1,2,3]
    list((1,2,3))==[1,2,3]
    {1,2,3}==[1,2,3]
    frozenset([1,2,3])==[1,2,3]
    frozenset([1,2,3])==(1,2,3)
    a,b,c,d=1,1,2,1; a==b==d, a==b==c
    a,b,c,d=1,1,2,1; a!=b!=d, a!=b!=c, a==b!=c!=d
    id((1,2,3)) == id((1,2,3))
    id('abc') == id('abc')
</code>
</pre>

PythonSf 環境ならば各行にカーソルを持っていき、;j または ,j 操作をするだけで各式の値が得られます。print 命令なぞ入りません。

    (1,2,3)==[1,2,3]
    ===============================
    False
    
    list((1,2,3))==[1,2,3]
    ===============================
    True

    {1,2,3}==[1,2,3]
    ===============================
    False

    frozenset([1,2,3])==[1,2,3]
    ===============================
    False

    frozenset([1,2,3])==(1,2,3)
    ===============================
    False

    a,b,c,d=1,1,2,1; a==b==d, a==b==c
    ===============================
    (True, False)

    a,b,c,d=1,1,2,1; a!=b!=d, a!=b!=c, a==b!=c!=d
    ===============================
    (False, False, True)

    id((1,2,3)) == id((1,2,3))
    ===============================
    False

    id('abc') == id('abc')
    ===============================
    True

numpy array となると、もっと予測がつきにくくなります。的確に答えられる方は、もっと少なくなると思います。

    np.array([1,2,3]) == np.array([1,2,3])
    np.array([1,2,3]) == [1,2,3]
    np.array([1,2,3]) == (1,2,3)
    np.array([[1,2,3],[4,5,6],[1,2,5]]) == [1,2,3]

上の numpy python 式が vim エディタ画面にあれば、その値は ;j または ,j 操作だけで得られます。

    np.array([1,2,3]) == np.array([1,2,3])
    ===============================
    [ True  True  True]

    np.array([1,2,3]) == [1,2,3]
    ===============================
    [ True  True  True]

    np.array([1,2,3]) == (1,2,3)
    ===============================
    [ True  True  True]

    np.array([[1,2,3],[4,5,6],[1,2,5]]) == [1,2,3]
    ===============================
    [[ True  True  True]
     [False False False]
     [ True  True False]]

>PythonSf では numpy モジュールは常に np の名前で import 済みです。numpy モジュールの下にある以下の全てを np 経由で扱えます

    dir(np)
    ===============================
    ['ALLOW_THREADS', 'BUFSIZE', 'CLIP', 'ComplexWarning', 'DataSource', 'ERR_CALL', 'ERR_DEFAULT', 'ERR_DEFAULT2', 'ERR_IGNORE', 'ERR_LOG', 'ERR_PRINT', 'ERR_RAISE', 'ERR_WARN', 'FLOATING_POINT_SUPPORT', 'FPE_DIVIDEBYZERO', 'FPE_INVALID', 'FPE_OVERFLOW', 'FPE_UNDERFLOW', 'False_', 'Inf', 'Infinity', 'MAXDIMS', 'MachAr', 'ModuleDeprecationWarning', 'NAN', 'NINF', 'NZERO', 'NaN', 'PINF', 'PZERO', 'PackageLoader', 'RAISE', 'RankWarning', 'SHIFT_DIVIDEBYZERO', 'SHIFT_INVALID', 'SHIFT_OVERFLOW', 'SHIFT_UNDERFLOW', 'ScalarType', 'Tester', 'True_', 'UFUNC_BUFSIZE_DEFAULT', 'UFUNC_PYVALS_NAME', 'WRAP', '__NUMPY_SETUP__', '__all__', '__builtins__', '__config__', '__doc__', '__file__', '__git_revision__', '__name__', '__package__', '__path__', '__version__', '_import_tools', '_mat', 'abs', 'absolute', 'absolute_import', 'add', 'add_docstring', 'add_newdoc', 'add_newdoc_ufunc', 'add_newdocs', 'alen', 'all', 'allclose', 'alltrue', 'alterdot', 'amax', 'amin', 'angle', 'any', 'append', 'apply_along_axis', 'apply_over_axes', 'arange', 'arccos', 'arccosh', 'arcsin', 'arcsinh', 'arctan', 'arctan2', 'arctanh', 'argmax', 'argmin', 'argpartition', 'argsort', 'argwhere', 'around', 'array', 'array2string', 'array_equal', 'array_equiv', 'array_repr', 'array_split', 'array_str', 'asanyarray', 'asarray', 'asarray_chkfinite', 'ascontiguousarray', 'asfarray', 'asfortranarray', 'asmatrix', 'asscalar', 'atleast_1d', 'atleast_2d', 'atleast_3d', 'average', 'bartlett', 'base_repr', 'bench', 'binary_repr', 'bincount', 'bitwise_and', 'bitwise_not', 'bitwise_or', 'bitwise_xor', 'blackman', 'bmat', 'bool', 'bool8', 'bool_', 'broadcast', 'broadcast_arrays', 'busday_count', 'busday_offset', 'busdaycalendar', 'byte', 'byte_bounds', 'bytes_', 'c_', 'can_cast', 'cast', 'cdouble', 'ceil', 'cfloat', 'char', 'character', 'chararray', 'choose', 'clip', 'clongdouble', 'clongfloat', 'column_stack', 'common_type', 'compare_chararrays', 'compat', 'complex', 'complex128', 'complex64', 'complex_', 'complexfloating', 'compress', 'concatenate', 'conj', 'conjugate', 'convolve', 'copy', 'copysign', 'copyto', 'core', 'corrcoef', 'correlate', 'cos', 'cosh', 'count_nonzero', 'cov', 'cross', 'csingle', 'ctypeslib', 'cumprod', 'cumproduct', 'cumsum', 'datetime64', 'datetime_as_string', 'datetime_data', 'deg2rad', 'degrees', 'delete', 'deprecate', 'deprecate_with_doc', 'diag', 'diag_indices', 'diag_indices_from', 'diagflat', 'diagonal', 'diff', 'digitize', 'disp', 'divide', 'division', 'dot', 'double', 'dsplit', 'dstack', 'dtype', 'e', 'ediff1d', 'einsum', 'emath', 'empty', 'empty_like', 'equal', 'errstate', 'euler_gamma', 'exp', 'exp2', 'expand_dims', 'expm1', 'extract', 'eye', 'fabs', 'fastCopyAndTranspose', 'fft', 'fill_diagonal', 'find_common_type', 'finfo', 'fix', 'flatiter', 'flatnonzero', 'flexible', 'fliplr', 'flipud', 'float', 'float16', 'float32', 'float64', 'float_', 'floating', 'floor', 'floor_divide', 'fmax', 'fmin', 'fmod', 'format_parser', 'frexp', 'frombuffer', 'fromfile', 'fromfunction', 'fromiter', 'frompyfunc', 'fromregex', 'fromstring', 'full', 'full_like', 'fv', 'generic', 'genfromtxt', 'get_array_wrap', 'get_include', 'get_numarray_include', 'get_printoptions', 'getbuffer', 'getbufsize', 'geterr', 'geterrcall', 'geterrobj', 'gradient', 'greater', 'greater_equal', 'half', 'hamming', 'hanning', 'histogram', 'histogram2d', 'histogramdd', 'hsplit', 'hstack', 'hypot', 'i0', 'identity', 'iinfo', 'imag', 'in1d', 'index_exp', 'indices', 'inexact', 'inf', 'info', 'infty', 'inner', 'insert', 'int', 'int0', 'int16', 'int32', 'int64', 'int8', 'int_', 'int_asbuffer', 'intc', 'integer', 'interp', 'intersect1d', 'intp', 'invert', 'ipmt', 'irr', 'is_busday', 'isclose', 'iscomplex', 'iscomplexobj', 'isfinite', 'isfortran', 'isinf', 'isnan', 'isneginf', 'isposinf', 'isreal', 'isrealobj', 'isscalar', 'issctype', 'issubclass_', 'issubdtype', 'issubsctype', 'iterable', 'ix_', 'kaiser', 'kron', 'ldexp', 'left_shift', 'less', 'less_equal', 'lexsort', 'lib', 'linalg', 'linspace', 'little_endian', 'load', 'loads', 'loadtxt', 'log', 'log10', 'log1p', 'log2', 'logaddexp', 'logaddexp2', 'logical_and', 'logical_not', 'logical_or', 'logical_xor', 'logspace', 'long', 'longcomplex', 'longdouble', 'longfloat', 'longlong', 'lookfor', 'ma', 'mafromtxt', 'mask_indices', 'mat', 'math', 'matrix', 'matrixlib', 'max', 'maximum', 'maximum_sctype', 'may_share_memory', 'mean', 'median', 'memmap', 'meshgrid', 'mgrid', 'min', 'min_scalar_type', 'minimum', 'mintypecode', 'mirr', 'mod', 'modf', 'msort', 'multiply', 'nan', 'nan_to_num', 'nanargmax', 'nanargmin', 'nanmax', 'nanmean', 'nanmin', 'nanstd', 'nansum', 'nanvar', 'nbytes', 'ndarray', 'ndenumerate', 'ndfromtxt', 'ndim', 'ndindex', 'nditer', 'negative', 'nested_iters', 'newaxis', 'newbuffer', 'nextafter', 'nonzero', 'not_equal', 'nper', 'npv', 'number', 'obj2sctype', 'object', 'object0', 'object_', 'ogrid', 'ones', 'ones_like', 'outer', 'packbits', 'pad', 'partition', 'percentile', 'pi', 'piecewise', 'pkgload', 'place', 'pmt', 'poly', 'poly1d', 'polyadd', 'polyder', 'polydiv', 'polyfit', 'polyint', 'polymul', 'polynomial', 'polysub', 'polyval', 'power', 'ppmt', 'print_function', 'prod', 'product', 'promote_types', 'ptp', 'put', 'putmask', 'pv', 'r_', 'rad2deg', 'radians', 'random', 'rank', 'rate', 'ravel', 'ravel_multi_index', 'real', 'real_if_close', 'rec', 'recarray', 'recfromcsv', 'recfromtxt', 'reciprocal', 'record', 'remainder', 'repeat', 'require', 'reshape', 'resize', 'restoredot', 'result_type', 'right_shift', 'rint', 'roll', 'rollaxis', 'roots', 'rot90', 'round', 'round_', 'row_stack', 's_', 'safe_eval', 'save', 'savetxt', 'savez', 'savez_compressed', 'sctype2char', 'sctypeDict', 'sctypeNA', 'sctypes', 'searchsorted', 'select', 'set_numeric_ops', 'set_printoptions', 'set_string_function', 'setbufsize', 'setdiff1d', 'seterr', 'seterrcall', 'seterrobj', 'setxor1d', 'shape', 'short', 'show_config', 'sign', 'signbit', 'signedinteger', 'sin', 'sinc', 'single', 'singlecomplex', 'sinh', 'size', 'sometrue', 'sort', 'sort_complex', 'source', 'spacing', 'split', 'sqrt', 'square', 'squeeze', 'std', 'str', 'str_', 'string0', 'string_', 'subtract', 'sum', 'swapaxes', 'sys', 'take', 'tan', 'tanh', 'tensordot', 'test', 'testing', 'tile', 'timedelta64', 'trace', 'transpose', 'trapz', 'tri', 'tril', 'tril_indices', 'tril_indices_from', 'trim_zeros', 'triu', 'triu_indices', 'triu_indices_from', 'true_divide', 'trunc', 'typeDict', 'typeNA', 'typecodes', 'typename', 'ubyte', 'ufunc', 'uint', 'uint0', 'uint16', 'uint32', 'uint64', 'uint8', 'uintc', 'uintp', 'ulonglong', 'unicode', 'unicode0', 'unicode_', 'union1d', 'unique', 'unpackbits', 'unravel_index', 'unsignedinteger', 'unwrap', 'ushort', 'vander', 'var', 'vdot', 'vectorize', 'version', 'void', 'void0', 'vsplit', 'vstack', 'warnings', 'where', 'who', 'zeros', 'zeros_like']


## dir, np.info, np.source： 内部を覗き込む

dir は既に上で使いましたが、モジュールまたはクラスの内部要素をアルファベット順に表示してくれる組み込み関数です。新しいモジュールやクラスに何が備わっているかを俯瞰できます。

np.info 関数は python の help 関数の情報より荒くモジュール・クラス・関数について説明してくれます。Python の help 関数はモジュールやクラスのときに返される説明が詳細すぎて役に立たないことが多いのに np.info 関数は適度な説明文を返してくれることが多いと感じます。np.info の機能が説明されることは少ないのですが、もっと使われるべき関数です。

np.source 関数はモジュール・クラス・関数のソースを表示してくれます。

    source(source)
    In file: C:\Python27\lib\site-packages\numpy\lib\utils.py

    def source(object, output=sys.stdout):
        """
        Print or write to a file the source code for a Numpy object.

        The source code is only returned for objects written in Python. Many
        functions and classes are defined in C and will therefore not return
        useful information.

        Parameters
        ----------
        object : numpy object
            Input object. This can be any object (function, class, module, ...).
        output : file object, optional
            If `output` not supplied then source code is printed to screen
            (sys.stdout).  File object must be created with either write 'w' or
            append 'a' modes.

        See Also
        --------
        lookfor, info

        Examples
        --------
        >>> np.source(np.interp)                        #doctest: +SKIP
        In file: /usr/lib/python2.6/dist-packages/numpy/lib/function_base.py
        def interp(x, xp, fp, left=None, right=None):
            \"\"\".... (full docstring printed)\"\"\"
            if isinstance(x, (float, int, number)):
                return compiled_interp([x], xp, fp, left, right).item()
            else:
                return compiled_interp(x, xp, fp, left, right)

        The source code is only returned for objects written in Python.

        >>> np.source(np.array)                         #doctest: +SKIP
        Not available for this object.

        """
        # Local import to speed up numpy's import time.
        import inspect
        try:
            print("In file: %s\n" % inspect.getsourcefile(object), file=output)
            print(inspect.getsource(object), file=output)
        except:
            print("Not available for this object.", file=output)

    ===============================
    None


## print 命令を挿入しない one-liner print debug

git も vim/emacs エディタの日報月報テキスト上で実行できます。

これは print debug を行ったプログラムに git で戻してやることで、one-liner print debug を再現できることを意味します。


一方で Python は全世界のユーザーによって膨大な規模のモジュールが存在します。これらを利用すれば、コンピュータにできる操作の大部分を vim/emacs エディタ上で実行できてしまいます。


## 結論

以上のように PythonSf の機能を使って vim/emacs 上で Python code の RITE:rapid iterative test and evaluation が行えます。これは PythonSf を使った vim/emacs の RITE ツール化だとも言えると思います。如何でしょうか。
