---
layout: post
title: "vsftpdのlogに日本語ファイル名を正常に出力する"
date: 2013-10-29 13:43
comments: true
categories:
- ubuntu
- software
- vsftpd
- ftp
---

Ubuntu 12.04にvsftpdをインストールしてみたが、/var/log/vsftpd.logを覗いてみたところ日本語ファイル名が正しく出力されていないことに気づいた。
vsftpdではログへのマルチバイト文字の出力ができないらしく、全て?に置換されてしまっている。

少々調べてみたところ、ソースを変更すれば良いということだったので、ソースをダウンロードしてきてmakeし、debを作ってインストールすることにした。

``` bash
cd src #ソースが展開されるので適当な場所にcd
sudo apt-get update
apt-get source vsftpd
sudo apt-get build-dep vsftpd #make/installに必要なpackageをインストール
cd vsftpd-*
```

ここで、logging.cの中の
``` c
str_replace_unprintable(p_str, '?');
```
の行をコメントアウトしてから、
``` bash
debchange -v 2.3.5-1ubuntu2+mblog #versionを変更
#エディタが起動するので、コメントをかきこんで保存
debuild -us -uc -b #deb作成
cd ..
sudo apt-get remove vsftpd #既存のvsftpdを削除
sudo dpkg -i vsftpd_2.3.5-1ubuntu2+mblog_amd64.deb
```
としてインストールする。
debchangeがない場合は apt-get install devscripts すると入る。
fakerootもないとだめかもしれない。

これでutf-8のファイル名ならばログにutf-8のまま出力されるようになったはずなので、確認する。

ついでに、勝手にアップデートされないようにバージョンを固定しておく。
``` bash
echo vsftpd hold | sudo dpkg --set-selections
sudo dpkg --get-selections vsftpd #holdになっているか確認
#echo vsftpd install | sudo dpkg --set-selections で解除できる
```

