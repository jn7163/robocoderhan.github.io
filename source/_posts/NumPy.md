---
title: NumPy
date: 2016-10-30 15:05:16
categories:
- 使用手册
---

Numpy常用函数总结。

> 总结参考[机器学习算法原理与编程实践](https://book.douban.com/subject/26667987/)

<!--more-->

### 矩阵的初始化

* 导入包

		import numpy as np 

* 创建全0和全1矩阵

		myZero = np.zeros([3,5])
		myOnes = np.ones([3,5])
		print myZero, myOnes

* 生成随机矩阵

		myRand = np.random.rand(3,4) #3行4列0到1之间的随机数矩阵

* 单位阵

		myEye = np.eye(3) #3x3的单位阵

### 矩阵的元素运算

	from numpy import *

* 元素相加减

		myOnes = ones([3,3])
		myEye = eye(3)
		print myOnes + myEye
		print myOnes - myEye

* 矩阵数乘

$$(cA)_{i,j} = c \cdot A_{i,j}$$

		mymatrix = mat( [[1,2,3],[4,5,6],[7,8,9]])
		print 10*mymatrix

* 矩阵所有元素求和

		sum(mymatrix)

* 矩阵各元素的积

		mymatrix2 = ones([3,3])
		print multiply(mymatrix,mymatrix2)

* 矩阵各元素的n次幂

		power(mymatrix,2)

### 矩阵的乘法

		mymatrix = mat([[1,2,3],[4,5,6],[7,8,9]])
		mymatrix2 = mat([[1],[2],[3]])
		dot(mymatrix,mymatrix2)
		print mymatrix * mymatrix2

### 矩阵的转置

		mymatrix.T

### 矩阵的行列数、切片、复制、比较

		[m,n] = shape(mymatrix)
		myscl1 = mymatrix[0]
		myscl2 = mymatrix.T[0]
		mycpmat = mymatrix.copy()

### 矩阵的行列式

		A = mat( [[1,2,3],[4,5,6],[7,8,9]])
		linalg.det(A)

### 矩阵的逆

		linalg.inv(A)

### 矩阵的秩

		linalg.matrix_rank(A)

### 可逆矩阵求解

		linalg.solve(A,T(b))

### 向量范数

		linalg.norm(A)

### 均值、方差、协方差、相关系数

		A = mat([[88,96,104,111],[12,14,16,18]])
		mean(A[0])
		std(A[0])
		cov(A)
		corrcoef(A)

### 特征值、特征向量

		A = [[8,1,6],[3,5,7],[4,9,2]]
		evals, evecs = linalg.eig(A)