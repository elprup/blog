+++
date = "2010-06-17T07:28:25Z"
title = "django template 重点摘要"
slug = "django_template_notes"

+++
用两个大括号括起来的文字（例如 {{ person_name }} ）是 变量(variable)

被大括号和百分号包围的文本(例如 {% if ordered_warranty %} )是 模板标签(template tag) 。标签(tag)定义比较明确，即：仅通知模板系统完成某些工作的标签。

最后，这个模板的第二段落有一个 filter 过滤器的例子,它能让你用来转换变量的输出， 在这个例子中, {{ship_date|date:"F j, Y" }} 将变量 ship_date 用 date 过滤器来转换,转换的参数是 "F j, Y" . date 过滤器根据指定的参数进行格式输 出.过滤器是用管道字符( | )来调用的,就和Unix管道一样.

Template 类就在 django.template 模块中

输入命令 python manage.py shell 启动交互界面

调用 Template 对象 的 render() 方法并传递context来填充模板

Python的字典数据类型就是关键字和它们值的一个映射。 Context 和字典很类似

注意到我们使用了三个引号来 标识这些文本，因为这样可以包含多行。这是Python的一个语法。

Python列表类型的索引是从0开始的，第一个元素的索引是0，第二个是1，以此类推。
句点查找规则可概括为：当模板系统在变量名中遇到点时，按照以下顺序尝试进行查找：
    •    字典类型查找 （比如 foo["bar"] ) 
    •    属性查找 (比如 foo.bar ) 
    •    方法调用 （比如 foo.bar() ) 
    •    列表类型索引查找 (比如 foo[bar] ) 
在方法查找过程中，如果某方法抛出一个异常，除非该异常有一个 silent_variable_failure 属性并且值为 True ，否则的话它将被传播。如果该异常 确有 属性 silent_variable_failure ，那么（所查找）变量将被渲染为空字符串

显然，有些方法是有副作用的，好的情况下允许模板系统访问它们可能只是干件蠢事，坏的情况下甚至会引发安全漏洞。
例如，你的一个 BankAccount 对象有一个 delete() 方法。不应该允许模板包含像 {{account.delete}} 这样的方法调用。
要防止这样的事情发生，必须设置该方法的 alters_data 函数属性

{% if %} 标签检查(evaluate)一个变量，如果这个变量为真（即，变量存在，非空，不是布尔值假），系统会显示在 {% if %} 和 {% endif %} 之间的任何内容

{% if %} 标签接受 and ， or 或者 not 关键字来对多个变量做判断 ，或者对变量取反（ not )

{% if %} 标签不允许在同一个标签中同时使用 and 和 or ，因为逻辑上可能模糊的

系统不支持用圆括号来组合比较操作

{% for %} 允许我们在一个序列上迭代。与Python的 for 语句的情形类似，循环语法是 for X in Y ，Y是要迭代的序列而X是在每一个特定的循环中使用的变量名称

给标签增加一个 reversed 使得该列表被反向迭代

Django不支持退出循环操作。如果我们想退出循环，可以改变正在迭代的变量，让其仅仅包含需要迭代的项目

forloop.counter 总是一个表示当前循环的执行次数的整数计数器。这个计数器是从1开始的，所以在第一次循环时 forloop.counter 将会被设置为1

forloop.counter0 类似于 forloop.counter 

forloop.revcounter 是表示循环中剩余项的整型变量

forloop.revcounter0 类似于 forloop.revcounter ，但它以0做为结束索引。

forloop.first 是一个布尔值。在第一次执行循环时该变量为True，在下面的情形中这个变量是很有用的。

forloop.last 是一个布尔值；在最后一次执行循环时被置为True。

forloop.parentloop 是一个指向当前循环的上一级循环的 forloop 对象的引用（在嵌套循环的情况下）

Django模板系统压根儿就没想过实现一个全功能的编程语言，所以它不允许我们在模板中执行Python的语句

