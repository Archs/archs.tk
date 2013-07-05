---
title: 使用Mingw Build Lua 的注意事项
date: '2013-07-05'
description:
categories:
    - tips
tags:
    - Lua
    - Mingw
    - gcc
---

1. Lua的扩展库使用LUA_LIB宏定义

    在c文件中引入lua的头文件之前加入：

        #define LUA_LIB

2. 使用windows库的时候，需要在gcc命令行中引入库的顺序

    这样
        
        gcc -lwsock32 ws_c_file.c

    可能会造成winsock函数未定义应用的问题，最好使用
        
        gcc ws_c_file.c -lwsock32 

    这个问题相当诡异，简直不知道怎么说是好。