+++
date = "2009-09-22T11:00:42Z"
title = "Windows环境下CVS管理的文件转换为正常文件"
slug = "cvs_convert"

+++

#Windows环境下CVS管理的文件转换为正常文件
级别： 初级 

作者：elprup 

2009 年 9 月 22 日 

##CVS 介绍和问题描述 

CVS 是 Concurrent Version System（并行版本系统）的缩写，用于版本管理。因为项目需要，一般都会对 CVS 管理的文件进行备份。CVS中的文件是CVS格式的。用CVS保存的文件会和原始文件有很大变化。不仅后缀名会添加“，V”字符，而且其内容中也会添加CVS系统的内容。 
所以，用CVS管理的文件变的无法使用，我们用安装CVS服务器，客户端的方法恢复文件的本来面目。 

##问题解决方案 

首先，安装CVS的服务器端和客户端程序。本文都是在WindowsXP环境下进行的。服务器的版本是CVSNT，客户端的版本是TortoiseCVS。 
下载地址如下： 
CVSNT： http://www.cvsnt.org/downhtml_255.php 
TortoiseCVS： http://www.tortoisecvs.org/download.shtml 

然后，可以参考玩转CVSNT+TortoiseCVS 版本控制系统（图解）一文来配置，文章地址为：http://lavasoft.blog.51cto.com/62575/19274 

特别注意的是，CVSNT在选择仓库的窗口repository configuration中添加需要转换的文件夹的上层目录。例如文件夹是C:\myfile\source，应填写C:\myfile。因为这个目录将作为仓库的主目录，而source作为项目名存储。 
TortoiseCVS中添加新模块中的仓库文件夹中需要填写repository configuration中设置的文件夹的name。 

最后，创建完新模块后，再进行CVS的更新，这样，这个文件夹中更新的文件就是正常格式了。 

##结束语 
CVS管理文件转换可以用以上的方法解决，也可以用软件如CVS2SVN这样的文件解决，但本人没有尝试，如果是海量文件，并且需要转换到新的CVS系统格式，可以尝试别的办法。 

##参考文档 

玩转CVSNT+TortoiseCVS 版本控制系统 作者：熔岩
