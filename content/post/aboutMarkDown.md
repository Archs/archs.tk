---
title: 使用Markdown
date: '2013-03-05'
description: "things about markdown"
categories:
    - "utils"
tags:
    - "markdown"
    - "test"
---

感觉比rst简单许多，链接的使用也没有那么多的麻烦。 
docutils是一个伟大的工具，但是有些 *繁琐* 。细节的使用上
比如插入图片，虽然可以定制格式但是调整起来还是很麻烦的。 <!--more-->

# 简单的列表

关键是列表啊，md的列表很比rst好用一些

1. 这是itme1

    可插入一个段落，作为itme1的一个子段落，
    这在rst中让我遇到很多麻烦

2. Item2

    rst中的ordered list不能够被自段落分开，否则编号不连续
    最后使用的还是手写编号的形式，就跟md一样了

# 方便的标题

标题要简洁一些，而且没有要求下划线至少要不标题长，少了一些约束。

# 代码插入

来段代码吧 :)

    // New in version 2.6.
    func PyErr_WarnPy3k(message string, stacklevel int) error {
        c_message := C.CString(message)
        defer C.free(unsafe.Pointer(c_message))

        return int2err(C._gopy_PyErr_WarnPy3k(c_message, C.int(stacklevel)))
    }

    // int PyErr_CheckSignals()
    // This function interacts with Python’s signal handling. It checks whether a signal has been sent to the processes and if so, invokes the corresponding signal handler. If the signal module is supported, this can invoke a signal handler written in Python. In all cases, the default effect for SIGINT is to raise the KeyboardInterrupt exception. If an exception is raised the error indicator is set and the function returns -1; otherwise the function returns 0. The error indicator may or may not be cleared if it was previously set.
    func PyErr_CheckSignals() bool {
        return int2bool(C.PyErr_CheckSignals())
    }

markdown跟HTML的紧密结合，使网页的手动写作方便了许多。虽然在多种到处格式上比
rst还是有些差距，但是用的机会毕竟不多。

想看教程的猛击这里[Markdown中文版语法说明](http://wowubuntu.com/markdown/) :)

