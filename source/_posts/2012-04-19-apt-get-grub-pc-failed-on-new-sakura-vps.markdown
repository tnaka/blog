---
layout: post
title: "Ubuntu 10.04 on 新さくらVPSでgrub-pcのapt-getに失敗する"
date: 2012-04-19 02:32
comments: true
categories:
- software
- linux
- ubuntu
- vps
---

さくらVPSでUbuntu 10.04のサーバを動かしているのだが、
プランが新しくなってメモリやHDD容量が増えたので、
旧プランのVPSからrsyncを使ってまるごと移行した。
しかし、apt-get upgradeしたところgrub-pcのpost-installation scriptだかでfailしてしまった。

dpkg-reconfigure -aしてもなおらないので調べたところ、どうやら
https://bugs.launchpad.net/ubuntu/+source/grub2/+bug/604335
のエラーらしい。
/dev/vdaの検出に失敗しているようだ。

解決法としては、上のページに添付されているパッチgrub-pc.postinst.udiffを
/var/lib/dpkg/info/grub-pc.postinstにあててやれば良い。
その後dpkg-reconfigure -aしたところ、
インストール先に/dev/vdaを選んでもなぜかパーティションに
インストールすることになるとの警告が出たものの、正常にインストールされた。

