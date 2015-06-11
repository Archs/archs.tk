---
title: "Embed ChickenScheme In Go"
date: '2013-04-10'
description:
categories:
    - go
tags:
    - scheme
    - Chicken
    - cgo
    - go
---

使用cgo加载c库的主要麻烦在于cgo不识别c头文件中的宏
而chicken scheme初始化的时候要调用一个全局函数CHICKEN_default_toplevel。
在cgo中使用 `void* dfp = CHICKEN_default_toplevel;` 可以直接声明。<!--more-->

    package main

    /*
    #cgo windows CFLAGS: -D_GUN_SOURCE -I"c:/my-chicken/include/chicken" -DHAVE_CHICKEN_CONFIG_H -DC_ENABLE_PTABLES
    #cgo windows LDFLAGS: -L"c:/my-chicken/lib" -lchicken -lm

    #include <chicken.h>

    void* dfp = CHICKEN_default_toplevel;
    */
    import "C"
    import (
        "unsafe"
    )

    func main() {
        src := "(+ 1 2 3)"
        nSrc := 10
        ret := make([]byte, nSrc)
        p := (*C.char)(unsafe.Pointer(&ret[0]))
        C.CHICKEN_run(C.dfp)
        C.CHICKEN_eval_string_to_string(C.CString(src), p, C.int(nSrc))
        println(string(ret))
    }

最近才发现，cgo不仅可以识别注释中的c语句，对于相同目录下面的*.c文件也可
自动编译，呵呵，像sqlite之类的库就可以静态的形式直接使用了。