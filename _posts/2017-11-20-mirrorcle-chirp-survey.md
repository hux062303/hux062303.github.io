---
layout:     post
title:      Mirrorcle Survey
subtitle:   
date:       2017-11-20
address:    DTU, Lyngby
author:     hux0620303
header-img: img/post-Thomas-DTU-Ballerup-Campus-exterioer_Tryk-600dpi_28.jpg
catalog: true
tags:
    - sensors
    - LIDAR
---

# Mirrorcle激光雷达

[Mirrorcle](http://mirrorcletech.com/index.html)是一家Berkeley spin-off的公司，主营业务是点对点的MEMS激光雷达。

> We provide a variety of optical microelectromechanical system (MEMS) products such as **world's fastest and lowest power-consuming** two-axis point-to-point beam-steering mirrors in many mirror sizes.

应用包括：

> MEMS Mirror-based **Display** and **Picoprojector** technology
> **3D Laser Tracking **and **LiDAR** technology
> Windshield Head-Up Display **(HUD) **technology



由于项目的需要，最近对Mirrorcle公司做了一轮详细的survey，公司的实力是很强了，毕竟是Berkeley孵化的公司。Mirrorcle背后的核心技术就是世界最快的2轴MEMS旋转mirror，所有的应用不论是tracking还是mapping，其实都是这儿技术的运用。我个人觉得他能做到世界最快，有以下几点原因：

1. 高精度的制造，超净，光刻，PC芯片级别的制造厂；
2. 极高平率的开环控制，对，就是开环控制，精确测量了每一个电压下面，mirror的朝向角度，所以最后的控制都是开环的给一个LUT的电压值；
3. 多种扫描模式，其实是高精度制造和高精度控制的必然结果，可以支持点点扫描，单轴谐振扫描，双轴谐振扫描；



不过价格就是很贵了。。。。。

Berkeley还有一家MEMS的3D Ultrasonic spin-off [Chirp](http://www.chirpmicro.com/)，用于gesture recognition，也很强...... 

*充分说明了一个道理：美帝，掌握核心科技........*
