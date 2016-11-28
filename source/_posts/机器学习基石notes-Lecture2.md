---
title: 机器学习基石notes-Lecture2 Learning to Answer Yes/No
date: 2016-11-02 14:15:58
tags: Machine Learning
categories:
- notes
- 机器学习基石
---

## 感知机模型

<!--more-->

对于一组特征向量 $X = (X_1,X_2,\cdots,X_d)$，${\cal y}: \lbrace +1,-1 \rbrace $，将特征向量与权值向量做内积，其计算结果与阀值作比较进而输出结果

${\mit h} \in {\cal H}$
$$
\begin{align}
{\mit h}(X) & = sign\left(\left(\sum_{i=1}^d { {W_i} {X_i} }\right) - threshold\right) \\
& = sign\left(\left(\sum_{i=1}^d { {W_i} {X_i} }\right) + \underbrace{(-threshold)}_{W_0}\cdot\underbrace{(+1)}_{X0}\right) \\
& = sign\left(\sum_{i=0}^d { {W_i} {X_i} }\right) \\
& = sign({W^T}X)
\end{align}
$$

例：在二维空间中，h函数表示为${\mit h}(X) = sign(W_0+{W_1}{X_1}+{W_2}{X_2})$
![二维空间感知机模型](http://ofzyomgms.bkt.clouddn.com/machinelearningfoundations/2016-11-02_194509.png)
空间中的点表示特征向量，标签y表示输出值，O表示+1，x表示-1，空间中的线表示假设

## 感知机学习算法

通过不断修正的方式，初始化的$W_0$，通过${\cal D}$训练样本不断修正。

具体算法：

在${\mit t} = 0,1,\cdots$循环过程中

1. 存在$( {X_n(t)} , { {\mit y}_n(t)} )$在$W_t$的规则下是错误的，即$sign( {W_t^T}X_n(t) ) \not= {\mit y}_n(t)$

2. 通过$W_{t+1} \leftarrow W_t + {\mit y}_n(t)X_n(t)$进行修正

3. 直到没有错误返回$W$为${\mit g}$

![感知机算法修正示意图](http://ofzyomgms.bkt.clouddn.com/machinelearningfoundations/2016-11-02_215946.png)

实际上W即为分割线的法向量，sign即为X在W方向上的分量与W是同向还是反向。

## 感知机算法的收敛性

PLA数据线性可分

假设${\cal D}$是线性可分的，那么PLA是否一定会停止



## 非线性可分数据集

但是PLA的前提是指导数据集是线性可分的

数据集中普遍存在噪声

解决办法

找一个在已知数据集上犯错最少的线

初始化当前最优预测向量$\hat{W}$，在${\mit t} = 0,1,\cdots$循环过程中

1. 存在$( {X_n(t)} , { {\mit y}_n(t)} )$在$W_t$的规则下是错误的

2. 通过$W_{t+1} \leftarrow W_t + {\mit y}_n(t)X_n(t)$进行修正

3. 如果新的$W\_{t+1}$比$\hat{W}$犯的错少，更新$\hat{W}$为$W_{t+1}$ 

3. 直到做了足够多次循环，返回$\hat{W}$为${\mit g}$
