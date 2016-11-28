---
title: CS229-Machine Learning notes-Lecture2-监督学习
date: 2016-11-05 09:06:40
tags:
- Machine Learning
categories: 
- notes
- CS229-Machine Learning
---

# 基础知识与引入

* 常用符号

* 机器学习的过程

# 线性回归

损失函数

$${\mit J}(\theta) = \frac{1}{2}\sum_{i=1}^{m} {(h_\theta(x^{(i)}) - y^{(i)})}^2$$

## 梯度下降算法

看原视频中的动画讲解或者复习微积分知识中偏导数的梯度问题

问题： 选取不同的初始值可能会得到不同的局部最优值

$$ {\theta}_j := {\theta}_j-\alpha \frac{\partial}{\partial {\theta}_j} {\mit J}(\theta) $$

$$ {\theta}_j := {\theta}_j + \alpha(y^{(i)} - h_{\theta}(x^{(i)}))x_j^{(i)} $$

批量梯度下降

$$ {\theta}_j := {\theta}_j + \alpha\sum_{i=1}^{m} {(y^{(i)} - h_{\theta}(x^{(i)}))x_j^{(i)}} $$

随机梯度下降

$$ {\theta}_j := {\theta}_j + \alpha(y^{(i)} - h_{\theta}(x^{(i)}))x_j^{(i)} $$

## 正规方程

$$X^TX\theta = X^T\vec{y}$$

### 最小二乘法

$$\theta = (X^{T}X)^{-1}X^{T}\vec{y}$$

## 概率解释

为什么使用平方损失函数

误差项满足独立同分布
如何根据给定的数据的概率和参数的似然性来估计参数
极大似然估计（选择参数$\theta$使得数据出现的可能性尽可能大，使似然性最大化）

误差项满足高斯分布并且独立同分布的情况下使似然性最大化

## 局部加权回归


对数似然性最大化
为了使函数最大化
可以使用梯度下降算法（使二次函数最小化）