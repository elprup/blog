+++
date = "2017-01-05T03:56:51Z"
title = "Grumpy: Google开发的用Go语言运行Python的开源库"
slug = "grumpy_go_run_python_introduction"

+++

近日，Google开源博客公布了一项名为Grumpy的新项目，可以用Go语言运行Python的代码。这个项目的目的在于提高Python代码并行性能。

摘自Google博客：

> So we asked ourselves a crazy question: What if we were to implement an alternative runtime optimized for real-time serving? Once we started going down the rabbit hole, Go seemed like an obvious choice of platform since its operational characteristics align well with our use case (e.g. lightweight threads). We wanted first class language interoperability and Go’s powerful runtime type reflection system made this straightforward. Python in Go felt very natural, and so Grumpy was born.

Grumpy将Python代码翻译为Go程序，并在Go的运行环境下执行。Grumpy没有设计成一个解释器。它不实现VM，所有的Python代码将翻译成对Grumpy runtime的调用。 所以Grumpy没有Global interpreter lock，能更好的实现并行计算。

Grumpy不支持c扩展模块，而为了解决这一问题，项目支持在Python代码中直接调用Go代码，所以可以用Go代码封装C代码库在Python中调用。

    from __go__.net.http import ListenAndServe, RedirectHandler
    
    handler = RedirectHandler('http://github.com/google/grumpy', 303)
    ListenAndServe('127.0.0.1:8080', handler)
    
项目现在还在alpha阶段,实现了Python多种基本结构的支持，但很多标准库的方法和属性还没有完全支持。

Grumpy代码主要包括了三个部分，`grumpc`将Python代码翻译为Go代码，`Grumpy Runtime`对Python语言的对象用Go来重写，从而支持`grampc`生成Go代码的调用。`Grumpy Standard Library`是对Python标准库的移植。

本文介绍了Grumpy的基本信息，下一篇将介绍Grumpy库在实际使用中的体验。

Github地址：  [https://github.com/google/grumpy](https://github.com/google/grumpy)

Google博客地址：[博客地址](https://opensource.googleblog.com/2017/01/grumpy-go-running-python.html)
