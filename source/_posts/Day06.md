---
title: Day06
date: 2019-07-20 14:18:08
tags: 实习
cover: https://i.loli.net/2019/07/17/5d2e73bb14bd344648.png
---

## 使用频率域滤波器平滑图像
----
### 理想低通滤波器

在以原点为圆心，以$D_0$为半径的圆内无衰减通过所有频率，而在圆外切断所有频率的二维低通滤波器，称为理想低通滤波器(ILPF)，定义为
$$y=\begin{cases}
1,\quad D(x,y)\leq 0\\
0, \quad D(x,y) > 0
\end{cases}$$
$D_0$是一个常数，D(u,v)是频率域中心点(u,v)与频率矩形中心的距离，即
$$ D(u,v)=\lbrack{(u-\frac{P}{2})^2+(v-\frac{Q}{2})^2}\rbrack^\frac{1}{2} $$
过渡点称为**截止频率**
![](https://i.loli.net/2019/07/21/5d341bf1f0cb755202.jpg)

### 布特沃斯低通滤波器
截止频率位于距原点$D_0$处的n阶布特沃斯低通滤波器(BLPF)的传递函数的定义为:
$$H(u,v)=\frac{1}{1+{[D(u,v)/D_0]}^{2n}}$$
截止频率点是当D(u,v) = $D_0$时的点
![](https://i.loli.net/2019/07/21/5d341bf1f101518273.jpg)

### 高斯低通滤波器
二维形式:
$$H(u,v) = e^{-D^2(u,v)/2{D_0}^2} $$
$D_0$ 是截止频率
![](https://i.loli.net/2019/07/21/5d341bf1f163e99741.jpg)

### MATLAB中低通滤波器的实现
#### 1. 高斯低通滤波器

```
f = imread('1.jpg');
f = rgb2gray(f);
[f, revertclass] = tofloat(f);
PQ = paddedsize(size(f));
[U, V] = dftuv(PQ(1), PQ(2));
D = hypot(U, V);
D0 = 0.05*PQ(2);
F = fft2(f, PQ(1), PQ(2));
H = exp(-(D .^ 2)/(2 * (D0^2))); %高斯低通滤波器
g = dftfilt(f, H);
g = revertclass(g);
figure, imshow(fftshift(H));
figure, imshow(log(1 + abs(fftshift(F))), [])
figure, imshow(g);
```
滤波结果：
![](https://i.loli.net/2019/07/20/5d32d02f5468855048.jpg)

除了之前说的几个M函数外，还需要用到`dftfilt()`函数
```
function g=dftfilt(f,H)
%DFTFILT Performs frequency domain filtering.
%   G=DFTFILT(F,H) filters F in the frequency domain using the
%   filter transfer function H. The output, G, is the filtered
%   image, which has the same size as F. DFTFILT automatically pads
%   F to be the same size as H. Function PADDEDSIZE can be used
%   to determine an appropriate size for H.
%
%   DFTFILT assumes that F is real and that H is a real, uncentered,
%   circularly-symmetric filter function.

%Obtain the FFT of the padded input.
F=fft2(f,size(H,1),size(H,2));

%Perform filtering.
g=real(ifft2(H.*F));

%Crop to original size.
g=g(1:size(f,1),1:size(f,2));
```

#### 2. Butterworth滤波

该函数输入为灰度图像，自由设置截止频率$D_0$和BLPF的阶数n，输出为滤波后的图像(已归一化到[0,255])
```
function [image_out] = Bfilter(image_in, D0, N)
% Butterworth滤波器，在频率域进行滤波
% 输入为需要进行滤波的灰度图像，Butterworth滤波器的截止频率D0，阶数N
% 输出为滤波之后的灰度图像

[m, n] = size(image_in);
P = 2 * m;
Q = 2 * n;

fp = zeros(P, Q);
%对图像填充0,并且乘以(-1)^(x+y) 以移到变换中心
for i = 1 : m
    for j = 1 : n
        fp(i, j) = double(image_in(i, j)) * (-1)^(i+j);
    end
end
% 对填充后的图像进行傅里叶变换
F1 = fft2(fp);

% 生成Butterworth滤波函数，中心在(m+1,n+1)
Bw = zeros(P, Q);
a = D0^(2 * N);
for u = 1 : P
    for v = 1 : Q
        temp = (u-(m+1.0))^2 + (v-(n+1.0))^2;
        Bw(u, v) = 1 / (1 + (temp^N) / a);
    end
end

%进行滤波
G = F1 .* Bw;

% 反傅里叶变换
gp = ifft2(G);

% 处理得到的图像
image_out = zeros(m, n, 'uint8');
gp = real(gp);
g = zeros(m, n);
for i = 1 : m
    for j = 1 : n
        g(i, j) = gp(i, j) * (-1)^(i+j);
        
    end
end
mmax = max(g(:));
mmin = min(g(:));
range = mmax-mmin;
for i = 1 : m
    for j = 1 : n
        image_out(i,j) = uint8(255 * (g(i, j)-mmin) / range);
    end
end
end
```
测试BLPF的阶数为2,截止频率分别为10,40,80,150,450
```
clear all;
close all;
clc;

image1 = imread('2.jpg');

image2 = Bfilter(image1, 10, 2);
image3 = Bfilter(image1, 40, 2);
image4 = Bfilter(image1, 80, 2);
image5 = Bfilter(image1, 150, 2);
image6 = Bfilter(image1, 450, 2);

% 显示图像
subplot(2,3,1), imshow(image1), title('原图像');
subplot(2,3,2), imshow(image2), title('D0 = 10, n = 2');
subplot(2,3,3), imshow(image3), title('D0 = 40, n = 2');
subplot(2,3,4), imshow(image4), title('D0 = 80, n = 2');
subplot(2,3,5), imshow(image5), title('D0 = 150, n = 2');
subplot(2,3,6), imshow(image6), title('D0 = 450, n = 2');
```
滤波结果如下:
![](https://i.loli.net/2019/07/20/5d32d02f43bc610556.jpg)
分析结果:    
1. 模糊的平滑过渡是截止频率增大的函数
2. 滤波后输出三副连续的色图，原因是rgb图像的分三次呈现    
   一副彩图是由三色组成,红绿蓝三色，图像读取到matlab后，有三个参数m × n × 3, 代表的是三色叠加，处理之后的图将三色展开分别呈现了，所以才会出现三副连续的色图    
![](https://i.loli.net/2019/07/20/5d32d80a09a9124012.jpg)

换成彩色图可以明显看到
![](https://i.loli.net/2019/07/20/5d32d80a3073491464.jpg)



