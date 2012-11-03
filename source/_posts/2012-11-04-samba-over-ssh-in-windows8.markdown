---
layout: post
title: "Windows 8 上で samba over ssh"
date: 2012-11-04 04:33
comments: true
categories:
- windows
- software
---
とある事情で外からsshしかできないネットワーク上にあるsambaマシンに外からアクセスする必要があり、
普段常用しているマシンでは Windows 7 に samba over ssh のための設定をしている。

この設定は[Cifs-over-SSH for Windows Vista/7](http://www.nikhef.nl/~janjust/CifsOverSSH/VistaLoopback.html)
に書かれている。
やっていることはloopbackデバイスを作り、そのデバイスの445ポートにsshでトンネリングした出口を繋げてやるというものである。
ただ、Windows 7 では普通にネットワークの設定をするだけではsambaで使う445ポートがunbindされないため、接続できない。
詳しいことは分からないが、sambaサービスの開始を遅らせることでこれを回避しているようである。

今回Windows 8を入れたマシンでこの設定をしようとしたところ、smbというサービスがなくなっていたためscからはじまるコマンドが失敗してしまった。
だめもとで次のnetshコマンドを実行し、スケジューラ関係の設定はせずにリブートしてトンネルを開けたところ、
問題なく接続できるようになった。

というわけで、Windows 8では7よりも楽にsamba over sshの設定ができることがわかった。

