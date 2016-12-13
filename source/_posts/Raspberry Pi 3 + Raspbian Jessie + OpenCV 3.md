---
title: Raspberry Pi 3 + Raspbian Jessie + OpenCV 3
tags:
- RaspberryPi
- OpenCV
categories:
- Robotics
---

[Install guide: Raspberry Pi 3 + Raspbian Jessie + OpenCV 3 by Adrian Rosebrock](http://www.pyimagesearch.com/2016/04/18/install-guide-raspberry-pi-3-raspbian-jessie-opencv-3/)

<!--more-->

## 扩展文件系统

    sudo raspi-config

选择第一个选项*1. Expand File System*，回车键*Enter*，结束按钮*Finish*，然后重启树莓派

    sudo reboot

之后可以使用`df -h`命令查看SD卡的使用分配情况

如果空间不足可以把系统中自带的Wolfram语言的环境卸载删除掉

    sudo apt-get purge wolfram-engine

## 安装依赖环境

更新升级安装库

    sudo apt-get update
    sudo apt-get upgrade

安装开发工具，包括CMake等

    sudo apt-get install build-essential cmake pkg-config

接着，我们需要安装一些图像I/O包，使得我们可以加载多种格式的图片文件

    sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev

国内的朋友如果有些包下不下来的话就换一下软件源，参考[树莓派学习笔记——修改树莓派软件源](http://blog.csdn.net/xukai871105/article/details/38614541)

安装视频I/O包

    sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
    sudo apt-get install libxvidcore-dev libx264-dev

OpenCv库的`highgui`模块需要依赖GTK开发库

    sudo apt-get install libgtk2.0-dev

一些OpenCv的运算可以通过一些扩展包增强

    sudo apt-get install libatlas-base-dev gfortran

安装Python 2.7和Python 3头文件

    sudo apt-get install python2.7-dev python3-dev

## 下载OpenCv源码

    cd ~
    wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.1.0.zip
    unzip opencv.zip

为了能使用SIFT、SURF等这样的高级特性需要下载扩展包

    wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.1.0.zip
    unzip opencv_contrib.zip

## Python 2.7或Python 2？

Python包管理工具`pip`

    wget https://bootstrap.pypa.io/get-pip.py
    sudo python get-pip.py

安装虚拟环境`virtualenv`和`virtualenvwrapper`，可以为不同的工作分离开不同的包版本

    sudo pip install virtualenv virtualenvwrapper
    sudo rm -rf ~/.cache/pip

将以下内容加入用户环境变量文件`~/.profile`

    sudo nano ~/.profile

```
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

通过`source`命令使`~/.profile`生效

    source ~/.profile

建立Python虚拟环境

    mkvirtualenv cv -p python2

通过以上这个命令建立了一个名字为`cv`的使用*Python 2.7*的虚拟环境

如果你想使用*python 3*，可以使用下面这个命令：

    mkvirtualenv cv -p python3

> Again, I can’t stress this point enough: the cv  Python virtual environment is entirely independent and sequestered from the default Python version included in the download of Raspbian Jessie. Any Python packages in the global site-packages  directory will not be available to the cv  virtual environment. Similarly, any Python packages installed in site-packages  of cv  will not be available to the global install of Python. Keep this in mind when you’re working in your Python virtual environment and it will help avoid a lot of confusion and headaches.

如何确认是否在"cv"虚拟环境

在命令行前面是否有`(cv)`，如果没有需要通过以下命令进入`cv`虚拟环境：

    source ~/.profile
    workon cv

**以下操作都是在`cv`虚拟环境下进行**

安装NumPy包

    pip install numpy

## 编译安装OpenCv

请先确认是否在`(cv)`虚拟环境下

    cd ~/opencv-3.1.0/
    mkdir build
    cd build
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D INSTALL_PYTHON_EXAMPLES=ON \
        -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.1.0/modules \
        -D BUILD_EXAMPLES=ON ..

编译OpenCv

    make -j4

`-j4`参数是树莓派3有4核，这样可以加快编译速度。

但是，有时会出现编译出错的情况，那么建议通过以下命令重新开始编译，并且只使用1核。

**强调是`make -j4`出错的情况下才从新使用以下命令从新开始**

    make clean
    make

编译过程会持续一个多小时。

编译完成后安装OpenCv

    sudo make install
    sudo ldconfig

## 安装最后

使用的是Python 2.7的：

通过以下命令查看OpenCv是否被安装到这个目录：

    ls -l /usr/local/lib/python2.7/site-packages/
    total 1852
    -rw-r--r-- 1 root staff 1895772 Mar 20 20:00 cv2.so

最后一步是为`cv`虚拟环境建立一个OpenCv的链接

    cd ~/.virtualenvs/cv/lib/python2.7/site-packages/
    ln -s /usr/local/lib/python2.7/site-packages/cv2.so cv2.so

使用的是Python 3的：

通过以下命令查看OpenCv是否被安装到这个目录：

    ls -l /usr/local/lib/python3.4/site-packages/
    total 1852
    -rw-r--r-- 1 root staff 1895932 Mar 20 21:51 cv2.cpython-34m.so

不知道什么原因，在Python 3环境下编译出来的动态链接库的名字是`cv2.cpython-34m.so`，而不是`cv2.so`，不过我们可以通过重命名轻易的修改过来：

    cd /usr/local/lib/python3.4/site-packages/
    sudo mv cv2.cpython-34m.so cv2.so

最后一步是为`cv`虚拟环境建立一个OpenCv的链接

    cd ~/.virtualenvs/cv/lib/python3.4/site-packages/
    ln -s /usr/local/lib/python3.4/site-packages/cv2.so cv2.so

## 验证是否安装正确

```bash
source ~/.profile
workon cv
python
>>> import cv2
>>> cv2.__version__
'3.1.0'
>>>
```

当OpenCV安装结束后，可以将`opencv-3.1.0`和`opencv_contrib-3.1.0`目录删除来释放一些空间。

    rm -rf opencv-3.1.0 opencv_contrib-3.1.0
