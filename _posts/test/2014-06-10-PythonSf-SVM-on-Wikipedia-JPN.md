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

##■■ 前提
sfCrrntIni.py

大部分の計算が one-liner でできてしまう。

##■■ Machine Learning の薦め
たんなる流行以上のもの
後々まで実用的な道具として広く使われる

### PythonSf one-liner
sklearn モジュールを使った計算を電卓なみの手軽さで実行できる

##■■ Support vector machine -- Wikipedia
http://en.wikipedia.org/wiki/Support_vector_machine
↑ 日本語は情報が少なすぎ;;http://ja.wikipedia.org/wiki/サポートベクターマシン


### 分離超平面: \\(w \cdot x - b = \pm 1\\) 

試し \\(w \cdot x - b = \pm 1\\) です

\\[
w \cdot x - b = \pm 1 \tag{1}
\\]

svm.LinearSVC(..)
svv.SVC(.)
