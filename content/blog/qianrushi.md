+++
date = "2007-05-18T07:01:46Z"
title = "嵌入式开发一次实践"
slug = "embeded_practice"
+++

# 嵌入式开发一次实践
01:51 上午 2007-5-18

为了编写秒表程序，需要在读到键盘输入以后马上就跳出循环。老师说用ioctl就可以解决问题。于是，编写了下面的代码：
    while(1){
    printf("%d\r",second++); //显示当前的时间值
    taskDelay(60); //延时60个时钟跳变
    ret = ioctl(stdIN, FIONREAD, (int)&bufCount);//将stdIN缓冲区值存入bufCount中
    if ( bufCount > 0 ) break;
    }
测试的结果是，在标准输入中输入时，总是要等待一个回车，才可以跳出循环。就是说在输入一个字符的时候，ioctl没有立刻变化bufCount的值。我想，系统或者就上ioctl的机制有问题。于是就找到了老师给的代码，据说这个代码在键盘有输入时候就可以马上反应。
      /* 取标准输入/输出设备文件描述符 */
      stdIN  = ioGlobalStdGet(0);
      stdOUT = ioGlobalStdGet(1); //标准输入输出的界面都是vxsimX
    
      /* 将标准输出设备输出模式设置为RAW */
      ioctl(stdOUT, FIOSETOPTIONS, OPT_RAW);  /* defined in iolib.h */
    
      /* 主循环 */
      while(1)
      {
    /* 在标准输入设备等待键盘输入字符 */
    read(stdIN,&c,1);
    
    /* 判断若输入字符为"ESC键"则退出主循环 */
    if(c == '\e')
      break;
    
    /* 向标准输出设备输出刚接收的字符 */
    write(stdOUT,&c,1);
    
    /* 若为"回车"键，则后续输出"换行"键 */
    if(c == '\r')
      write(stdOUT,"\n",1);
      }
看了以后才知道根本就没有用传说中的ioctl和输入流有关的部分。是不断的从键盘读入字符，判断这个字符。原来read本来就有这样的性质，不用回车也可以从缓冲区读取。于是我很高兴的把ioctl(stdOUT, FIOSETOPTIONS, OPT_RAW); 给删了。反正这是一个ms是和标准输出有关系的语句。然后在退出来以后又在while外面加了printf测试。结果确实还是能立刻反应，但是printf把值都写到了shell里面取了。我很奇怪。printf不应该是默认的写到stdout中吗？怎么会到shell里面来呢？
于是我把没有参数的printf改成了fprintf(stdOUT,"****");结果报错了。在stdio.h中，fprintf的第一个参事是FILE*，那就变成系统的默认stdout吧。结果还是不行。看来，要看看printf的默认输出为什么到了shell，就要看看shell的fd是什么了。用iosFdShow看了一下：
 fd name                 drv
  3 /tyCo/0                1
  4 /vio/1                 4
value = 0 = 0x0
估计应该是fd＝3吧（vxsimX应该是4）
于是就改一下把printf改成write(3,"****",4);结果在vxsim里面打出来了。看来vio才是shell。于是该成write(4,"****",4);果然是这样。并且shell并没有作为标准的输入输出设备出现。系统在前面默认的是vxsim才是标准的输入输出。但是我就是想在shell里面做，怎么办呢？
答案是重定向。那就开始重定向吧。重定向有系统级的和任务级的，我们只要任务级的就行。看一个例子：
    /* 打开PC控制台，并得到控制台的文件描述符*/
    consolefd = open("/pcConsole/", O_RDWR, 0);
    /* 将全局标准输入重定向到PC控制台*/
    ioGlobalStdSet(STD_IN, consolefd);
    那我们先要得到shell的文件描述符。用open函数得到：
    int open
    (
    const char * name,        /* name of the file to open */
    int          flags,       /* O_RDONLY, O_WRONLY, O_RDWR, or O_CREAT */
    int          mode         /* mode of file to create (UNIX chmod style) */
    )
我有点担心，会不会打开过了我们这个设备，再打开一个shell呢？try一下。编写了语句：
consoleFd = open("/vio/1",O_RDWR, 0);
taskID = taskNameToId("stdio_test");//先把name转成task ID
ioTaskStdSet(taskID, STD_IN, consolefd);
ioTaskStdSet(taskID, STD_OUT, consolefd);
结果这个tasktoid函数真的很难用。所以我还是回到了回来的程序，还有原因是张浩然说他能接收任意键了，就是用的ioctrl结果就用了他的程序。但是发现大部分都和我一样。sigh。我觉得不一样的就是
    ioctl(stdOUT, FIOSETOPTIONS, OPT_RAW);//veryimportant
    ioctl(stdIN,FIOFLUSH,0);//veryimportant
特别是后来一句，如果不加就会进入马上进入暂停程序。但是奇怪的是这和接不接收输入相关呢？如果不加，在vxsim里输入就无法到达程序被接收了。今天先到这了。dddd.cpp是实现了的秒表。
