---
title: GIT的奇技淫巧--修改历史
date: '2013-07-13'
description:
categories:
	- tips
tags:
	- git
---

使用版本控制系统这么久，被GIT震惊了， 简直是无所不能啊，下面就是
一个修改历史版本中所有用户信息的命令，叹为观止。

	git filter-branch --commit-filter '
	       if [ "$GIT_AUTHOR_EMAIL" = "previous@email.address" ];
	       then
	               GIT_AUTHOR_NAME="NowName";
	               GIT_AUTHOR_EMAIL="now@email.address";
	               git commit-tree "$@";
	       else
	               git commit-tree "$@";
	       fi' HEAD