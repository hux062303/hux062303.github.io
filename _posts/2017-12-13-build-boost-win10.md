---
layout:     post
title:      boost install on WIN10 with vs2017
subtitle:   
date:       2017-11-24
address:    DTU, Lyngby
author:     hux0620303
header-img: img/boost.png
catalog: true
tags:
    - boost library
---

# 安装步骤  
1. 访问[boost](http://www.boost.org/)，获取release版本的boost，**一般最好不要选择最新的或者alpha，beta等非正式版本**；
2. 下载安装文件，这里我使用的是[boost_1_65_1.zip](http://www.boost.org/users/download/)；
3. 解压，注意阅读[Getting Stated](http://www.boost.org/doc/libs/1_65_1/more/getting_started/windows.html), 大部分的包是header only的，也就是说boost的一部分库是可以和Eigen一样直接包含使用的~但是，有一部分包需要build，而由于boost里面的包相互之间可能会有一些包含引用，未避免后续使用出现问题，还是建议把所有的包都build了；
4. 打开正确的command，由于使用的是VS2017，所以我打开的是command prompt for VS2017（**这里注意，使用不正确的command，可能出现类似build engine 的错误，不要慌**）, cd到boost目录，执行`.\bootstrap.bat`  

`b2 install --prefix=DIRECTORY_YOU_WANT_TO_INSTALL_BOOST`  

然后，等待~，之后你会在DIRECTORY_YOU_WANT_TO_INSTALL_BOOST看到include和lib。  

最后，打开VS，试一试一下CODE，如果能正确编译，boost的安装就好了。   



```
#include <boost/regex.hpp>
#include <iostream>
#include <string>
int main()
{
    std::string line;
    boost::regex pat( "^Subject: (Re: |Aw: )*(.*)" );

    while (std::cin)
    {
        std::getline(std::cin, line);
        boost::smatch matches;
        if (boost::regex_match(line, matches, pat))
            std::cout << matches[2] << std::endl;
    }
}
```
