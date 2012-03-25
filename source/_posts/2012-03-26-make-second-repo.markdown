---
layout: post
title: "octopressの編集用リポジトリを別の場所に作る"
date: 2012-03-26 03:52
comments: true
categories:
- octopress
- software
---
せっかくgitを使っているんだから、別のPCでもblogに投稿できるとうれしいと思って試してみた。

このblogは一つのリポジトリにgh-pagesブランチとmasterブランチがあり、
masterブランチの方にoctopressのルートをコミットし、
gh-pagesの方に生成されたhtmlがコミットされるようになっている。
一つにまとめたかったのでこのような構成にしたが、
これからやる人はbitbucketかなにかにソース用として
別のリポジトリを作ったほうが、こんがらがることがなくて良いと思う。

で、当然rubyの設定はしておくとして、その後適当なディレクトリで
	git clone git@github.com:tnaka/blog.git blog
としてソース類を取ってくる。ここで、カレントブランチがgh-pagesを取ってきている場合は、
	git checkout -b master origin/master
して、masterをcheckoutしておく。
その後、
	gem install bundler
	rbenv rehash
	bundle install
	rake setup_github_pages
として、ここでリポジトリの書き込み可能なurlを指定する。
ここでpostを作成し、
	rake gen_deploy
すれば問題なくpushされた。
ただ、gh-pagesブランチを強制的に上書きしているようで、
git logがcloneしてからのものとなってしまう。
どうにかならないものか。

その後、
	git add .
	git commit
	git push origin master
すると、sourceがmasterにコミットされる。

別のリポジトリでは、
	git pull origin master
すれば他所での編集もマージされるっぽい。

とりあえずはこの方法で様子見することにする。
