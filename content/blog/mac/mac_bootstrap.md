+++
date = "2014-07-20T07:01:46Z"
title = "mac os x tips for developers in china"
slug = "mac_bootstrap"
+++
# mac os x tips for developers in china
 2014年7月20日 星期日 上午9:58
0. first boot
DO NOT enable wifi and connect to your apple ID for first time, it will halt on “creating account” page. 

1. mouse rolling:
default is reversed to windows. enter config/mouse to change it.

2. download Xcode
RECOMMEND: download IMG by manual. 
download link:
[http://adcdownload.apple.com/Developer\_Tools/xcode\_5.1.1/xcode\_5.1.1.dmg](http://adcdownload.apple.com/Developer_Tools/xcode_5.1.1/xcode_5.1.1.dmg)

NOT RECOMMEND: view in apple Xcode page to download, it will open mac app store page.
If it fails, change the dns server to 8.8.8.8, 8.8.4.4, 114.114.114.114 to try.

3. pages
save: windows + s
TIPS: all CTRL will change to WINDOWS key in Mac system.

4. Chinese input method
1) change hot key for input method change:
preference -\> keyboard \>   hot key
2) change keyboard type to simple Chinese will add input method.

5. hot keys for common use
Close: WIN+q
End: WIN+(-\>)
Switch window: WIN+Tab
6. SSH console
search Terminal 

7. recognize extra keys for your keyboard
TODO

8. recommend SSH: iterm

9. code editor: atom.io

10. key editor:
change template: by default, the alignment is middle, select one slide and click 编辑母版幻灯片 to change it to top align.
change theme: template will only affect current file, if you want all the settings take effect for ever, you just modify the template and save it as “save theme”

11. multiple desktop and dashboard
add new desktop: ctrl + up key and click right top corner
one question is if you run chrome in desktop one and run it again in another desktop, it will jump to desktop 1. The solution is right click icon, in option, assign it to all desktop.
dashboard: F12
show desktop: F11

12 office word
输入法显示乱码
我通过以下两法解决小方块和拼音字母乱上的问题：
一、把office2011升级到最新版本14.3.9
二、我还用endnote，需要禁用endnote插件，或者关闭以下两项：
1. Tools---\>Cite While You Write---\>CWYW Preferences---\>General,取消“Enable Instant Formatting on new Word documents”
2. Tools---\>Cite While You Write---\>Format Bibliography, 关闭掉“Instant Formatting”

13 剪切
方法很简单 
传统意义的 command－c 复制 command－v 粘贴 （保留原文件）

新增加的
command－c 是一样复制 command－option－v 移动（剪切 不保留原文件）
