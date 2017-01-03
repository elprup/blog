+++
date = "2010-06-20T07:24:32Z"
title = "mysql python 的字符编码问题"
slug = "mysql_charset"

+++
mysql python字符集

在以下几处有字符集设置

python 文件编码 ：指存储py文件的编码
python 代码编码：指赋值字符串时的默认编码

mysqldb connection对象：mysql db连接器代码。这与mysql connection是有区别的。举例来说，在mysql的表中将mysql connection的编码设定为utf8，申请一个不带编码参数的mysqldb 的connection, 它的编码还是latin-1。
mysql 数据库配置的编码：mysql的编码。
