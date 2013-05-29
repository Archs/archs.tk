---
title: DSL的模式和方法
date: '2013-05-29'
description:thoughts about dsl
categories:
    - ProgramDesign
tags:
    - DSL
    - Lisp
    - Go
---

最近研究Lisp，对Domain Specific Language很是产生了一些兴趣。

James Gosling曾经说过： `每个配置文件最终都会变成一门编程语言。` 这大概是
DSL之所以兴起和引人关注的一点吧。另外Martin Fowler在[《领域特定语言》][]中
强调了与领域专家的沟通作为DSL的一个主要优点和设计出发点。

广义的说HTML之于服务后端的业务逻辑实现和。net中XAML对于C#的代码来说都是
一种具体的DSL，所以说DSL促成了或者达到了UI和业务分离也不为过，这实际上
可以认为是领域专家（UI设计师）沟通的延伸，但是达成的效果是实实在在的。
上面举到的两个例子跟XML的关系是非同一般的紧密，同时也说了XML作为DSL载体
的优势。

内部和外部DSL
===================

大体上讲DSL可以分为两种。Martin Fowler说的第三种语言工作台我不是很理解。
内部DSL与宿主语言结合紧密，实际上可以认为是宿主语言的特殊形式，这一点可
能与XML的例子大相径庭，也是我刚接触LISP的时候最困惑的一点：宿主语言自身
也可以有DSL吗？

[《领域特定语言》][]中定义DSL为：`针对某一特定领域，具有受限表达性的一种
计算机程序设计语言` 。

这里如果硬性认定 **计算机程序设计语言** 的话，那么好多DSL就不能被考虑在内
了。实际上DSL的定义可以相当 **广泛** ：具有一定模式的特殊语法形式都可以
认为DSL的一种，比如Lisper经常会自行定义的特殊语法结构，有的时候只是为了完成
一部分的功能；再比如Make和Ant都被认为是DSL的典型范例，但是它们作为计算机设
计语言都是不完备的，不仅不能算作完备，甚至连完整也称不上。

外部DSL就比较常规了，感觉我们通常说的DSL就是这一种了。

内部DSL的几种方式
===================

看[《领域特定语言》][]比较有收获的一点是关于内部DSL的认识，让我长了不少见识。
内部DSL可以根据宿主语言的语法特点有针对的构造，方法（不仅仅是函数）、闭包和
Literal Map都是大有帮助的语法糖，所以在面向对象和动态类型的语言里面实现起来
都是比较便利的，当然Go也其中哦。

经典的几种方式：

+ 表达式生产器
    
    这种方式说是实在的与方法级联的形式差不多，主要在于对原有的命令式接口函数
    的组装。这里说的表达式，就是原来的命令是的接口所谓表达式，应该就是程序
    设计语言中原有的关于表达式的概念。

+ 方法级联
    
    这是常见的方式，广为人知的形式应该就是 **jQuery的链式操作** 了，很经典。不过，
    如果这都算是一种DSL的话，那么DSL的范围应该是十分广泛的。一个方法级联的例子：

        computer()
            .processor()
                .cores(2)
                .speed(2500)
                .i386()
            .disk()
                .size(150)
            .dist()
                .size(75)
                .speed(7200)
                .sata()
            .end()

    上面这种还好，不过 **jQuery的链式操作** 中噪杂的信息太多了，
    实际上应该还是专注于宿主语言的使用了。这种形式的API好像是越来越多了，
    [Google API的Go Client](https://code。google。com/p/google-api-go-client/)也多少采用了类似的形式。

+ Literal Map
    
    这种方式就要借助于宿主语言的实现方式了，支持Literal Map在脚本语言里很常见，
    最近主流的静态类型语言也开始支持了，比如C#和Go。 在Go语言中这是一个很好的
    特性，下面这段代码引自 *Go语言的Windows GUI项目[Walk][]*

        MainWindow{
        AssignTo: &mw。MainWindow，
        Title:    "Walk Actions Example"，
        MenuItems: []MenuItem{
            Menu{
                Text: "&File",
                Items: []MenuItem{
                    Action{
                        AssignTo:    &openAction,
                        Text:        "&Open",
                        Image:       "。。/img/open。png",
                        Enabled:     Bind("enabledCB。Checked"),
                        Visible:     Bind("openVisibleCB。Checked"),
                        Shortcut:    Shortcut{walk。ModControl, walk。KeyO},
                        OnTriggered: mw。openAction_Triggered,
                    },
                    Menu{
                        AssignTo: &recentMenu,
                        Text:     "Recent",
                    },
                    Separator{},
                    Action{
                        Text:        "E&xit",
                        OnTriggered: func() { walk。App()。Exit(0) },
                    },
                },
            },
            Menu{
                Text: "&Help",
                Items: []MenuItem{
                    Action{
                        AssignTo:    &showAboutBoxAction,
                        Text:        "About",
                        OnTriggered: mw。showAboutBoxAction_Triggered,
                    },
                },
            },
        },
        ...
        }

    这段代码用[Walk][]的说法是 *Declarative(声明式)* 的，但是正好使用了
    DSL中Literal Map的形式，其本身也是Go中的Literal Map。

另外还有 *嵌套函数* ， *函数序另列* 等形式，前者还好，在函数式编程中颇为常见，
后者就有点难受了，实现起来要严重的依赖全局变量，想来不是什么好方法吧:) 

程序中的几种语义模型
===================
DSL主要用来是我们的程序更加的 *灵活* ， 那么这种灵活的关键是程序中关于语法
和语义的分离。 在使用DSL的程序中建立语义模型就很关键。 下面罗列几种常见的程序
语义模型:

+ 决策表
+ 依赖网络
+ 产生式规则
+ 状态机

决策表实际上和状态机有点相似，但是实现起来略为简单一些。 依赖网络用来管理
大量数据的共生关系，可以想象一下Make中的目标和依赖，一般可以用有向无环图来
实现。 产生式规则，看起来简单，却是一个水很深的领域，早期的人工智能和应用着重
研究专家系统，而产生式系统是专家系统的基石。不过往简单的方面来考虑，使用决策树
来做产生式规则还是很直观的。



[《领域特定语言》]:http://book。douban。com/subject/21964984/
[Walk]:https://github。com/lxn/walk/blob/master/examples/actions/actions。go