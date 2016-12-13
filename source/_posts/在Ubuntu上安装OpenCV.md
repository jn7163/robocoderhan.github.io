---
title: 在Ubuntu上安装OpenCV
tags:
- OpenCV
categories:
- Robotics
---

[Install OpenCV 3.0 and Python 2.7+ on Ubuntu by Adrian Rosebrock](http://www.pyimagesearch.com/2015/06/22/install-opencv-3-0-and-python-2-7-on-ubuntu/)

[ubuntu14.04+opencv 3.0+python2.7安装及测试 lireagan](http://www.cnblogs.com/llxrl/p/4471831.html)

[Installation in Linux](http://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html?)

<!--more-->

* 在cmake的步骤中可能会出现`CMake Error at 3rdparty/ippicv/downloader.cmake:77 (message): ICV: Failed to download ICV package: ippicv_linux_20151201.tgz. ...`的错，可以以下方式解决：

1. 下载<http://www.linuxfromscratch.org/blfs/view/svn/general/opencv.html>里的[Additional Downloads Optional file](https://raw.githubusercontent.com/Itseez/opencv_3rdparty/81a676001ca8075ada498583e4166079e5744668/ippicv/ippicv_linux_20151201.tgz)

2. 将下载的压缩包放到 `opencv-3.1.0/3rdparty/ippicv/downloads/linux-808b791a6eac9ed78d32a7666804320e`目录中，如果有的话就覆盖掉原来的

* [如何卸载OpenCv](http://blog.csdn.net/xulingqiang/article/details/52496701)

1. 进入opencv的源代码文件夹下的release（这是你在安装opencv时候自己命名的，cmake时候所在的目录，也可能是build目录）

2. 依次执行下面的代码

```bash
make uninstall
cd ..
sudo rm -r release
sudo rm -r /usr/local/include/opencv2 /usr/local/include/opencv /usr/include/opencv /usr/include/opencv2 /usr/local/share/opencv /usr/local/share/OpenCV /usr/share/opencv /usr/share/OpenCV /usr/local/bin/opencv* /usr/local/lib/libopencv_*
```
