---
title: pyserial
tags:
- RaspberryPi
categories:
- Robotics
---

### pyserial

[项目文档](http://pythonhosted.org/pyserial/)

[项目主页](https://github.com/pyserial/pyserial)

<!--more-->

* 安装

	sudo apt-get install python-serial

* 查看串口设备端口

1. `ls /dev/tty*`

2. `lsusb`

3. `python -m serial.tools.list_ports`

* 初步使用

```bash
python

>>> import serial
>>> ser = serial.Serial('/dev/ttyUSB0', 9600)
>>> ser.write("****")
>>> receive = ser.readline()
>>> ser.close()
```

### RaspberryPi和Arduino通过串口通信

RaspberryPi和Arduino串口通信可以使用GPIO口连接和USB串口线连接，但是好像RaspberryPi 3的GPIO串口和板载蓝牙有冲突，需要设置才能使用，所以建议使用USB转串口线连接Arduino的串口GPIO口，或者直接使用Arduino的USB下载线也可以。

* RaspberryPi端

```python
 # -*- coding: utf-8 -*  
import serial  
import time  
# 打开串口  
ser = serial.Serial("/dev/ttyUSB0", 9600)  
def main():  
    while True:  
        # 获得接收缓冲区字符  
        count = ser.inWaiting()  
        if count != 0:  
            # 读取内容并回显  
            recv = ser.read(count)  
            ser.write(recv)  
        # 清空接收缓冲区  
        ser.flushInput()  
        # 必要的软件延时  
        time.sleep(0.1)  

if __name__ == '__main__':  
    try:  
        main()  
    except KeyboardInterrupt:  
        if ser != None:  
            ser.close()  
```
* Arduino端

```c
void setup()
{
	Serial.begin(9600);
}
void loop()
{
	Serial.print("Hello RaspberryPi \n");
	if(Serial.available()>0)
	{
		char c=Serial.read();
		Serial.println(c);
	}
}
```

### 常用串口调试工具（Linux系统下）

#### xgcom

个人推荐使用这个。

* 源码下载

<http://code.google.com/p/xgcom/>

也可以从其他人的github上clone：`clone https://git.oschina.net/osc_allen/xgcom.git`

* 安装依赖库

```bash
sudo apt-get install automake
sudo apt-get install libglib2.0-dev
sudo apt-get install libvte-dev
sudo apt-get install libgtk2.0-dev
```

* 安装xgcom

进入xgcom目录下

```bash
./autogen.sh
make
sudo make install
xgcom
```

#### minicom

* 安装minicom

	sudo apt-get install minicom

* 启动minicom

	\# 串口设备名和波特率根据自己的做相应修改

	minicom -b 9600 -o -D/dev/ttyAMA0
