---
layout: post
category : PythonSf
tagline: "Supporting tagline"
tags : [PythonSf, Emacs]
---

## 概要

PythonSf を使えば vim や emacs といったエディタを Matlab や Mathematica 以上に有用な計算ツールに変貌させられます。PythonSf と使い慣れたエディタを使って効率的に計算作業をコンピュータに行わせましょう。


## Emacs での pysf.el script 組み込み
Emacs エディタで PythonSf 計算をさせるには pysf.el script を Emacs に組み込みます。その手順は簡単です。Git で配布している pysf.el を ディレクトリの ..\vim73\plugin\に置くだけです。キー･バインドは pysf.vim にハード･コーディングされています。

![pysf.el を .emacs.d\elisp\ へ](/images/2013/pysfElToElisp.png "pysf.el のコピー")

<!--
//@@
    Git                  .emacs.d\elisp\
    +------------+       +---------------+  
    |            | copy  |               |  
    |  pysf.el   +---- ->|  pysf.el      |  
    |            |       |               |  
    +------------+       +---------------+
                         
//@@@
//java -jar \utl\ditaa0_9.jar __tmp pysfElToElisp.png
-->

    ; 下のキー・バインディングを .emacs.d\init.el に追加する
    (require 'pysf)
    (global-set-key (kbd "C-; C-j") 'ExecPySf_1liner)       ;PythonSf one-liner
    (global-set-key (kbd "C-; C-a") 'Exec_command)          ;shell command one-liner
    (global-set-key (kbd "C-; C-f") 'Exec_start)            ;shell command one-liner with start
    (global-set-key (kbd "C-, C-j") 'ExecPySfOp_1liner)   ;PythonSfOp one-liner
    (global-set-key (kbd "C-; C-k") 'ExecSf_Block)          ;PythonSf Block
    (global-set-key (kbd "C-; C-p") 'ExecPy_Block)          ;PythonSf Block
    (global-set-key (kbd "C-, C-k") 'ExecSfOp_Block)      ;PythonSfOp Block

    (global-set-key (kbd "C-, C-g") 'ConvertAlpbt2Greek)    ;Convert alpahbet to Greek

