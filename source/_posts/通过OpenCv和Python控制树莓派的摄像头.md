---
title: 通过OpenCv和Python控制树莓派的摄像头
tags:
- RaspberryPi
categories:
- Robotics
---

[Accessing the Raspberry Pi Camera with OpenCV and Python by Adrian Rosebrock](http://www.pyimagesearch.com/2015/03/30/accessing-the-raspberry-pi-camera-with-opencv-and-python/)

<!--more-->

## 将摄像头模块装到树莓派上

## 设置摄像头模块有效

	sudo raspi-config

选择`Option 5: Enable camera`，`回车键`，`Finish`按钮。

## 测试摄像头模块

最好有一个外界显示器，或者通过VNC访问树莓派。

	raspistill -o output.jpg

## 安装picamera

picamera是树莓派摄像头模块的Python接口。

如果在树莓派上安装OpenCv也是按着这个博主的教程完成的话，应该是有一个通过`virtualenv`和`virtualenvwrapper`创建好了的`cv`虚拟环境。

	source ~/.profile
	workon cv

	pip install "picamera[array]"

## 使用Python和OpenCv获得单个图像

新建一个文件`test_image.py`，输入以下代码：

```python
# import the necessary packages
from picamera.array import PiRGBArray
from picamera import PiCamera
import time
import cv2

# initialize the camera and grab a reference to the raw camera capture
camera = PiCamera()
rawCapture = PiRGBArray(camera)

# allow the camera to warmup
time.sleep(0.1)

# grab an image from the camera
camera.capture(rawCapture, format="bgr")
image = rawCapture.array

# display the image on screen and wait for a keypress
cv2.imshow("Image", image)
cv2.waitKey(0)
```

	python test_image.py

## 使用Python和OpenCv获得视频流

新建一个文件`test_video.py`，输入以下代码：

```python
# import the necessary packages
from picamera.array import PiRGBArray
from picamera import PiCamera
import time
import cv2

# initialize the camera and grab a reference to the raw camera capture
camera = PiCamera()
camera.resolution = (640, 480)
camera.framerate = 32
rawCapture = PiRGBArray(camera, size=(640, 480))

# allow the camera to warmup
time.sleep(0.1)

# capture frames from the camera
for frame in camera.capture_continuous(rawCapture, format="bgr", use_video_port=True):
	# grab the raw NumPy array representing the image, then initialize the timestamp
	# and occupied/unoccupied text
	image = frame.array

	# show the frame
	cv2.imshow("Frame", image)
	key = cv2.waitKey(1) & 0xFF

	# clear the stream in preparation for the next frame
	rawCapture.truncate(0)

	# if the `q` key was pressed, break from the loop
	if key == ord("q"):
		break
```

	python test_video.py

------

博主中的文章里的图像和视频都是可以直接在窗口中显示出来的，不需要外界显示器，不需要通过VNC登陆，通过命令行就可以，我的系统是Ubuntu 14.04，通过终端登陆就可以显示。

	ssh -X pi@[你的树莓派的IP地址]

当运行test_video.py时，窗口是黑色没有图像的话可以通过下面的方式解决

	pip uninstall picamera
	pip install 'picamera[array]'==1.10

还有以下解决办法可以尝试

重启一下：`sudo reboot now`

代码中的bgr改成RGB

升级固件：`sudo raspi-update`
