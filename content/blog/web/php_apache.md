+++
date = "2009-03-06T07:01:46Z"
title = "php配置"
slug = "php_config"
+++

1、软件
apache2.0.58
php 5.1.2

2、首先安装apache，一路next，记住要在安装之前保证80端口没有被占用。
3、配置apache和php的集成
将php安装目录下的php.ini-dist文件改名为php.ini
并将：
extension=php_mbstring.dll
extension=php_dbase.dll
extension=php_gd2.dll
extension=php_mysql.dll
这些扩展前面的分号去掉,也就是去掉注释。
修改extension_dir为你的php目录下的ext文件夹
extension_dir = “d:/php/php/ext/”

修改好php.ini文件后：将php安装目录下的php.ini和libmysql.dll两个文件拷贝到C:\WINDOWS目录下

配置apache，修改http.conf
主要修改：
1：修改DocumentRoot
改后的值：DocumentRoot “D:/php/webroot”
2：修改Directory
改后的值：
3：增加php服务功能
在ScriptAlias下面加上：
ScriptAlias /cgi-bin/ “D:/php/Apache2/cgi-bin/”
ScriptAlias /php/ “d:/php/php/”
addtype application/x-httpd-php .php
action application/x-httpd-php “/php/php.exe”

4：加载模块
在Dynamic Shared Object (DSO) Support下面加入：
LoadModule php5_module “d:/php/php/php5apache2.dll”

重启apache就可以了。

详细设置待修改
