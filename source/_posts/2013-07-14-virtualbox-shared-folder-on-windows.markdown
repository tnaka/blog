---
layout: post
title: "WindowsホストのVirtualBoxでLinuxのVMから共有フォルダで日本語ファイルを扱う"
date: 2013-07-14 18:42
comments: true
categories:
- windows
- software
- virtualbox
---

Windowsホスト上でVirtualBoxを走らせてUbuntuなど使っているわけだが、
共有フォルダを使ったときに日本語のファイルがうまく共有されなかった。
正しくエンコーディングの設定をするとうまくいくようだ。

```
sudo mount -t vboxsf shared -o iocharset=utf8,convertcp=cp932 /mnt/shared
```

