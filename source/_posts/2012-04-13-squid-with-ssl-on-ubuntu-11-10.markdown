---
layout: post
title: "ubuntu 11.10でsslサポート付きのsquidを使う"
date: 2012-04-13 01:44
comments: true
categories:
---

ubuntu 11.10でapt-getして入るsquidにはsslサポートが付いていないので、
巷の記事を読んでhttps\_portなどをsquid.confに書いたところで
https接続は有効にならない。
そこでこれまた巷の記事を参考にsquidをソースからコンパイルして入れてみた。

#手順
まずはソースを取ってくる。

``` bash
apt-get source squid #エラーが出たらdevscripts等をインストール
apt-get build-dep squid
apt-get install libssl-dev
cd squid-2.7.STABLE9
```

ここでビルドの設定を変更。
debian/rulesのConfigure the package以下のそれっぽい場所に--enable-sslを追加する。

また、このままではSSLv2なんたらが見付からないとのエラーが出る。
opensslにはSSLv2は実装されていないようなので、パッチを当てる。
``` diff
--- src/ssl_support.c.orig      2012-04-13 01:37:21.661625364 +0900
+++ src/ssl_support.c   2012-04-13 01:39:08.857629352 +0900
@@ -446,10 +446,12 @@
     ERR_clear_error();
     debug(83, 1) ("Initialising SSL.\n");
     switch (version) {
+#ifndef OPENSSL_NO_SSL2
     case 2:
        debug(83, 5) ("Using SSLv2.\n");
        method = SSLv2_server_method();
        break;
+#endif
     case 3:
        debug(83, 5) ("Using SSLv3.\n");
        method = SSLv3_server_method();
@@ -609,10 +611,12 @@
     ERR_clear_error();
     debug(83, 1) ("Initialising SSL.\n");
     switch (version) {
+#ifndef OPENSSL_NO_SSL2
     case 2:
        debug(83, 5) ("Using SSLv2.\n");
        method = SSLv2_client_method();
        break;
+#endif
     case 3:
        debug(83, 5) ("Using SSLv3.\n");
        method = SSLv3_client_method();
```

さらに、
``` bash
./configure
debuild -us -uc -b
cd ..
sudo dpkg -i squid_2.7.STABLE9-4ubuntu4_amd64.deb squid-common_2.7.STABLE9-4ubuntu4_all.deb
```

これでOK。

