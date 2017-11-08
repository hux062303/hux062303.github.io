---
layout:     post
title:      Install OpenCV with VIZ on win10 and VS2017.
subtitle:   
date:       2017-11-08
address:    DTU, Lyngby
author:     hux0620303
header-img: img/post-Thomas-DTU-Ballerup-Campus-exterioer_Tryk-600dpi_28.jpg
catalog: true
tags:
    - OpenCV
---


# How to build OpenCV with VIZ module on windows 10 and with VS 2017

> lots of fucking and strange errors occurred during the building process........

1. Down load [CMake](https://cmake.org/download/);

2. Download VTK ([version 6.2.0](https://github.com/Kitware/VTK/tree/v6.2.0));

3. **Suppose VS2017 has been installed**, 

   1. use CMake to configure VTK, unselect the building options of TEST and build shared libs;

   2. **Sorry, but you have to modify some internal code in order to get VTK build successfully.** </span>

      * Project: **vtklibxml2**, find **config.h**

      ``` c++
      // comment the following line of codes
      /* Win32 Std C name mangling work-around */
      /* MS VS2015: deprecated. snprintf is now the standard in VS2015. */
      // #if defined(_MSC_VER)
      // # define snprintf _snprintf
      // #endif

      ```

      * Project: **vtktiff**,  find File: **tiffiop.h**

        ```c++
        // MS VS2015: added header
        # include <corecrt_search.h>

        // MS VS2015: deprecated
        /* #ifdef HAVE_SEARCH_H
        # include <search.h>
        #else
        extern void *lfind(const void *, const void *, size_t *, size_t,
                           int (*)(const void *, const void *));
        #endif */
        ```

      *  Project: **vtkhdf5**, find File:  **H5Omtime.c**

        ```c++
        #elif defined(H5_HAVE_LNX_TIMEZONE) // Dummy argument for now
        	/* Linux libc-5 */
            the_time -= timezone - (tm.tm_isdst?3600:0);
        #elif defined(H5_HAVE_TIMEZONE) && defined(_MSC_VER) && _MSC_VER >= 1900
        // In Visual Studio prior to VS2015 'timezone' is a global variable declared
        // in time.h. That variable was deprecated and in VS2015 is removed, with
        // _get_timezone replacing it.
        long time_zone = 0;
        _get_timezone(&time_zone);
        the_time -= time_zone - (tm.tm_isdst ? 3600 : 0);
        ```

      * Project: **vtkCommonCore**, find **vtkWin32ProcessOutputWindow.cxx**

        ```c++
        //Fix: add a space before and after PRIdword.
        // Construct the executable name from the process id, pointer to
          // this output window instance, and a count.  This should be unique.
          sprintf(exeName, "vtkWin32OWP_%" PRIdword "_%p_%u.exe",
                  GetCurrentProcessId(), this, this->Count++);
        ```

4. Download **OpenCV**([version 3.1.0](https://github.com/opencv/opencv/tree/3.1.0)) and [OpenCV Contrib](https://github.com/opencv/opencv_contrib/tree/3.1.0); Be careful to set the following options:

   1. **WITH VTK**
   2. set **VTK DIR** to *your_path_of_VTK/build*
   3. set **opencv_extra_modules_path** to *your_path_of_opencv_contrib/modules*
   4. configure and generate

5. VS2017 select **ALL BUILD**, build.

:smile:I hope that you have successfully built OpenCV with VIZ module activated.
