---
title: Day13
date: 2019-07-27 18:48:06
tags: 实习
cover: https://i.loli.net/2019/07/17/5d2e73bb14bd344648.png
---

# 边缘检测之梯度详解

梯度的概念和性质在边缘检测中比较重要，所以我又仔细看了一下相关知识点，总结一下。

## 梯度概念
梯度是一个可以确定图像f的(x,y)位置处的边缘方向和强度的工具，用 $\triangledown f$来表示，用 **向量**来定义

$$\triangledown f = \begin{bmatrix} g_x \\ g_y \\ \end{bmatrix} = \begin{bmatrix} \frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial x} \end{bmatrix}$$

## 梯度性质

1. 梯度向量大小。    
$\triangledown f$的大小为M(x,y)
$$\triangledown f = mag(\triangledown f)=[g_x^2+g_y^2]^{
1/2}= [(\partial f/\partial x)^2+(\partial f/\partial y)^2]^{1/2}$$
这是梯度向量方向变化率的值。其中$g_x$,$g_y$和M(x,y)都是和原图像大小相同的图像。称M(x,y)为梯度图像。

2. 梯度向量的方向
$$\alpha (x,y) = tan^{-1}(\frac{g_x}{g_y})$$
同理，　$\alpha(x,y)$也是由$g_x$和$g_y$阵列创建的尺寸相同的图像。    
任意点(x,y)处的一个边缘的方向与该点处梯度向量的方向$\alpha(x,y)$正交。  
梯度向量有时也称为边缘法线

3. 梯度指出f在(x,y)处的最大变化率的方向

## 梯度算子
### 一维模板

$$g_x = \frac{\partial f(x,y)}{\partial x} = f(x+1,y) - f(x,y)$$
和
$$g_y = \frac{\partial f(x,y)}{\partial y} = f(x,y+1)-f(x,y)$$

### 二维模板

罗伯特交叉梯度算子(Roberts),ROberts算子以求对角像素之差为基础:
$$g_x = \frac{\partial f}{\partial x} = (z_9 - z_5)$$
和
$$g_y = \frac{\partial f}{\partial y} = (z_8 - z_6)$$

### 3×3模板

#### Prewitt算子
$$g_x = (z_7 + z_8 + z_9)-(z_1 + z_2 + z_3)$$
和
$$g_y = (z_3 + z_6 + z_9) - (z_1 + z_4 + z_7)$$

#### Sobel算子

$$g_x = (z_7 + 2z_8 + z_9)-(z_1 +2 z_2 + z_3)$$
和
$$g_y = (z_3 + 2z_6 + z_9) - (z_1 +2 z_4 + z_7)$$

在中心位置处使用2可以平滑图像

注意:所有模板中的系数之和为0，这意味着恒定灰度的响应为0.
