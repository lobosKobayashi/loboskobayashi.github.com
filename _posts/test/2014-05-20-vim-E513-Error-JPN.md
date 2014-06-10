---
layout: post
category : editor
tagline: "Vim  E513"
tags : [vim]
# Vim エディタ E513 エラー原因と対策 

## 概要

## エラー発生までの経過
- othersMemoV.txt という私の日常メモ・ファイルに追加エディットの最中に突然書き込みできなくなり "E513: write error, conversion failed (make 'fenc' empty to overwride)" の赤い注意書きが出るようになった。":ec &fenc" を実行すると、確かに何も返してくれない。":set fenc=utf-8" を行えは ":ec &fenc" は utf-8 を返すようになるが、":w" で E513 write error を返すのに変わりはない。 ":w!" でも書き込めない。

- othersMemoV.txt ファイル自体は utf-8 で書かれており、修正前の古いファイルを開きなおすと ":ec &fenc" で "utf-8" を返してくれる。

- 日常ノートとして作業内容を残していっている schld05.txt を見直してみると "git merge --squash" を行った結果を othersMemoV.txt に貼り付けたときから "E513 write error" が出るようになっている。

もともと git 操作は日常ノート上に git コマンドを書き、";a" 操作でエディタに git コマンドを実行させています。git コマンド結果はエディタの status 行に表示させると同時に、@* レジスタにも記録されるので、残しておきたい結果については "p" Vim 操作で その結果文字列を貼り付けている [pysf.vim マクロ]() の賜物です。それが今回の原因追及を可能にしてくれました。

## 原因の特定

古い othersMemoV.txt に schdl05.txt 目もノートの git merge --squash 部分を結果文字列と一緒に貼り付けてみると、":ec &fenc" は空白に変わってしまい ":w" で "E513 write error" も再現します。

ならば git merge --squash 部分を結果文字列を少なくしていってエラーが再現する文字列を小さくしていってみると下のようになりました。

<center><b>E513 write error を発生させる原因文字列の特定</b></center>
    daily date:2014/05/02 (ﾂ凝･) time:19:23
    ↑この一行だけであかん
    ﾂ凝･
    ↑この一行だけであかん
    ･
    ↑この一文字だけであかん

"･" 文字コードが原因のようです。"ﾂ凝･" 部分は Web ページ上では再現できるか分かりませんが、私の作業ノート schdl05.txt 上では下のコードになっていました。

<center><b>"ﾂ凝･" コードを表示させるPythonSf ワン･ライナー</b></center>
    [hex(ord(ch)) for ch in "ﾂ凝･"]
    ===============================
    ['0xc2', '0x8b', '0xc3', '0xa0']

どうも LF:0xa0 が悪さをしているようです。この部分を 0xa5 に書き換えてやると "E513 write error" は発生しなくなります。このエラーが発生するようになった othersMemoV.txt は、元々 utf-8 で書いています。0xa0 の文字コードがはぃつて来て utf-8 で処理しきれなくなって Vim の日本語処理部分が &fileencoding 変数をクリアしてしまったのだろうと推測しています。文字エンコードが不明となったVim としては、":w" 書き込み動作に対して,":w!" に対してさえも、安全サイドの "E513 write error" にしたのだと推測しています。ただし私自身は 0xa0 だと "E513 write error" になって 0xa5 だとエラーにならない理由までは追求しきれていません。
、
<!その改行は Windows の "0x0d, 0x0a" を使っています。 そこに "0x0a" だけの行が突然挿入されたことにより Vim エディタが fenc の扱い方に困って勝手にクリアしてしまったものと推測されます。そして安全サイドの動作をするようになったため ":w!" でさえも書き込みできなくなったのだと思っています。
-->

ちなみに、私の .gvimrc で漢字処理に関係しそうなのは下の様に書かれている部分だけでした。

<center><b>.gvimrc で漢字処理に関係しそうな部分</b></center>
    let g:save_window_file = expand('~/_vimwinpos')
    augroup SaveWindow
      autocmd!
      autocmd VimLeavePre * call s:save_window()
      function! s:save_window()
        let options = [
          \ 'set columns=' . &columns,
          \ 'set lines=' . &lines,
          \ 'winpos ' . getwinposx() . ' ' . getwinposy(),
          \ ]
        call writefile(options, g:save_window_file)
      endfunction
    augroup END

    if filereadable(g:save_window_file)
      execute 'source' g:save_window_file
    endif

･
･

この部分を修正してやるのが根本的な修正のようです。でも その明確な解決策までには到達していません。原因の推測以上のことは時間がもったいないと判断しました。

実際の対策は "ﾂ凝･" 文字列を "ﾂ凝" 文字列に置き換えるだけで済ませました。もともと git が日本語を utf-8 決めうちしているようで、Windows での shift-jis 文字列が入りこんだ文字列を git が 0xa5 のコードに変換してしまい、それが Vim に "E513 write error" を引き起こしたと推測しています。それを確信にするまでの原因追求は私には時間がかかりすぎそうです。弥縫策として git commit 文字列に日本語を使わないルールを自分に課します。


こんな内容でも "E513 write error" で苦労している Windows Vim user には役に立つのではと思い、この記事を書いてみました。
