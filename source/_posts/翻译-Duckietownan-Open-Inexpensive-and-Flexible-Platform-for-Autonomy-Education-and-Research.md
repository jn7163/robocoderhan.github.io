---
title: 翻译 Duckietown an Open, Inexpensive and Flexible Platform for Autonomy Education and Research
date: 2016-11-08 23:35:42
tags: 
- Robotics
categories:
- Duckietown
---

> [Duckietown](http://duckietown.mit.edu/)是MIT的一个课程项目，该文是翻译自该小组向[International Conference on Robotics and Automation (ICRA) 2017](http://icra2017.org/)的论文。

摘要--Duckietown对自动化的教学和研究来说是一个开源的、廉价而且扩展性好的平台。这个平台由自主机器人小车（“Duckiebots”）和城市（“Duckietowns”）组成，自主小车用现成组件组装成而成，城市包含了道路、引导标示、交通信号灯障碍物和需要运输的居民（duckies）。Duckietown平台的优势是利用利用少量的投入可以实现很广泛的功能。一个Duckiebot仅仅利用一个单目摄像头来感知整个世界和一个机载的树莓派二代来执行所有的处理，到目前为止已经具备以下功能：追随车道线并且避开障碍物、行人（duckies）和其它的Duckietown；在全局地图中进行定位；在城市中导航；以及和其它的Duckiebots协作以避免发生碰撞。Duckietown对于教学来说是一个非常有用的工具，不需要开发所有与所需课程相关的基础设施和工具，这样对教育机构来说可以节省大量的资金和时间。所有的资源都是开源、可获得的，目的是为了其它的团体也可以把这个平台应用于教育和研究。到目前为止已经有另外的四个机构使用了Duckietown平台。

<!--more-->

# 简介

自动驾驶小车定位成为一项普遍并且可用的自动化应用。但是，还有很多挑战存在在实现广泛的使用前，这其中相关的更多的是整体系统方面的，而不只是单个组成部分。如包括硬件和算法的一体化设计，感知和控制的解耦和，在并发过程中最佳的处理资源分配以及多agent系统的安全。

一个现代的自动化方面的课程应该可以同时训练学生在单个系统以及系统级的方面。但是仍然存在一些挑战，第一，构建一个全尺寸的车对大多数的机构来说太昂贵的并且增加了难度和安全方面的问题。第二，构建所有的组件和结构需要大量的时间，并且多数浪费在与希望的目的不同上的任务上的。

![图1](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2001.jpg)

为了解决这个问题，我们意将Duckietown（图1）做成一个自动化方面教育和研究的开源平台。这个平台包括了一个叫做“Duckiebots”的小车。最小化系统仅仅只是用了一个树莓派二代做所有的计算和一个单目摄像头做感知，而且已经具备了完整的但系统和多系统机器人的功能。Duckiebots住在用模块化的组件拼装而成的丰富的“Duckietowns”微模型里。这个项目里异想天开和引人注目的外观是考虑到功能上延伸性的深思熟虑的选择。Duckietowns和Duckiebots是很容易进行复制并且花费低廉，每辆小车大概只需要花费150美元，场地模型每平方的造价仅仅在2美元左右。

为了使Duckietown能满足在感知、规划和控制等不等难度程度下的使用，它的设计经过了认真的考虑，使得这个平台可以适应非常广泛的应用，覆盖了从本科生教育到进行研究阶段的问题。比如说，单个的Duckiebot仅仅通过进行车道线检测和对控制信号做出反应就可以顺利的在场地中行驶，同时也可以通过识别道路信号进行点到点的导航。在转弯处，既可以通过贴在信号灯上的标志物（AprilTags[^1]）也可以通过物体检测进行信号检测的模拟。实现一些更加复杂的功能，例如基于视觉的多机器人协调，尤其是计算能力方面的限制，使其可以达到了研究层面的挑战。

Duckietown平台突出的优势是它是有一个复杂的软件架构作支持，包含了标定、可配置、初级的感知、物体检测、非线性估计、全局定位、顶层的规划和分散协调模块。我们的目标提供一个可以与复杂的大规模的实现（如[^2]）媲美的完善的教科书式的架构,并且同时对初学者保证了可读性和易理解性。

Duckietown主要的用途是为自动化班的本科生或毕业年级的同学提供帮助。Duckietown尤其适为了完善不同子系统而需要一起工作的同学进行小组学习。这就是它在MIT2016春季学年[^3]的使用方式。同时，Duckietown也非常适合只需要专注于其中一个单独的方向或子系统（比如计算机视觉或非线性控制）的人。利用这种方式可以明显的看到子系统在整个系统中的作用和产生的影响。这是NCTU[^4]的使用方式。Duckietown通过利用已知的设定好的环境方面的先验知识（例如已知的尺寸、颜色、标志物的方位、标志上的基准、允许的网络拓扑等）使得它成为一个更加可用的研究平台。作者使用Duckietown作为研究计算性能不高的情况下的感知[^5]、协同设计[^6]和可以安全相互协调驾驶的合适方案[^7]等课题的平台。

概述：第二部分介绍了使用现成的组件搭建的Duckiebot平台。另外的部分介绍了体系的构建。针对每一个功能模块，我们会介绍基础的实现，一些用到该功能某块的课题，和一些可扩展的可能方式。按照功能的复杂性介绍这个体系架构。第三部分介绍了单系统机器人进行车道线追随的流程。第四部分介绍了涉及到单系统机器人定位，规划和导航的相关子系统。第五部分介绍了涉及到多系统机器人交换信息和协调的方式。最后，我们在第六部分中针对系统级的资源管理做了讨论，在第七部分中进行了总结和结束。 

# Duckie机器人

Duckiebots是为了实现一个经济上可承受、模块化并且易于搭建的自动化小车而设计的。我们提供了三种配置：

* 最小自动化配置能够满足所有单系统机器人的功能实现。
* 扩展配置加入了一些“奢侈”的特性，以便可以更方便的进行开发，比如操纵杆和板载的无线模块。
* 集群配置包括了LED灯以作为Duckiebot信息交换并使多机器人可以相互协调。

![表一](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2021.jpg)

## 最小系统配置

1) 最小自动化配置：在最小配置方案里使用到的组件在表1中列出。这些都是非常容易可替换的现成组件。每一个机器人根据能力的不同需要15分钟左右的焊接工作和大约半个小时到一个半小时的专配工作 。

 a) 计算：所有的计算处理都依靠树莓派2，它具有900MHz的ARM和1GB的RAM。操作系统是安装了ROS Indigo的Ubuntu14.04。

 b) 运动：Duckiebot的设计是基于非常容易找到的差分驱动底盘套件，10-20美元左右就可以在网上买到。马达是通过树莓派上自带的直流电机驱动接口控制。我们使用里程计做估计，所以底盘是易于替换的部分，可以使用任意通过差分方式配置的直流电机替代。

 c) 感知：唯一用来感知的模块是一个鱼眼镜头的摄像头。摄像头通过专用的并行接口与树莓派相连。

 d) 信息交换：这个版本中的Duckiebot是通过WiFi模块进行交流（用于安装，调试，通过键盘手动控制）。

## 扩展配置

2) 扩展配置：在需要大型团队开发的教学和研究工作中，最好的方式是在网络覆盖的环境中每一个Duckiebot都配备一个板载的网络模块。这些移动热点为每一个机器人构建了一个专用的5GHz的网络，并且通过以太网直接与树莓派相连。作为可选的，使用USB接口的无线模块可以使更加方便的控制。最后，可以使用一个32GB的USB扩展外存，这样可以存储大量的数据日志文件。增加这个配置需要额外的花费75美元。

## 集群配置

3) 集群配置：这个版本的Duckiebot配备了使用PWM波控制的五个RGB LED灯。这些LED灯通过显示Duckiebot的状态和意图以达到和其它的机器人小车进行信息交换的目的。LED灯在基础版本平台的基础上增加了大约30美元的花费。

![表二](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2022.jpg)

[^8] [^9] [^10] [^11] [^12] [^13] [^14] [^15] [^16] [^17] [^18] [^19] [^20] [^21]

在表二中我们将基础版的Duckiebot与其它的一些流行的用于教育和研究的机器人做了一个比较。特别值得注意的是很少有价格低廉的机器人能够使用视觉作为首要的传感器。使用摄像头而不是深度或者红外传感器使得我们的系统更真实的表示了全规模平台。

# 车道线追随

Duckiebot最基础的功能是车道线追随。我们通过图2中的计算机视觉流程实现了车道线追随的功能，流程包含以下步骤：

* 光照变化补偿
* 道路标志物检测
* 从图像空间到现实世界的映射，基于外在和内部的校准
* 基于非参数贝叶斯过滤的车道线定位
* 车道线控制

![图2](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2002.jpg)

## 基础设施 - Duckie城道路层

Duckietowns的设计是想实现一个易于理解的形式规范的例子，如果环境可以很好的满足这个规范，那么Duckiebot就可以保证正常行驶。
Duckietowns有两层：道路层和信号层。车道线追随仅仅依赖于道路层。道路层的构建是通过对图3中的五种拼接模块进行组合实现的。道路标志物颜色和位置的精确是用来感知的规范和重要的先验的一部分。

![图3](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2003.jpg)

## 亮度补偿

光照变化是计算机视觉的一个挑战。我们通过以下步骤来描述：

* 如何将机器学习运用到自动机器人的循环当中。
* 使用先验知识的重要性。正因为此，才在Duckietowns的道路层的规范当中才规定了先验的颜色。

![图4](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2004.jpg)

这个功能的基本实现中，我们使用k-means算法在重采样的道路像素图上检测出主要的集合，我们将它们和先验的期望的红色，黄色，白色，灰色集合进行匹配。然后我们在RGB彩色空间得到一个检测到的集合和将它们平衡后的颜色映射变换。正规化表示出我们关照条件下的先验知识和道路的颜色。通道分离变换：

$$
\begin{equation}
I_{obs}^{(i)} = {a_i}I_{orig}^{(i)} + b_i + n \text{, } i \in \{R,G,B\}
\label{eq:e1}
\end{equation}
$$

$n$表示高斯噪声，而且为了正规化我们假设高斯先验的参数$a\_{i},b_{i}$。等式的结果是一个6 X 6的线性系统

$$
\begin{equation}
\begin{pmatrix} A_{data} \\ A_{reg} \\ \end{pmatrix}p_I = \begin{pmatrix} b_{data} \\ b_{reg} \\ \end{pmatrix}
\label{eq:e2}
\end{equation}
$$

这里的$A\_{data},b\_{data}$是从观测模型(1)中得到的结果，$A\_{reg},b\_{reg}$是从正规化中得到的结果。向量$p_I$是光照的参数向量。式(2)中的平方剩余误差是拟合程度的估计值和允许的检测错误值。

## 道路标志物检测

在Duckiebots的基本实现中，通过提取沿着道路线标志物边界的定向线段实现道路上的定位，然后运行一个无参数的贝叶斯过滤器。由于贝叶斯过滤引起的鲁棒性，定向线段的检测可能产生许多假阳性。

![图5](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2005.jpg)

定向线段检测的大致流程如图5所示。当每一个颜色被检测以后这个流程被运行一次。当图像被采集以后，并行执行以下两个滤波算法：Canny滤波检测边缘，HSV彩色空间阀值检测给定的颜色。结果通过与门融合。独立的线段通过概率Hough变换[^22]提取。

## 图像到道路的转换

下一步将在图像空间中检测到的定向线段变换到三维坐标中。使用先验的平面图，建立一个从图像空间到三维空间的一一对应关系图是可能的。首先，使用相机内部的标定了的点是没有失序的。同时，变换由一个单应$x_i \simeq Hx_g$表示，这里$x_i$和$x_g$是图像中和地平面上的点，$H \in {\mathbb {R}}^{3x3}$是单应矩阵，$ \simeq $表示规模等价。

![图6](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2006.jpg)

Duckietown有标定工具使得可以估计相机的特性和外在。

## 车道相对估计

为了执行车道线追随功能，我们必须获取Duckiebot侧面与车道线的距离和方向的预测。不过我们不需要知道纵坐标（沿${\cal x}$方向）。这是一个用最小充分统计量实现控制任务的很好的例子。

因为随机的很高的认知的异常值（正如图2中看到的检测直线标志物）的存在和处理模型的非线性性使得常用的参数估计方法（如使用了高斯估计的卡尔曼滤波）在这里失效了。因此，在基本的实现中我们使用了非线性、非参数的直方图滤波[^23]。

![图7](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2007.jpg)

小车$t$时刻在道路中的的状态通过降维的状态：${\cal x} := \langle d_{t},{\phi}_t \rangle$，$d_t \in [d_min,d_max]$表示侧面和道路线的横向位移（$d = 0$时表示直线是道路线的中线），$ {\phi}_t \in [{\phi}_min,{\phi}_max]$表示如图七中表示的一样代表和原点的角度。从定义的规范中我们知道了道路线的宽度$w$，以及右边（白色）和左边（黄色）分别的宽度$l_W$和$l_Y$，我们综合使用这些信息，并通过使用道路标志物检测获得的每一个线段就可以得到对状态的预测。

对于每个线段的序列都进行一次计算。我们用每个线段进行一次投票。这些投票放入直方图中，所有的直方图表示了一种计算的可能性（图7）。

当所有的线段都被运算过以后，将直方图规范化并且作为用来测量校准的概率分布函数。

## 车道控制

当我们有了Duckiebot在道路中位置和方向的估计后，就可以用来产生控制信号使它沿着车道行驶。我们明确的设计了曲线也就使得Duckiebot在转弯处（图3(e)）表现出的相对误差非常小，仍然收敛于简单线性跟踪控制。这样就减少了涉及到轨迹的参数化的工作。

我们使用了比例-微分（PD）控制，控制命令形如$u(t)=k\_{y}d(t)+k\_{\phi}{\phi}(t)$。道路线追踪实际的效果演示可以参考附件第一部分。

![图8](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2008.jpg)

我们评估了车道线追随在有圆角的方形轨道面上（图8）的表现。在理想的光照条件下，Duckiebot车道线追随表现的鲁棒性很好，平均故障时间大于30分钟，交叉轨迹误差始终在0.15m以内。

# 导航

基本的道路线追随流程作为内部的嵌套使得更复杂和有趣的功能得以实现。比如，图9中的导航流程概述。

![图9](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2009.jpg)

## 基础设施 - 第二层 - 信号层

Duckietown的第二层是信号层。这样的信号灯有两种：信号和交通灯。这一部分我们只描述指示信号，有两种类型：交通信号，道路名称。交通信号使得在十字路口可通过成为可能，图10中包含了十字路口的类型（交通信号灯和停止信号）和一些其它重要的道路信息。这些信号包含了所有的用于定位和导航的必要信息。每一个信号都另外添加了一个AprilTag[1]，可以将检测标示与检测引发的功能相互分离。我们限制性的将这些信号放置在八种固定的姿态中的一种，这样可以保证当Duckiebot停在停止线上时这些信号可以在摄像头的视野当中，并且可以使得能够自动生成指示特征图。

![图10](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2010.jpg)

## 地图表示

因为Duckietowns是用模块的拼图组合成的，可以使用一个拼图矩阵（如图3所示）完整的表示出地图。另外，我们指定了在拼图上的信号的存在、类型和位置。因此，这个矩阵可以用来自动的生成两种形式的地图，特征地图用于定位，拓扑网络图用于规划。图十一展示了这两种图。

![图11](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2011.jpg)

## 全局定位

全局定位包括Duckiebot姿态相对于全局参考系的的估计。在全局参考系中的姿态的信息对于其它功能的实现是一个关键，例如到某点的线路规划。

对于人来说，交通信号灯和道路名称对于定位提供了有用的信息。在基本的定位中我们假定自动地图生成模块（图11(a)）已经提供了地图。然而，实时定位和地图构建（SLAM）是必要的扩展。使用已知的标示的尺寸，每当一个标示被检测到时，我们可以就可以获得摄像头相对于标示的相对姿态。因为标示的姿态是已知的，每次检测都可以测量出Duckiebot的绝对位置。在图12(a)中展示了Duckiebot在Duckietown中的十字交叉路口的情况。通过内部的摄像头（图12(c)）可以定位自己在地图中的位置，如图12(b)所示。

![图12](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2012.jpg)

这个基本的算法保证无论何时检测到标示以后都可以准确的定位。在我们的测试中，Duckiebot能够以3Hz的频率进行位置估计，并且当至少有三个标示在可视范围时定位延迟大约在0.1m内。

## 决策规划

为了完成导航的任务，Duckiebot需要一个从由全局定位模型的位置到拓扑道路网络图的映射，通过它才可以搜索得到一个连接可行的路线（图11(b)）。

当图生成以后并且起始位置和目标位置确定以后我们通过$A^*$搜索算法可以生成得到最佳规划路线。一个有趣的扩展是这个可以用来同时模拟一组管理系统来为多个Duckiebot规划线路。

## 有穷状态机

从宏观层面操作Duckiebot的是通过有穷状态机（FSM）进行的，图13展示了简化的版本。有穷状态机中的转换是通过异步的基于事件的感知产生的，例如检测到了停止线。

![图13](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2013.jpg)

这种模式用于混合（离散连续）系统配置下[^24]的Duckiebot控制。根据不同的期望动作开发了一系列的控制，图13中的模式用来选择哪种控制被触发同时将其与实际的轮子驱动相连。

![图14](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2014.jpg)

## 路径遍历

当规划生成以后，需要根据每个交叉路口正确的转弯序列进行。比如图11-(b)中的序列对应[s,f,s,f,r,f,s,f]，这里的f，s，r，l分别对应追随道路线，直走，右转，左转，使得Duckiebot从节点21到节点31。

每一个交叉路口的行驶是通过执行序列状态“LANE_FOLLOWING”$\to$“INTERSECTION_TRAVERSAL”的循环直到到达某个结束态（在该点我们通过忽略“COORDINATION”行为和假设路口总是可通过的来简化这个过程）实现的。从LANE_FOLLOWING到INTERSECTION_TRAVERSAL的转换是因为到达了停止线引发的。从INTERSECTION_TRAVERSAL再回到LANE_FOLLOWING的转换是由道路线预测超过了某个阀值引起的。

做规划的时间从初始化到进行平均需要8.38秒，包括了将拼图的地图规范文件到图形表示转换，对于用于规划的道路网络包含了大致30个节点。手工选择路线，规划算法返回可视化的反馈时间大致在2.34秒以内。如果存在网络延迟的话需要更多的时间。

Duckiebot实际行驶很大程度上取决于存在小车交汇的交叉路口的数量。

# 多机器人协同

高级的功能包括多Duckiebot同时在一个Duckietown中进行交互。这种情况下，Duckiebot必须相互协调分配共有的资源。

我们考虑其中两种交互模式：交通信号灯和停止线，如图15所示。交通信号灯交互是一种简化并且集中地的交互解决方案（仅仅使用交通信号灯交互就足以构建和测试多机器人算法）。停止信号需要Duckiebot的信息交流。交流是分布式并且基于感知。我们使用LED表示目的和状态。当Duckiebot停在停止线上时情况会变得复杂，它的左侧是在摄像头视野之外。

![图15](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2015.jpg)

## 基础设施 - 交通信号灯

第二层上的第二种信号是交通信号灯。交通信号灯单元是在每条将要出现的道路正面装配的LED灯。

交通信号灯实际上是为Duckiebot建立一个无线网络。

## LED灯检测和翻译

基于LED灯的信息交换系统有两个单独的组成部分：一个是用于捕捉图像流和决定场地中所有LED灯位置和频率的检测器，另一个是接收信息和对每一个检测器标记物理对象（小车或交交通信号灯）和协调消息的解释器。

图16展示了LED信息交流的模式。图17上半部分和下半部分分别展示了LED检测器和解释器的详细情况。

![图16](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2016.jpg)

![图17](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2017.jpg)

解释器利用假设分辨检测出的LED灯在水平线以上的属于交通信号灯，水平线以下的属于Duckiebot的信息交换灯。

## 协调 - 信号灯

交通信号灯十字路口有交通信号灯盒子，朝着每一个方向都有一个LED灯。交通信号灯盒子循环的一次朝一个方向发出通行信号，这样当多个Duckiebot从不同的方向驶来的时候不会在交汇路口的中央相遇（图15）。这种交通信号灯十字路口的处理方式是很直接的：当一个Duckiebot到达停止线时，它就开始检测视野内的LED灯的闪烁频率，当检测到与通行信号相对应的频率时，就启动通过交汇路口。

## 协调 - 交叉路口

停止信号十字路口没有任何的中心建筑，因此，Duckiebot需要通过安装在它们上面的LED灯进行信息交换进而协调通过十字路口的顺序。如图15所示，每个Duckiebot只能够看到它们自己右面和前面的Duckiebot，不能看到十字路口左面的Duckiebot。并且，其它Duckiebot发出的检测信号需要一个明显的时间（约2秒），个别Duckiebot的检测时间是不同步的。基础的协调算法已经使得传感的局限到可控范围。直观地讲，协议遵守以下几条规则：(a)每个Duckiebot需要服从于右边的Duckiebot（因为它位于它右边Duckiebot的可视范围之外），(b)当两个相向而来的Duckiebot到达交互路口时，先到的那个先行驶通过，(c)当相向而来的两个Duckiebot根据检测精度几乎同时到达时，通过等待一个随机的时间间隔再尝试与另一个Duckiebot协调沟通来解决这个问题。

协调的模式是通过一系列图13中的状态进行处理。处于AT_STOP_CLEARING时，Duckiebot需要等待预设的时间以保证交汇路口是可通行的，处于AT_STOP_CLEAR时，Duckiebot需要等待到没有其它的Duckiebot在交汇路口，以此保证确实是可以前进的，当处于RESERVING时，Duckiebot是表示（通过它的LED灯）它将要尝试行驶通过，CONFLICT表示两个Duckiebot同时试图通过路口，直到到GO时Duckiebot才开始行驶通过交汇路口。

## 评估

我们对两种类型下的协调行为进行了测试并实际连续进行了几个小时。当预想的算法出现时，比如，小车停在了沿着道路方向的停止线上，协调行为可以可靠地安排每次一辆小车行驶通过交汇路口因此避免碰撞。在停止信号交汇路口的协调结果如图18所示。信号A，B和C分别对应绿色，红色和黄色LED灯。

![图18](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2018.jpg)

# 资源管理

我们的目的是搭建一个不昂贵但是可用的自动化教育和研究的平台。但是，当成本降低的同时可用的资源，如传感器、计算性能、内存、电源以及带宽也会被降低。

为了达到我们的目的就需要在软件架构上开发增加的算法。我们已经在设计中使用了两种基础的策略来减少资源开销：基于事件的计算和驱动模式感知。

## 基于事件的计算

1)基于事件的计算：在我们的实现中我们利用了机器人操作系统(ROS)，它重要使用了发布-订阅模式用于数据交换。

因为平台计算资源的限制，程序（节点）使用基于事件和限制频率的处理和发布模式：一个节点只当输出与上次发布的输出有明显的不同并且在发布频率以内时才发布。

### 模型 - 驱动型感知

2)驱动模式的感知：最多的计算集中在与感知有关的任务上，尤其是我们只使用视觉作为传感模块。为了实现多机器人最为复杂的功能就需要机器人处理很多的感知任务（直线检测、道路线滤波、停止线滤波、信号检测、LED检测、LED解码、小车检测）不过不是同时的。因此我们利用一些开关。这些开关被所有节点订阅并且允许它们只选择订阅那些只有当它们应该作用时的模式下的必要的输入（图像）。表3展示了在每一种FSM模式中哪种感知模块会起作用的概况。

![表三](http://ofzyomgms.bkt.clouddn.com/duckietown/duckietown_icra_2017%2023.jpg)

我们使用了与之相似的办法在根据给出任务的请求来控制摄像头图像的分辨率。

# 结论

我们展示了一个可扩展的用于自动化教育和研究的平台Duckietown。我们在开发这个系统中通过精确地规范和资源管理使其得以实现。

Duckietown已经（或计划在下个学期使用）被五个机构使用：清华大学（中国）、国立交通大学（台湾）、麻省理工大学MIT、伦斯勒理工学院、罗格斯大学（美国）。

所有的资料都是基于开源/免费软件协议的；资料可以在<http://duckietown.mit.edu>网站找到。我们希望其他机器人团体的人可以使用这个平台并且帮助它成长。

# 致谢

This work was supported by the National Science Foundation, with National Robotics Initiative award IIS-1405259, and with Robust Intelligence award IIS-1318392. Additional support was given by the Toyota Research Institute and the Ford Motor Company. We would also like to acknowledge the contributions made by the studens during the Spring 2016 semester of the MIT 2.166 class.

# 引用

[^1]: E. Olson, “AprilTag: A robust and flexible visual fiducial system,” in 2011 IEEE International Conference on Robotics and Automation. Institute of Electrical & Electronics Engineers (IEEE), may 2011. [Online]. Available: http://dx.doi.org/10.1109/icra.2011.5979561
[^2]: P. Furgale, U. Schwesinger et al., “Toward automated driving in cities using close-to-market sensors: An overview of the v-charge project,” in 2013 IEEE Intelligent Vehicles Symposium (IV). Institute of Electrical & Electronics Engineers (IEEE), jun 2013. [Online]. Available: http://dx.doi.org/10.1109/ivs.2013.6629566
[^3]: J. Tani, L. Paull et al. (2016) Duckietown: an innovative way to teach autonomy, submitted to EDUROBOTICS 2016. [Online]. Available: http://tiny.cc/9vduey
[^4]: H.-C. Wang. (2016) Icn9005 robotic vision. [Online]. Available: duckietown.nctu.edu.tw
[^5]: L. Paull, G. Huang, and J. J. Leonard, “A unified resource-constrained framework for graph SLAM,” in 2016 IEEE International Conference on Robotics and Automation (ICRA), May 2016, pp. 1346–1353.
[^6]: A. Censi, “A mathematical theory of co-design,” CoRR, vol. abs/1512.08055, 2015. [Online]. Available: http://arxiv.org/abs/1512.08055
[^7]: D. Hoehener, G. Huang, and D. D. Vecchio, “Design of a lane departure driver-assist system under safety specifications,” in Conference on Decision and Control, 2016.
[^8]: M. Rubenstein, B. Cimino et al., “AERobot: An affordable one-robotper-student system for early robotics education,” in IEEE International Conference on Robotics and Automation (ICRA), 2015, pp. 6107–6113.
[^9]: M. Rubenstein, C. Ahler, and R. Nagpal, “Kilobot: A low cost scalable robot system for collective behaviors,” in 2012 IEEE International Conference on Robotics and Automation. Institute of Electrical & Electronics Engineers (IEEE), may 2012.
[^10]: B. Thursk`y and G. Gaˇspar, “Using Pololu’s 3pi robot in the education process.” [Online]. Available: http://tiny.cc/zfluey
[^11]: S. Kernbach, “Swarmrobot.org - Open-hardware microrobotic project for large-scale artificial swarms,” arXiv preprint arXiv:1110.5762,2011. [Online]. Available: http://arxiv.org/pdf/1110.5762v1.pdf
[^12]: P. Robinette, R. Meuth et al., “LabratTM: Miniature robot for students, researchers, and hobbyists,” in 2009 IEEE/RSJ International Conference on Intelligent Robots and Systems, Oct 2009, pp. 1007–1012.
[^13]: F. Riedo, M. Chevalier et al., “Thymio II, a robot that grows wiser with children,” in 2013 IEEE Workshop on Advanced Robotics and its Social Impacts. Institute of Electrical & Electronics Engineers (IEEE), nov 2013. [Online]. Available: http://dx.doi.org/10.1109/arso.2013.6705527
[^14]: P. Inc. (2016) Scribbler s3 robot. [Online]. Available: https://www.parallax.com/product/28333
[^15]: R. K. Cole, “STEM outreach with the Boe-Bot,” Robots in K-12 Education: A New Technology for Learning: A New Technology for Learning, p. 245, 2012.
[^16]: Parallax. (2016) Activitybot robot kit. [Online]. Available: https://www.parallax.com/product/32500
[^17]: M. Dekan, F. Duchoˇn et al., “iRobot create used in education,” Journal of Mechanics Engineering and Automation, vol. 3, no. 4, pp. 197–202,2013.
[^18]: Bot’n Roll. (2016) Bot’n roll ONE A. [Online]. Available: http://botnroll.com/onea en/
[^19]: J. McLurkin, A. McMullen et al., “A robot system design for low-cost multi-robot manipulation,” in 2014 IEEE/RSJ International Conference on Intelligent Robots and Systems. IEEE, 2014, pp. 912–918.
[^20]: Robots in Course. (2016) Hemission. [Online]. Available: http://www.robotsinsearch.com/products/hemisson
[^21]: S. Wilson, R. Gameros et al., “Pheeno, a versatile swarm robotic research and education platform,” IEEE Robotics and Automation Letters, vol. 1, no. 2, pp. 884–891, July 2016.
[^22]: D. H. Ballard, “Generalizing the Hough transform to detect arbitrary shapes,” Pattern recognition, vol. 13, no. 2, pp. 111–122, 1981.
[^23]: S. Thrun, W. Burgard, and D. Fox, Probabilistic Robotics, 2005.
[^24]: T. A. Henzinger, “The theory of hybrid automata,” in Logic in Computer Science, 1996. LICS ’96. Proceedings., Eleventh Annual IEEE Symposium on, Jul 1996, pp. 278–292.

