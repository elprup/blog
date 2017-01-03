+++
date = "2009-09-24T11:09:07Z"
title = "Windows环境下C语言控制你的PC喇叭（PC Speaker）编程"
slug = "pc_speader"

+++
级别： 初级 

作者：elprup 

2009 年 9 月 24 日 

## PC喇叭 

在很久之前，声卡还是很奢侈的时候，几乎所有的声音都是由这个喇叭发出的。所有的电脑都配有这个喇叭，就是发出开机时那个嘟的喇叭。实验室里突然有一个软件用这个喇叭来作为电话终端的铃声。某同学静音了也没有禁掉这个声音。我感觉很好玩，在网上随便搜了一下，还真有在Windows下可以控制喇叭的API。 

编程实例 
转载地址：http://www.xiugoo.com/bbs/thread-217627-1-1.html 

    1.    #include <iostream>  
    2.    using namespace std;  
    3.    #include <Windows.h>  
    4.      
    5.    int main(int argc, char ** argv)  
    6.    {  
    7.      
    8.            for (int ii = 1000; ii <1500; ++ii)  
    9.            {  
    10.                    Beep(ii, 5);  
    11.            }  
    12.      
    13.            cout << "all right!" << endl;  
    14.            cin.get();  
    15.            return 0;  
    16.    }  


其中关于Beep函数的用法为： 

Beep 

The Beep function generates simple tones on the speaker. The function is synchronous; it performs an alertable wait and does not return control to its caller until the sound finishes. 


BOOL Beep( 
  DWORD dwFreq, 
  DWORD dwDuration 
); 

Parameters 
dwFreq 
[in](#) Frequency of the sound, in hertz. This parameter must be in the range 37 through 32,767 (0x25 through 0x7FFF). 
Windows Me/98/95:  The Beep function ignores this parameter. 
dwDuration 
[in](#) Duration of the sound, in milliseconds. 
Windows Me/98/95:  The Beep function ignores this parameter. 

Return Values 
If the function succeeds, the return value is nonzero. 

结束语 

网上还看到直接在C语言中嵌入ASM汇编语言实现的例子，不过在这里好像跑不通。有了这个功能，恶搞一下别人的PC终于声情并茂了，呵呵。 

附：音频表 
低八度                                             中八度                                     高八度 
　 1     2    3    4     5    6     7     1    2     3    4     5     6     7     1     2     3     4     5    6      7 
C 131 147 165 175 196 220 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 
D 147 165 175 196 220 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 1108 
E 165 175 196 220 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 1108 1244 
F 175 196 220 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 1108 1244 1318 
G 196 220 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 1108 1244 1318 1479 
A 220 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 1108 1244 1318 1479 1660 
B 247 262 294 330 349 392 440 494 523 587 659 698 784 879 987 1108 1244 1318 1479 1660 1863 

