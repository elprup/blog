+++
date = "2010-06-18T07:25:59Z"
title = "getline处理windows和unix换行符时的差异"
slug = "getline_between_windows_unix"

+++

getline 用 \n 作为分隔符。因为windows下的换行为\r\n，unix下为\n，所以，当unix环境处理windwos文件时，getline的字符串会带\r.


