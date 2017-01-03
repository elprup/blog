+++
date = "2011-05-23T11:13:37Z"
title = "python 装饰函数的理解"
slug = "python_wrapper"

+++

python 装饰函数是指这样一种神奇的函数。你在某个函数声明之前添加一行@func()之类的语句，于是，你可以对某个函数的各种行为进行编程。你可以在函数执行前后执行操作，改变函数接受的值，甚至可以完全忽略某个函数而执行其他的函数。我们把这样的函数称为装饰函数。装饰函数困扰了我很久的时间，特别是从C语言函数的角度，你是很难理解python函数可以作为一个对象进行操作这个概念。 

其实，装饰函数并没有那么高深，只是需要我们一步一步慢慢来。 

事实1 python中函数是一个对象，函数调用对应的是，调用这个对象的`__call__`方法。 

    class foo(): 
        pass 

a=foo 
a() # 失败，显示foo不能被调用( not callable) 

    class foowithcall(): 
        def __call__(self): 
            print 'hello' 

b=foowithcall 
b() # 成功 
从上面的例子我们看出，foo本身只是一个对象，而如果我们为它加上了call方法，它就变成了类似于函数的对象。 

事实2 嵌套函数定义中，外层声明变量对内层函数可见 
    def foo(): 
        i = 88 
        def bar(): 
            print i 
            return 
        return bar() 

foo() # 打印88 


============================================================================ 


基于以上两个事实，我们就可以开始编写自己的装饰函数了。首先介绍一下@符号。这个符号加在函数的前面，系统自动的将之下的函数作为参数对象传递给@后的方法。 

    def foo() 
        def atholder(f): 
            print f 
            return f 
        return atholder 
    
    @foo() 
    def hello(): 
        print 'hello' 
    
    hello() 

难点:这时，系统遇到带@方法的函数，将hello函数对象（并不包括hello后面的括号）作为参数传递给foo（）返回的函数。（请仔细体会这句），执行后得到了return f这个函数对象，在调用这个对象上的call方法。 

而我们在atholder里面就可以对f进行加工了，最常见的莫过于返回一个函数对象，其中包括了f的操作。 

装饰函数 
    def foo(): 
        def atholder(f): 
            def func(*args,**kwargs): 
                print 'before' 
                ret = f(*args,**kwargs) 
                print 'after' 
                return ret 
            return func 
        return atholder 
    
    @foo() 
    def hello(): 
        print 'hello' 

这是一个标准的对hello函数的装饰。其实就是将hello函数对象作为参数传给了atholder后，atholder返回了另一个函数对象，这个函数对象天生的知道了hello对象的存在，并且可以接收参数，再传递给hello对象。 

  ========================================================================================== 
    ||   Ecclesiastes 1:14 我 见 日 光 之 下 所 作 的 一 切 事 ， 都 是 虚 空 ， 都 是 捕 风 
      ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ 7
