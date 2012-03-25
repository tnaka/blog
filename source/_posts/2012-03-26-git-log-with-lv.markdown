---
layout: post
title: "lvインストール後のgit log文字化け"
date: 2012-03-26 04:34
comments: true
categories:
- git
- software
---
ページャにはいろいろなエンコーディングに対応したlvが良いと聞き、
早速インストールしたが、git logで色を付けるコントロールシーケンスが
うまく解釈されず、そのままゴミになって表示されてしまうようになった。

対処としては、gitのページャの設定を変える。
``` bash
git config --global core.pager "lv -c"
```
として、lv側でANSIのエスケープシーケンスを認識するオプションを有効にしてやると、
きちんと表示されるようになる。
