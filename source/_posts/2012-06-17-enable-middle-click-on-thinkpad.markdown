---
layout: post
title: "最近のthinkpadでミドルクリックを有効にする"
date: 2012-06-17 22:39
comments: true
categories:
- thinkpad
- windows
---

Thinkpadのトラックポイントは、Linux上ではドライバのできが良いのか
中クリックと上下左右スクロールを両立できる。
Windowsの標準ドライバではこの共存はできないが、
ドライバ側の中クリックスクロールを無効にして
TrackWheelというソフトを入れることで、
両立することができる。

しかし、この間買ったThinkpad X220tのWindows環境では、
ドライバの設定で中ボタンドラッグによるスクロールを解除しても
中クリックが有効にならなかった。
T61pでは上記の設定で普通に動いているので、
トラックポイントのドライバのバージョンが新しいとこの問題が起きるらしい。

SynTPEnh.exeを終了しておくと中ボタンが効くようになったので、
msconfigで起動時にSynTPEnh.exeを実行しないように設定した。
