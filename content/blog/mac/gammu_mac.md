+++
title = "Mac上用Gammu库控制Nokia 6120C发送短信"
date = "2016-01-18T15:50:00+08:00"
#Modified: 2016-01-18 15:50
categories = ["Phone"]
tags = ["phone"]
#Slug: gammu-mac-nokia-send-message
#Author: elprup
#Summary: 如何用Mac控制Nokia手机发送短信
+++

[Gammu](http://bandungwebdeveloper.blogspot.com/2014/06/installing-gammu-on-mac-os-x-109.html)是一个激动人心的库。它支持了多种手机的控制功能，利用蓝牙。你可以轻松的通过你的电脑（linux，mac，windows）控制你的手机。现在就用Mac和Nokia 6120c为例。看看如何做到这样的功能。

## 安装gammu
在mac上打开命令行

	brew install cmake
	brew install gammu
	
在.gammurc中配置了蓝牙地址后连接手机，尽管可以运行gammu，但是会报错

	gammu --identify
	需求的函数在编译期间已经关闭了。
	
这显然是因为支持的库没有全部安装。只能从源码开始安装了。

下载最新源码1.36.8后解压，进入源码目录后开始编译。

	mkdir build
	cd build
	cmake .. -DWITH_NOKIA_SUPPORT=ON -DWITH_BLUETOOTH=ON
	make

在编译过程中出现错误：

	[ 18%] Building C object libgammu/CMakeFiles/libGammu.dir/device/proxy/proxy.c.o
	/Users/uniqme/gammu/gammu-1.36.8/libgammu/device/proxy/proxy.c:169:12: error: implicit declaration of function 'sigtimedwait' is
      invalid in C99 [-Werror,-Wimplicit-function-declaration]
    while (sigtimedwait(&newset, &si, &ts)>=0 || errno != EAGAIN);
           ^
	1 error generated.
	make[2]: *** [libgammu/CMakeFiles/libGammu.dir/device/proxy/proxy.c.o] Error 1
	make[1]: *** [libgammu/CMakeFiles/libGammu.dir/all] Error 2
	make: *** [all] Error 2
	
这可能是编译器使用版本的问题。一些编译器是将此类问题作为warning的，但在C99标准下，这被处理成为错误。那么升级编译器试试了。但调研深入发现这个函数在Mac OS下似乎是不支持的，所以才会找不到。悲剧过后，只能找降级版本碰碰运气了。找到gammu 1.36.5 (homebrew 提交版本）
编译后可以安装，但还是会有同意的错误。用gammu打印支持的feature列表，发现支持的协议里好像没有蓝牙

```
elprupdeMac-mini:~ uniqme$ gammu features
Compiled in features:
 * 协议
  - MBUS2
  - FBUS2
  - FBUS2DLR3
  - FBUS2PL2303
  - DKU2PHONET
  - DKU2AT
  - DKU5FBUS2
  - AT
  - ALCABUS
 * 手机
  - NOKIA650
  - NOKIA3320
  - NOKIA6110
  - NOKIA7110
  - NOKIA9210
  - NOKIA6510
  - NOKIA3650
  - DCT3
  - DCT4
  - ATGEN
  - ALCATEL
  - OBEXGEN
  - GNAPGEN
  - DUMMY
 * Miscellaneous
  - CELLBROADCAST
  - BACKUP
  - GETTEXT
  - ICONV
  - CURL
  - GLIB
```

在看库用到的蓝牙库是 bluez，bluez是不支持Mac系统的。看来如果能够连接，唯一的可能性就是fbusdlr3方式了。用usb线连接Mac后，发现并不支持，只能用at19200方式尝试一下，还真的成功了。

```
~/.gammurc内容
[gammu]
port = /dev/tty.usbmodem1419
connection = at19200
model = Nokia

elprupdeMac-mini:~ uniqme$ gammu --identify
设备               : /dev/tty.usbmodem1419
制造商            : Nokia
型号               : unknown (Nokia 6120c)
固件               : V 06.01,17-09-08
IMEI                 : 略
SIM IMSI             : 略
```

注意：在手机上需要把usb连接模式设置为PC套件。根据文档，AT19200模式除了看一下info以外，什么都干不了。但我不小心尝试了一下，还真能发短信。（除了速度极其慢，而且没有反馈外）

```
gammu sendsms TEXT 13800138000 -text "Halooo .."
If you want break, press Ctrl+C...
Sending SMS 1/1....waiting for network answer
```

## 总结
gammu对mac的支持很小，作者的意思大概就是mac也算是linux，你就看着办吧。Mac版的gammu首先新版的编译会不通过，然后之前版本即使编译通过也不能用蓝牙连接。推荐有兴趣的同学还是自行绕道，用linux原生系统体验gammu吧。
