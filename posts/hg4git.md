---
title: 使用hg访问git仓库
date: '2013-04-11'
description:
categories:
    - python
tags:
    - hg
    - git
---

hg用久了，觉得git命令很别扭，高级功能很少用到，速度差别基本上
感觉不出来，哈，所以还是hg喽. 唯一的问题就在于Github，它不支持hg。
好在hg有一个hggit的扩展，有一次感叹一下python的“batter included”！

## 安装前准备

Linux下面自然不用多说，Windows下面要注意几个问题

- 删除Mercurial提供的exe或者mis安装过的独立版本
- 安装有Mingw，当然你用msvc的话下面的就可以省略了，
  不过我还没有尝试过
- 设置Python Disutils的默认编译器为Gcc
  进入 `PYTHON_HOME/Lib/distutils` 建立新文件 `distutils.cfg` 。
  输入以下内容：

        [build]
        compiler=mingw32

## 安装

安装就简单了，使用 `easy_install` 或者 `pip` 直接

    pip install mercurial
    pip install hg-git

然后配置hg加载hggit扩展，在用户目录下的 `.hgrc` 文件中加入

    [extensions]
    bookmarks =
    hggit =

然后输入 `hg help` 就能看看到hggit已加载的输出

    enabled extensions:

    hggit         push and pull from a Git server

## 使用

使用的时候就是普遍的hg的命令了，这是相当happy的。
要注意的是git路径到hggit的转换，比如github中ssh方式访问
`git@github.com:username/reposname.git` 要换成
`git+ssh://git@github.com:username/reposname.git` 就可以了

还有一个推送已有hg库到git库的方法：

    $ cd hg-git # (a Mercurial repository)
    $ hg bookmark -r default master # make a bookmark of master for default, so a ref gets created
    $ hg push git+ssh://git@github.com/schacon/hg-git.git
    $ hg push

# Enjoy you Git Now!
