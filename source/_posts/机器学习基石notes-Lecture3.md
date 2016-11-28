---
title: 机器学习基石notes-Lecture3 Types of Learning
date: 2016-11-10 21:50:04
tags: Machine Learning
categories:
- notes
- 机器学习基石
---

机器学习的分类

<!--more-->

## 输出空间${\cal Y}$

* 二分类：${\cal Y} = \{-1,+1\}$

* 多类别分类：${\cal Y} = \{1,2, \cdots ,K\}$

* 回归：${\cal Y} = {\Bbb R}$

* 结构化学习：${\cal Y} = $结构

## 数据标记${\sf y}_n$

1. 监督学习

2. 无监督学习

* 聚类

* 密度估计

* 异常检测

3. 半监督学习

4. 增强学习

## 获取数据方式${\sf f} \Rightarrow (X_n,{\sf y}\_n)$

* 批量（batch）：填鸭式学习

* 在线（online）：被动循序学习

* 主动（active）：主动学习，问问题进而可以很快的提高

## 输入空间${\cal X}$

* 具体特征（concrete features）：经过了人类智慧对这个问题的描述，经过了人类的预处理

* 原始特征（raw features）：

例：手写数字识别

具体特征：(对称程度，密度(复杂程度))

原始特征：16x16的灰度图像转化为256维的特征向量（0,0,0.9,0.6，$\cdots$）

* 抽象特征（abstract features）

例：视频推荐系统

具体特征：用户的特征

原始特征：图像特征

抽象特征：用户、视频的ID