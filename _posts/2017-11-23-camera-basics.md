---
layout:     post
title:      camera basics
subtitle:   
date:       2017-11-23
address:    DTU, Lyngby
author:     hux0620303
header-img: img/sea-2755908_960_720.jpg
catalog: true
tags:
    - camera
    - sensor
    - pixel size
    - focal length
    - rolling shutter
    - binning
---

# sensor size & focal length	

1. focal length will not change for a lens. distance in millimeters.

2. sensor size, 就是底的大小，底越大，同样的pixel数目，每个pixel的physical size就越大，能接受的光就更多，ISO更好。

   >A larger sensor is better, as this allows larger pixels on the sensor which in turn helps record more light, and will also allow the manufacturer to offer a wider ISO range, as the camera will be able to shoot at higher ISO speeds, whilst keeping noise low. For example a Full-Frame sensor is bigger than an APS-C sensor, and an APS-C sensor is larger than a Micro Four Thirds sensor, and therefore the larger the sensor, the larger the light collecting area, and the larger the pixels will be, assuming all sensors are the same megapixel count, and use the same [sensor technology](http://www.ephotozine.com/article/digital-camera-image-sensor-technology-guide-16808).

3. *sensor type*: 

   > **CCD** - Charge-Coupled Device
   >
   > Each pixel is responsible for collecting light, but as the pixel is simply detecting light, a colour [filter](https://www.ephotozine.com/filterzone) is needed in order for the sensor to pick up the different colours in the scene. The CCD sensor is often used in lower price cameras, as is often limited to slower continuous shooting and lower resolution video recording (720p). The **Bayer Sensor pattern** is the most popular and common arrangement of the filter, with a Blue, Green, Green, and Red arrangement:

   ![Bayer Sensor Pattern](https://www.ephotozine.com/articles/buyers-guide-to-digital-camera-sensor-technology-16808/images/01_bayer.gif)

   > **CMOS** - Complementary Metal–Oxide–Semiconductor
   >
   > sensors use less power than CCD sensors and often allow quicker read speeds than CCD sensors, allowing high speed continuous shooting and high speed FullHD video, as well as 4K video recording in some cameras.
   >
   > *Back-lit CMOS*:
   >
   > BSI CMOS sensors are of particularly benefit to small sensors such as those found in camera phones, and compact cameras where the small size of the sensor means the sensor is mostly dominated by the wiring which blocks light from reaching the sensor. More recent developments has seen the use of Back-lit CMOS sensors used in [APS-C CMOS sensors](https://www.ephotozine.com/article/samsung-nx1-full-review-26178), as well as [full-frame CMOS sensors](https://www.ephotozine.com/article/sony-alpha-a7r-mark-ii--ilce-7rm2--review-28008) for improved noise performance. The majority of BSI CMOS sensors use the Bayer filter pattern.[2](https://www.ephotozine.com/article/digital-camera-image-sensor-technology-guide-16808)

   Details explanation about phone, 1 inch, micro four thirds, full frame sensor[3](https://www.ephotozine.com/article/complete-guide-to-image-sensor-pixel-size-29652).

   ​

## ANGLE OF VIEW

> This is a measure of the vantage point from the lens. A short focal length will give you a wide angle of view, taking in more of the image than the narrow angle created by a long focal length lens.

##FIELD OF VIEW (or FOV)

> We are using Field of View to mean the length that the lens will cover at a certain distance. __The FOV can be measured horizontally, vertically or diagonally__. Our examples are of the Horizontal Field Of View. Shorter focal lengths will have a wider FOV than longer focal lengths. The horizontal and vertical fields of view define the frame lines at a given distance. **(Note that some people use Field of View to mean the same thing as Angle of View)**.

Very mimic details can be found at [1](http://www.panavision.com/sites/default/files/docs/documentLibrary/2%20Sensor%20Size%20FOV%20%283%29.pdf).



 rule of thumb:

1. > nearly no zoom with a ratio over 3 gives a nice image quality (that's why there are none of those in [my equipment recommendations](http://www.similaar.com/foto/equipment/us_lensc.html)). 



more things about 

[Basic photography tutorial ](http://www.similaar.com/foto/tuten/510.html)



[**how to define your vision sensors and focal length**](http://digital.ni.com/public.nsf/allkb/1BD65CB07933DE0186258087006FEBEA)





# [Rolling shutter](https://en.wikipedia.org/wiki/Rolling_shutter)

Smear effect:涂抹效应

![An iPhone's CMOS camera has no shutter, and reads the pixel values off in rows rather than all at once. In this photo the propeller spins about 5 times while the photo is taken, producing a strange effect](https://i.stack.imgur.com/PdKScm.jpg)

![A photo of a Eurocopter EC-120](https://i.stack.imgur.com/I8tnZ.jpg)

partial exposure效应：读出的时候曝光变了，导致上下部分的曝光不一致

![lighting](https://i.stack.imgur.com/aXpaD.jpg)

skew and jello effect:skew是指垂直的东西向对角线倾斜，jello effect指的是图像部分发生了颤动，例如下图第2到第3帧，图像瞬间由倾斜状态恢复垂直，就会发生jello effect.

![jello](http://www.dslrvideocollege.com/wp-content/uploads/2013/05/jello.jpg)

#Binning

- ![img](http://www.microview.com.cn/uploadfile/2016/0525/20160525101046923.png)

- Binning 的作用在于把相临几个像素所积累的电荷累加起来作为一个电荷进行转换。这样做的效果相当于把几个像素拼成了一个大像素使用。速度快但分辨率降低。

- Binning后可以带来：

- **①更高的动态范围**

- **②更高的信噪比**

- **③更快的数据读出速度**

- **④图像的分辨率随像素尺寸的大小而变化**

- ![img](http://www.microview.com.cn/uploadfile/2016/0525/20160525101121895.png)

- ![img](http://www.microview.com.cn/uploadfile/2016/0525/20160525101140622.png)
  ![img](http://www.microview.com.cn/uploadfile/2016/0525/20160525101157820.png)

