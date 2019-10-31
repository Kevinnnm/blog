---
title: Day09
date: 2019-07-23 10:02:38
tags: 实习
cover: https://i.loli.net/2019/07/17/5d2e73bb14bd344648.png
---
# 图像分割
---
> 整体等于部分之和    
>                 -----欧几里德

---
图像分割把图像细分为它的组成要素或物体，细分的水平取决于要解决的问题。    
单色分割的分割算法通常是基于图像亮度值的两个基本特征:不连续性和相似性。第一类，方法是基于亮度的突变来分割一幅图像，比如边缘;第二类，主要方法是根据事先定义好的准则把图像分割成相似的区域

## 点、线和边缘检测

### 背景知识
1. 边缘像素是图像中灰度突变的像素，边缘是连接的边缘像素的集合
2. 一条线可以视为一条边缘线段，该线段两侧的背景灰度要么远亮于该线像素的灰度，要么远暗于该线像素的灰度。孤立点可视为一条线，只是长度和宽度都是一个像素
3. 局部变化检测可以用微分(一阶微分和二阶微分)    
   - 对于一阶导数的任何近似，约定:
      - 在恒定灰度区域必须为0
      - 在灰度台阶和或斜坡开始处必须不为0
      - 在沿灰度斜坡点处也必须不为0
   - 类似的对于二阶导数的近似
      - 在恒定灰度区域必须为0
      - 在灰度台阶或斜坡开始除和结束处必须不为0
      - 沿灰度斜坡必须为0
   - 一维函数展开为关于x的泰勒级数,结果差分
   $$ \frac{\partial f}{\partial x}=f'(x)=f(x+1)-f(x)$$
    二阶导数
    $$ \frac{\partial ^2 f}{\partial ^2 x}=f''(x)=f(x+1)+f(x-1)-2f(x)$$
    - 可以得出结论:
      - 一阶导数通常在图像中产生较粗的边缘
      - 二阶导数对精细细节，如细线、孤立点和噪声有较强的响应
      - 二阶导数在灰度斜坡和灰度台阶过渡处会产生双边缘响应
      - 二阶导数的符号可用于确定边缘的过程是从亮到暗还是从暗到亮
    - 计算图像中每个像素位置的一阶导数和二阶导数的可选择方法是空间滤波器。模板在该区域中心点处的响应为
    $$R = w_1z_1 + w_2z_2 + ... + w_9z_9 = \sum_{k=1}^{9}w_kz_k$$

$w_1$ | $w_2$ | $w_3$
:---: | :---: | :---:
$w_4$ | $w_5$ | $w_6$
$w_7$ | $w_8$ | $w_9$

这是一个普通的3×3空间滤波器掩模

### 孤立点检测
- 点的检测应以二阶导数为基础，这意味着使用laplace
$$\triangledown ^2f(x,y) = \frac{\partial ^2 f}{\partial x^2} + \frac{\partial ^2 f}{\partial y^2}$$
偏微分之后可求得laplace为
$$\triangledown ^2f(x,y) = f(x+1,y)+f(x-1,y)+f(x,y+1)+f(x,y-1)-4f(x,y)$$
点检测laplace模板

1 | 1 | 1
:---:  | :---: | :---:
1 | -8 | 1
1 | 1 | 1


如果在某点处的该模板的响应的绝对值超过了一个指定的阈值，那么就说在模板中心位置(x,y)处的该点已经被检测到。在输出图像中，这样的点被标注为1,所有其他点被标注为0
$$g(x,y)=\begin{cases}
1,\quad |R(x,y)| \geqq T\\
0, \quad 其他
\end{cases}
$$
- MATLAB实现

```
f = imread('moon.jpg');

f = rgb2gray(f);

w = [-1 -1 -1; -1 8 -1; -1 -1 -1];

g = abs(imfilter(f, w));

T = max(g(:));

g = g >= T;

figure(1);

subplot(1,2,1)

imshow(f)

subplot(1,2,2)

imshow(g)
```
结果：
![](https://i.loli.net/2019/07/24/5d37b3a82234833730.jpg)

### 线检测
可以预期，二阶导数将导致更强的响应，并产生比一阶导数更细的线

线检测模板
- 水平

-1 | -1 | -1
:---:  | :---: | :---:
2 | 2 | 2
-1 | -1 | -1

- +45度

2 -| -1 | -1
:---:  | :---: | :---:
-1 | 2  | -1
-1 | -1 | 2

- 垂直

-1 | 2 | -1
:---:  | :---: | :---:
-1 | 2 | -1
-1 | 2 | -1

- -45度

-1 | -1 | 2
:---:  | :---: | :---:
-1 | 2  | -1
2 | -1 | -1

对于恒定的背景，当线通过模板的中间一行时可能产生更大的响应。    
每个模板的系数之和为0,这表示在恒定亮度区域内，模板的响应为0.
- MATLAB实现检测指定方向上的线
```
clc

clear


f = imread('11111.jpg');


f = rgb2gray(f);

figure(1);

subplot(2,3,1)

imshow(f);

w = [-1, 2, -1; -1 2 -1; -1 2 -1];

% g = imfilter(tofloat(f),w);

g = imfilter(f,w);

subplot(2,3,2)

imshow(g, [ ]);

gtop = g(1:120, 1:120);

% gtop = pixeldup(gtop, 4);

subplot(2,3,3)

imshow(gtop, [ ]);

gbot = g(end - 119:end, end - 119:end);

% gbot = pixeldup(gbot, 4);

subplot(2,3,4)

imshow(gbot, [ ]);


g = abs(g);

subplot(2,3,5)

imshow(g, [])


T = max(g(:));

g = g >= T;

subplot(2,3,6);

imshow(g)

```

结果:
![](https://i.loli.net/2019/07/24/5d37b3a815b7125153.jpg)
可能会用到的M函数pixeldup
```
function B=pixeldup(A,m,n)%pixeldup用来重复像素的，在水平方向复制m倍，在垂直方向复制n倍，m，n必须为整数，n没有赋值默认为m%检查输入参数个数
if nargin<2
	error('At least two inputs are required.');
	end
if nargin==2
	n=m;
	end
u=1:size(A,1);%产生一个向量，其向量中元素的个数为A的行数%复制向量中每个元素m次m=round(m);%防止m为非整数u=u(ones(1,m),:);
u=u(:);%在垂直方向重复操作
v=1:size(A,2);
n=round(n);
v=v(ones(1,n),:);
v=v(:);
B=A(u,v);
```

**慎用tofloat函数**

