---
layout:     post
title:      image compression & normally used format
subtitle:   
date:       2017-11-24
address:    DTU, Lyngby
author:     hux0620303
header-img: img/sunset-1331088_1920.jpg
catalog: true
tags:
    - image compression
---


# [Image Compression](https://en.wikipedia.org/wiki/Image_compression)

1. Lossy: may introduce [compression artifacts](https://en.wikipedia.org/wiki/Compression_artifact)(like [ringing artifacts](https://en.wikipedia.org/wiki/Ringing_artifacts#JPEG)), JPEG 
2. lossless: BMP, PNG





## PNG V.S JPEG V.S BMP

> [JPEG](https://en.wikipedia.org/wiki/JPEG) (Joint Photographic Experts Group) format can produce a **smaller file than PNG for [photographic](https://en.wikipedia.org/wiki/Photography) (and photo-like) images**, since JPEG uses a [lossy encoding method](https://en.wikipedia.org/wiki/Lossy_compression)specifically designed for photographic image data, which is typically dominated by soft, low-contrast transitions, and an amount of noise or similar irregular structures. **Using PNG instead of a high-quality JPEG for such images would result in a large increase in filesize with [negligible](https://en.wikipedia.org/wiki/Transparency_(data_compression)) gain in quality**. In comparison, when storing images that contain text, line art, or graphics – images with sharp transitions and large areas of solid color – the PNG format can compress image data more than JPEG can. Additionally, **PNG is lossless, while JPEG produces noticeable visual artifacts around high-contrast areas**. Where an image contains both sharp transitions and photographic parts, a choice must be made between the two effects. JPEG does not support transparency.
>
> **JPEG's lossy compression also suffers from [generation loss](https://en.wikipedia.org/wiki/Generation_loss), where repeatedly decoding and re-encoding an image to save it again causes a loss of information each time, degrading the image.** This does not happen with repeated viewing or copying, but only if the file is edited and saved over again. Because **PNG is lossless, it is suitable for storing images to be edited.** While PNG is reasonably efficient when compressing photographic images, there are lossless compression formats designed specifically for photographic images, lossless [WebP](https://en.wikipedia.org/wiki/WebP) and [Adobe DNG](https://en.wikipedia.org/wiki/Adobe_DNG)(digital negative) for example. However these formats are either not widely supported, or are proprietary. An image can be stored losslessly and converted to JPEG format only for distribution, so that there is no generation loss.

> .bmp	image/bmp	Windows位图	最常被Microsoft Windows 程序以及其本身使用的格式。可以使用无损的資料压缩，但是一些程序只能使用未经压缩的文件。bmp almost no compression file, larger than PNG
