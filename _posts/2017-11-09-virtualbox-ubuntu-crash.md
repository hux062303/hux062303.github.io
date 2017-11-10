---
layout:     post
title:      解决virtualbox+ubuntu 16.04蓝屏问题
subtitle:   
date:       2017-11-09
address:    DTU, Lyngby
author:     hux0620303
header-img: img/post-Thomas-DTU-Ballerup-Campus-exterioer_Tryk-600dpi_28.jpg
catalog: true
tags:
    - ubuntu 16.04
    - virtual box
---



# Virtual Box + Ubuntu 16.04

virtual box + ubuntu 16.04很不稳定，我周二收到新电脑被IT人员装上了virtual box。

> really 死板的丹麦人，我心疼你在新电脑上装ubuntu驱动搞不定，才让你帮我装windows+虚拟机的，结果你丫非要用正版的......选了virtualbox这么个玩意，哥以前又没用过。坑货啊....还不如我自己把电脑一拿，下点破解版不就结了么......

结果这两天build了一些windows上需要用的软件之后，今天在登录ubuntu，改了个密码，在登录居然就蓝屏了......

> ubuntu真心是个妖孽啊，被坑无数回了。之前在法国实习，电脑死了至少2两会，第一回，手贱点了ubuntu upgrade，结果进不去了.....弄了半天；第二回，有事upgrade，结果鼠标键盘全部悲剧，用弄了半天；在DJI，为了新产品升级了ubuntu 14.04有出现没有terminal，文件也都看不见了，有弄了半天.....*解决方案一般都是进入命令行，然后狂升一波linux 内核就tm好了*

今天这个情况，google了一下，果然不是个例啊....

好了，废话不多说，**正解如下：**

```bash
I had the same problem. It seems to be some updates that Ubuntu has done on the last session but some updates are missing.

So...what I did was:

1-. Opening terminal on login screen: Ctrl+Alt+F1
2.- sudo dpkg --configure -a
When you run it, you will see there is a dependency on linux-generic-hwe that is not updated. This dependency seems to be linux-headers-generic. So...
3.- sudo apt-get install linux-headers-generic
4.- When everything is installed then reboot.
5.- You will see your desktop working well again

I hope it will work for all of you also.
```

# shared folder在virtualbox里面mount不上的问题的解决方案

> You have to mount your folder on your VM.  
> First you need to install Guest Additions (although I already did this during the installation).  
> Start your VM  
> Devices > Insert Guest Additions CD image... *如果你没有cd，没有关系，去下载一个Guest Additions.ios就可以了*  
> I had to manually mount the CD: sudo mount /dev/cdrom /media/cdrom *如果你不是走CD这条路，需要先到/media目录下看一眼，VBoxLinuxAdditions。run在哪个文件夹下面*  
> Install the necessary packages: **sudo apt-get install make gcc linux-headers-$(uname -r)**，很关键，我就是试了这一步之后才成功的  
> Install the Guest Additions: **sudo /media/cdrom/VBoxLinuxAdditions.run**.这一步也很关键，注意去你自己的目录下run，不要直接copy，不会成功的.  
> Now you can mount your share using:  
> mkdir ~/new  
> sudo mount -t vboxsf New ~/new  
> Where New is the name of your shared folder.  
> Now you can access the shared folder at ~/new.  

## reference
[Blue screen on Ubuntu start](https://forums.virtualbox.org/viewtopic.php?f=6&t=80178)
[Why can't I access a shared folder from within my Virtualbox machine?](https://askubuntu.com/questions/456400/why-cant-i-access-a-shared-folder-from-within-my-virtualbox-machine)
