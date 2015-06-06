---
title: "在TinyCore中取代BusyBox"
date: '2013-04-18'
description:
categories:
    - tips
tags:
    - TinyCore
    - Linux
---

TinyCore为了最小化体积，使用BusyBox构建基本的Shell环境，所以不是
完全的 `GNU compatible` ，在编译软件的时候可能造成一定的困扰。如果
在TinyCore下面编译软件遇到问题，可注意下兼容性问题。<!--more-->

使用
    
    tce-load -wo coreutils util-linux

以OnDemand的方式安装 `coreutils` 和 `util-linux` ，下载必须软件包。
使用的时候

    tce-load -i coreutils util-linux

即可。参见[TinyCore Wiki](http://tinycorelinux.net/faq.html#compatibility)