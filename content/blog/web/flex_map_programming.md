+++
date = "2009-06-17T08:00:14Z"
title = "Google Maps API 中的 标注编程"
slug = "flex_map_programming"

+++

Google Maps API 除了提供能被 Ajax 应用程序调用的 JavaScript 接口之外，还提供了能被 Flash 应用程序调用的 Flex 语言接口。本文就将介绍如何用 Google Maps API for Flash 来开发基于 Flash 的地图应用程序中，关于地图标注的部分。 

## Flex 简介 

Flex 是 Abode 公司贡献的一个开源框架，用于快速构建和维护高度交互并具有丰富表现力的互联网应用( Rich Internet Application )。这些 RIA 应用程序既可以通过 Adobe Flash Player 部署在所有主流的浏览器上，也可以通过 AIR(Adobe Integrated Runtime) 部署在所有主流操作系统的桌面上。 

一个 Flex 应用程序通常有 ActionScript 和 MXML 两种代码组成。ActionScript 是一种面向对象的脚本语言，符合 ECMAScript 标准，用于实现 Flex 应用程序的逻辑；MXML 是一种类似于 HTML 和 XML 的标记语言，用于构建 Flex 应用程序的用户界面。Flex 应用程序最终需要被编译成 SWF 文件以运行在 Adobe Flash Player 或 AIR 上。 
本文并不详细讲述 MXML 和 ActionScript 的语法，如想了解这两种语言的用法，可以查看 参考资源 中的相关教程。 

## Google Maps API for Flash 简介 

Google Maps API for Flash 使得 Flex 开发人员能够把 Google Map 嵌入到 Flex 应用程序中。该 API 提供了很多实用的工具用来操控 Google Map。利用该 API，您可以处理地图事件、添加地图控件、在地图上添加标记、或者在地图上面画线，也可以利用 Geocoding 将地址转换成经纬度数据。笔者写作期间 Google Maps API for Flash 还不支持 AIR 环境，只能在 Flash Player 里运行。 
开始创建简单的地图应用 
这里可以参考 
http://www.ibm.com/developerworks/cn/web/wa-lo-flexgoogle/ 
写的一个教程，这个教程详细介绍了地图应用的创建，并且有实例。 

## 在地图上创建标注 

首先简单的介绍一下google maps在Flash环境下的标注。标注是作为一个overlay加入应用程序的，英文名为marker，可以再googleAPI中找到http://code.google.com/apis/maps/documentation/flash/reference.html#Marker 
所以我们的任务就是加入一个marker的overlay。下面一段代码就是创建这个marker的overlay的： 
Mxml代码 
```
    1.    var markerA:Marker = new Marker(  
    2.                    new LatLng(48.858842, 2.346997),  
    3.                    new MarkerOptions({  
    4.                        //strokeStyle: new StrokeStyle({color: 0x987654}),  
    5.                        //fillStyle: new FillStyle({color: 0x223344, alpha: 0.8}),  
    6.                        //radius: 12,  
    7.                        //hasShadow: true  
    8.                    }));  
    9.                    googleMap.addOverlay(markerA);  
```
首先创建一个marker对象，其中latlng是googlemap自己定义的表明位置的对象，markerOption用于初始化marker的信息，可以为空，具体的信息可以查找googlemapsapi。最后是把这个图层加入到我们创建好的googlemap对象中。这样，就可以看见图上有标注了。 

## 捕捉标注点击动作 

创建标记后，点击标记没有任何反应，于是需要我们来创建一个标记的事件处理函数。marker可以用addEventListener的方法来创建处理鼠标事件的函数。这个函数的第一个参数是鼠标事件，这里特别注意，需要用MapMouseEvent中的动作定义，不能直接写flash中的事件定义。然后是事件处理函数。因为处理函数需要知道marker对象，用来打开marker的窗口，marker需要作为参数传递过去。然而，默认的addEventListener不能传递除鼠标事件外的参数。我在网上查了一下，需要用一下的方式解决，详细的方法可以参考：http://www.flashj.cn/wp/flashria/addeventlistener事件监听传递参数/ 
Mxml代码 
```
    1.    markerA.addEventListener(MapMouseEvent.CLICK, function(e:MapMouseEvent){mark_click(e,markerA)});  
    2.    private function mark_click(evt:MapMouseEvent,mark:Marker):void{  
    3.                    mark.openInfoWindow(new InfoWindowOptions({title: "Hello", content: "这是我在丹东的家"}));  
    4.                }  
```
mark_click就是事件处理函数，这样，就可以实现对事件的响应了。 

## 结束语 

现在关于GoogleMapsAPI的资料真难找，实现这样的简单功能就用了半天的时间。要多多加油了。看文档要仔细，这样会避免因为字面意思的理解导致的时间的浪费。代码见附件。希望对大家有所帮助。 

