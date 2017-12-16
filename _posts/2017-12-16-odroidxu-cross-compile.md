---
layout:     post
title:      Odroid XU4 Cross Compile相关资料及总结
subtitle:   
date:       2017-12-16
address:    DTU, Lyngby
author:     hux0620303
header-img: img/milky-way.jpg
catalog: true
tags:
    - Odroid XU4
---


# [Odroid XU4](http://www.hardkernel.com/main/products/prdt_info.php) cross-compile 相关资料及总结

**本文以[Odroid XU4](http://www.hardkernel.com/main/products/prdt_info.php)为平台，但所述内容并不仅仅局限于[Odroid XU4](http://www.hardkernel.com/main/products/prdt_info.php).**

## 前言

什么是[cross-compile](https://en.wikipedia.org/wiki/Cross_compiler)以及为什么要进行[cross-compile](https://en.wikipedia.org/wiki/Cross_compiler)?

简言之：[cross-compile](https://en.wikipedia.org/wiki/Cross_compiler)是在一个系统上编译生成另外一个系统的应用程序。由于嵌入式系统的硬件条件限制以及诸如没有IDE GUI的支持，直接在嵌入式系统上写代码并且编译效率很低或者无法实现，[cross-compile](https://en.wikipedia.org/wiki/Cross_compiler)就是为了解决这个问题产生的。

## 关键工具 [toolchain](https://en.wikipedia.org/wiki/Toolchain)

要编译代码，需要编译器；而要进行[cross-compile](https://en.wikipedia.org/wiki/Cross_compiler) ，就需要能产生目标平台可执行程序的编译器以及配套工具，称作[toolchain](https://en.wikipedia.org/wiki/Toolchain)。 针对[Odroid XU4](http://www.hardkernel.com/main/products/prdt_info.php)，由于其CPU是ARM结构的，所以需要arm-chain。

而针对不同的arm内核，直接搜索的话，会得到非常多相关的toolchain，搞得人非常的莫名奇妙。

请移步之此文了解不同编译器之间的区别：

[arm交叉编译器gnueabi、none-eabi、arm-eabi、gnueabihf、gnueabi区别](http://www.veryarm.com/296.html)

这里直接说结论，如果Odroid XU4 上的gcc 或者 g++是不带hf的，那么就选择不带hf的gnueabi，反之选择带hf的。不过建议选择带hf的，因为Odroid XU4的大小核cortexA15和cortex A7都有fpu，也就是独立的硬件float计算单元，对于密集型float运算，可以产生多达[三倍的性能提升](http://www.cnblogs.com/xiaotlili/p/3306100.html)。

**Note:**

----

如果搭配错了cross-compile，编译的程序在目标板上运行，可能会出现如下[错误](https://www.embbnux.com/2014/04/09/cross_compile_no_such_file_or_directory/)：   

```bash
-base, No such file or directory.
```

此时可以用:   

```
arm-linux + TAB出你的gcc compiler -v
```

查看一下两处的gcc版本是不是一样即可。





**Note:**

----

对于使用[OpenCV](www.opencv.org)的朋友，请注意根据下文内容使用cross-compile编译OpenCV:

[Ubuntu 14.04 LTS下使用arm-linux-gcc交叉编译OpenCV 2.4.9](http://blog.csdn.net/ajianyingxiaoqinghan/article/details/70194392)

注意替换稳重的arm-linux-gcc为你自己的cross-compiler，不要`ctrl+c` `ctrl+v`就完事了。  

<S>`export PATH=$PATH:/usr/local/arm/4.3.2/bin ` </S>

<S> 8、OpenCV依赖库复制到ARM编译器路径下 </S>

这些没有必要加。



## GCC 及 ARM 相关编译选项

[gcc 优化选项 -O1 -O2 -O3 -Os 优先级，-fomit-frame-pointer](http://blog.csdn.net/lanmanck/article/details/5776173)

由于本人不是软件工程师，是算法工程师，所以比较关注的选项为：

* > -include file: 包含某个代码,简单来说,就是便以某个文件,需要另一个文件的时候,就可以用它设定,功能就相当于在代码中使用#include<filename> 例子用法: gcc hello.c -include /root/pianopan.h 

　　 **可用于获取版本号。**

* > -imacros file: 将file文件的宏,扩展到gcc/g++的输入文件,宏定义本身并不出现在输入文件中 


* > -Dmacro 相当于C语言中的#define macro 

* >-Dmacro=defn 相当于C语言中的#define macro=defn 

* > -llibrary 制定编译的时候使用的库 例子用法 gcc -lcurses hello.c 使用ncurses库编译程序 

* > -Ldir 制定编译的时候，搜索库的路径。比如你自己的库，可以用它制定目录，不然编译器将只在标准库的目录找。这个dir就是目录的名称。 

* > -O0 

  > -O1 

  > -O2 

  > -O3 

  > 　　编译器的优化选项的4个级别，-O0表示没有优化,-O1为缺省值，-O3优化级别最高　　 

针对ARM，

[3.18.4 ARM Options](https://gcc.gnu.org/onlinedocs/gcc/ARM-Options.html)

比较关注的有：

* `-mfloat-abi=name`

  > Specifies which floating-point ABI to use. Permissible values are: ‘soft’, ‘softfp’ and ‘hard’. 
  >
  > Specifying ‘soft’ causes GCC to generate output containing library calls for floating-point operations. ‘softfp’ allows the generation of code using hardware floating-point instructions, but still uses the soft-float calling conventions. ‘hard’ allows generation of floating-point instructions and uses FPU-specific calling conventions. 
  >
  > The default depends on the specific target configuration. Note that the hard-float and soft-float ABIs are not link-compatible; you must compile your entire program with the same ABI, and link with a compatible set of libraries.


* > `-march=name[+extension…]`
  >
  > This specifies the name of the target ARM architecture. GCC uses this name to determine what kind of instructions it can emit when generating assembly code. This option can be used in conjunction with or instead of the -mcpu= option.

  用于指定目标ARM的结构。

* > `-mtune=name`
  >
  > This option specifies the name of the target ARM processor for which GCC should tune the performance of the code. For some ARM implementations better performance can be obtained by using this option.

* > `-mfpu=name`
  >
  > This specifies what **floating-point hardware** (or hardware emulation) is available on the target. Permissible names are: ‘auto’, ‘vfpv2’, ‘vfpv3’, ‘vfpv3-fp16’, ‘vfpv3-d16’, ‘vfpv3-d16-fp16’, ‘vfpv3xd’, ‘vfpv3xd-fp16’, ‘neon-vfpv3’, ‘neon-fp16’, ‘vfpv4’, ‘vfpv4-d16’, ‘fpv4-sp-d16’, ‘neon-vfpv4’, ‘fpv5-d16’, ‘fpv5-sp-d16’, ‘fp-armv8’, ‘neon-fp-armv8’ and ‘crypto-neon-fp-armv8’. Note that ‘neon’ is an alias for ‘neon-vfpv3’ and ‘vfp’ is an alias for ‘vfpv2’.

  ​

  ​
