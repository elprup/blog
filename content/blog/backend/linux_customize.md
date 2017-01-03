+++
title = "Linux配置手册v0.1"
slug = "linux_customize"
date = "2010-10-22T20:25:30Z"

+++

1. vim 配色方案
vim默认的配色方案有一点极不爽，就是注释，默认是蓝色的，在黑色背景下几乎看不清是什么东西。

倒腾了很多天，终于找到了解决办法，只要：

$ vi /.vimrc

加一行

colorscheme murphy

保存退出就好了。

如果觉得murphy配色方案不好，可以看一下/usr/share/vim/vim63/colors，那里保存着很多的配色方案，你可以任选一个替换上面的murphy。

