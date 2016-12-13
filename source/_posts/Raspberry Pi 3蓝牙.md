---
title: Raspberry Pi 3蓝牙
tags:
- RaspberryPi
categories:
- Robotics
---
RaspberryPi 3 板载蓝牙的坑比较多，在蓝牙和串口可选择的情况下还是建议直接使用串口。

<!--more-->

### 蓝牙配置

[树莓派3 model:B 连接蓝牙设备](https://zhuanlan.zhihu.com/p/20713396)

### 蓝牙编程

[pybluez](https://github.com/karulis/pybluez)
[Bluetooth programming with Python - PyBluez](http://people.csail.mit.edu/albert/bluez-intro/c212.html)

### 基本知识与指令

蓝牙的PIN code一般默认值为：1234

```bash
# 蓝牙配置
sudo bluetoothctl
power on
scan on
agent on
pair <MAC地址>
trust <MAC地址>
connect <MAC地址>
quit

# 查看蓝牙状态
systemctl status bluetooth
```
