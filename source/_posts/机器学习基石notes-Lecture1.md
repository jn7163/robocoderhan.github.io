---
title: 机器学习基石notes-Lecture1 The Learning Problem
date: 2016-11-02 08:51:59
tags:
- Machine Learning
categories:
- notes
- 机器学习基石
---

> [课程主页](https://www.coursera.org/course/ntumlone)，教材主页[Learning From Data](http://amlbook.com/)

<!--more-->

## 课程介绍

机器学习包括了理论与实践两部分，该课程从基础出发，从而掌握机器学习这个工具。

总体分为when why how better四个部分。

## 什么是机器学习

学习：从观察通过学习转化为技巧

机器学习：通过对数据的处理转化为技巧

技巧：可以提高某方面的表现

### 为什么使用机器学习

某些问题很难通过直接的编程解决，而通过进行很多的学习可以更好地解决

### 一些应用场景

* 没办法编程

* 无法进行严格的定义

* 人类无法快速解决的问题

* 个性化的问题

### 机器学习的关键

* 存在某些潜在的模式可以学习

* 没有办法编程定义

* 数据

## 机器学习的应用

食、衣、住、行、育、乐

## 机器学习的组成

> 机器学习：通过对数据样本的学习进而获得一个与目标函数接近的假设

![机器学习简易符号表示](http://ofzyomgms.bkt.clouddn.com/machinelearningfoundations/2016110201.png)

${\cal A}$ takes ${\cal D}$ and ${\cal H}$ to get ${\mit g}$

例：银行发放信用卡的标准

* 输入：$x \in {\cal X}$（用户申请）

* 输出：$y \in {\cal Y}$（是否发信用卡）

* 潜在模式$\iff$目标函数

  $${\mit f}:{\cal X} \to {\cal Y}$$（理想未知的信用卡发放公式）

* 数据$\iff$训练样本：$${\cal D} = \{(x_1,y_1),(x_1,y_1),\cdots,(x_N,y_N)\}$$（银行历史记录数据）

* 假设$\iff$函数

  $${\mit g}:{\cal X} \to {\cal Y}$$（学习到的可用的准则）

![机器学习简易符号表示](http://ofzyomgms.bkt.clouddn.com/machinelearningfoundations/2016110202.png)

机器学习详细流程

学习模型包括${\cal A}$和${\cal H}$两部分

## 机器学习与其它领域的关系

### 机器学习（Machine Learning）与数据挖掘（Data Mining）

> 数据挖掘：从数据中挖掘某种有趣的关系、性质

* 当有趣的性质本身就是某种与目标函数接近的假设，那么数据挖掘就与机器学习相同

* 当有趣的性质与目标函数接近的假设有关系时，机器学习和数据挖掘相互关联

* 数据挖掘侧重大数据中的高效计算

很难将两者进行明确的划分

### 机器学习（Machine Learning）与人工智能（Artificial Intelligence）

> 人工智能：使机器具有某种智能行为

* 机器学习是实现人工智能的一种方法

例：下棋问题

传统人工智能方法：搜索树、$\alpha-\beta$剪枝算法

机器学习方法：通过对棋盘的数据集进行学习到掌握下棋方法

### 机器学习（Machine Learning）与统计学（Statistics）

> 统计：使用数据对未知的事情进行推论

* 假设是某种推论，目标函数是未知，统计是实现机器学习的某种方法

* 传统的统计学是通过数学推论推出结果，机器学习从可计算的角度出发

一些机器学习中使用的某些工具和方法很早就在统计学中使用过