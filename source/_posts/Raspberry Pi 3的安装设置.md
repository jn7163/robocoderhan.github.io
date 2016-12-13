---
title: RaspberryPi 3的安装配置
tags:
- RaspberryPi
categories:
- Robotics
---

我的环境是Raspberry Pi 3 + raspbian-jessie(RASPBIAN JESSIE WITH PIXEL)，硬件环境和系统环境不同所以一些配置实用也会不同，在wifi和蓝牙配置使用上有不同，我就是按着老的教程修改了蓝牙的配置，导致板载的蓝牙检测不到了，就需要重新装系统。

<!--more-->

## 安装Raspbian系统

在开始安装Raspbian之前，需要先下载[Raspbian](https://www.raspberrypi.org/downloads/raspbian/)镜像文件，**新手或者对Linux系统不熟悉的最好不要用`lite`版**。同时你还需要下载[Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)。两个软件都下载完后，解压Win32 Disk Imager，将SD卡插入到读卡器中。运行此工具，选择正确的驱动盘符，然后点击文件图标进行浏览，选择你刚才下载的最新的Raspbian镜像文件。开始进行安装，点击Write，然后等待。当此过程结束时你会看到一个结束的提示框。到此，你的树莓派就已经安装好了！

## 第一次进入

使用树莓派的方式有很多种：

* 树莓派+外接显示器
* 树莓派+WiFi+SSH+VNC
* 树莓派+网线+SSH
* ...

我使用的方式是第二种，但是第一次你需要先登录一次WiFi才可以，因为Raspberry Pi 3板载了WiFi模块，所以设置很简便，HDMI接口外接一个显示器，再加外接键盘、外接鼠标，通电以后就会有界面显示，点击WiFi图标，连接你笔记本发的WiFi。Windows可以用[猎豹免费WiFi](http://wifi.liebao.cn/)软件，这种情况下可以通过将鼠标放在猎豹WiFi的你的用户名上看到分给Raspberry Pi的IP地址；Ubuntu系统可以参考这个[ubuntu14.10建立热点wifi](http://jingyan.baidu.com/article/363872ecd8f35d6e4ba16f97.html)，Ubuntu系统查看分给Raspberry Pi的IP的方法是，终端中输入`sudo apt-get install nmap`，然后用`nmap -v -sP 10.42.0.0/24`命令查看分配的IP地址。

当获得了，笔记本分配给Raspberry Pi的IP以后，就可以彻底解放它了，再也不需要外接任何东西了。不过这个过程中可能会遇到以下问题：

* 外接显示器不显示东西

*新的raspbian系统好像不会有这个问题了。*

可以[如何解决树莓派B+使用HDMI转VGA线点不亮屏幕]()中的方式尝试解决，在将SD卡插入Raspberry Pi之前，修改SD卡目录中的`config.txt`文件，内容如下：

```
#强制使用HDMI输出
hdmi_force_hotplug=1
#HDMI信号增强
config_hdmi_boost=4
#HDMI输出适配于计算机显示器
hdmi_group=2
#HDMI输出的分辨率及刷新频率
hdmi_mode=9
#禁止树莓派检测显示器分辨率，直接使用配置文件中制定的分辨率输出
hdmi_ignore_edid=0xa5000080
#禁止黑边
disable_overscan=1
start_x=1
gpu_mem=128
```

* 没有外接显示器，使用的不是Raspberry Pi 3

参考以下博文：

[树莓派+一根网线直连笔记本电脑](http://shumeipai.nxez.com/2013/10/15/raspberry-pi-and-a-network-cable-directly-connected-laptop.html)

[树莓派-上手体验](http://blog.csdn.net/kongdefei5000/article/details/18518777/)

[树莓派开发系列教程4——树莓派网络与更新配置（有线、无线）](http://blog.csdn.net/xdw1985829/article/details/38817195)

[利用USB网卡配置树莓派为无线热点](https://www.embbnux.com/2015/02/08/setup_raspberry_to_wifi_access_point_with_rtl8188/)

[树莓派3命令行配置wifi无线连接和蓝牙连接](https://www.embbnux.com/2016/04/10/raspberry_pi_3_wifi_and_bluetooth_setting_on_console/)

~~如果是Raspberry Pi 3而且系统用的是RASPBIAN JESSIE WITH PIXEL版，外接显示器都不需要，可以直接在SD卡中的/boot目录下wpa_supplicant.conf文件中添加以下内容即可：~~

```
network={
        ssid="你的WiFi名"
        psk="你的WiFi密码"
        key_mgmt=WPA-PSK
}
```

## SSH登陆树莓派

Windows下SSH推荐使用[MobaXterm](http://mobaxterm.mobatek.net/download.html)，它兼具了FTP等很多功能，也可以使用[PuTTY](http://www.putty.org/)。装好以后用分给树莓派的IP地址就能登陆。登陆以后会让输入用户名和密码，Raspbian系统默认的用户名是*pi*，密码是*raspberry*。

Ubuntu直接在终端中输入`ssh pi@[分配给树莓派的IP地址]`，然后输入密码就可以了。Ubuntu下的FTP推荐使用FileZila。

## VNC登陆树莓派

SSH登陆只能在命令行环境中使用，使用VNC可以直接在笔记本上登陆Raspberry Pi的桌面环境。

Windows下推荐使用[VNC-Viewer](https://www.realvnc.com/download/viewer/)，Ubuntu下可以直接使用系统自带的Remmina。

然后需要先在SSH环境下为树莓派安装VNC服务器端程序并设置VNC的密码（不是Raspberry Pi的密码）：

		sudo apt-get install tightvncserver
		vncpasswd

树莓派启动VNC服务：

		sudo tightvncserver :1

`1`表示VNC窗口号，可以开多个VNC服务，一般使用第1个，缺省的话默认使用0号，一般不用。

然后笔记本通过VNC-Viewer或者Remmina使用`[树莓派IP]:1`的方式就可以登陆了。

树莓派输入`sudo tightvncserver -kill :1`命令结束VNC服务。

## 基本设置

* 输入`sudo raspi-config`进行一些基本的设置，设置第一项expand filesystem，设置第四项internationalisation options，进入后选change_locale，空格键表示打勾或去掉勾，去掉en_GB.UTF-8，选上en_US.UTF-8，zh_CN.UTF-8，zh_CN.GBK，按TAB键切换到OK。

* [如何让树莓派显示中文](http://shumeipai.nxez.com/2016/03/13/how-to-make-raspberry-pi-display-chinese.html)

* 更换软件源

```bash
cd /etc/apt
cp sources.list sources.list_back
# 修改
sudo nano sources.list
# 【wheezy版本】修改为：deb http://mirrors.neusoft.edu.cn/raspbian/raspbian wheezy main contrib non-free rpi
# 【jessie版本】修改为：deb http://mirrors.neusoft.edu.cn/raspbian/raspbian jessie main contrib non-free rpi
# 更新软件源
sudo apt-get update
# 更新软件
sudo apt-get upgrade
```
