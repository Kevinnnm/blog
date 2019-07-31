---
title: Day07
date: 2019-07-21 14:47:14
tags: 实习
cover: https://i.loli.net/2019/07/17/5d2e73bb14bd344648.png
---

## 使用频率域滤波器锐化图像
---
### 高通滤波器
图像的锐化可以在频率与通过高通滤波器来实现
一个高通滤波器可以由一个低通滤波器来实现:
$$H_{HP}(u,v)=1-H_{LP}(u,v)$$
被低通滤波器衰减的频率可以通过高通滤波器
#### 理想高通滤波器
二维理想高通滤波器可以定义为
$$ H(u,v)=\begin{cases}
1,\quad D(u,v)\leq D_0\\
0,\quad D(u,v)>D_0
\end{cases}
$$
![](https://i.loli.net/2019/07/21/5d341bf1c410443073.jpg)
#### 布特沃斯高通滤波器
截止频率为$D_0$的n阶布特沃斯高通滤波器(BHPF)的定义为:
$$ H(u,v)=\frac{1}{1+[D_0/D(u,v)]^{2n}}$$
![](https://i.loli.net/2019/07/21/5d341bf1c376281237.jpg)
#### 高斯高通滤波器
截止频率处在距频率矩形中心距离为$D_0$的高斯高通滤波器(GHPF)的传递函数如下:
$$H(u,v)=1-e^{-D^2(u,v)/2D_0^2}$$
![](https://i.loli.net/2019/07/21/5d341bf1c3f7954220.jpg)
#### 在MATLAB中使用高通滤波器来锐化图像
使用高通滤波器来锐化图像，与平滑图像类似，只是将低通滤波器换成了高通滤波器，具体步骤不再赘述
```
f = imread('1.jpg');
f = rgb2gray(f);
[f, revertclass] = tofloat(f);
PQ = paddedsize(size(f));
[U, V] = dftuv(PQ(1), PQ(2));
D = hypot(U, V);
D0 = 0.05*PQ(1);
F = fft2(f, PQ(1), PQ(2));
H = hpfilter('gaussian',PQ(1), PQ(2), D0);
g = dftfilt(f, H);
g = revertclass(g);
figure(1)
subplot(2,2,1);
imshow(f,[]);
title('原图像')
subplot(2,2,2);
imshow(fftshift(H));
title('高斯高通滤波器');
subplot(2,2,3);
imshow(log(1 + abs(fftshift(F))), [])
title('滤波后图像谱');
subplot(2,2,4);
imshow(g);
title('滤波后图像');
```
同样这里需要的是高通滤波函数hpfilter()
```
function [H] = hpfilter(type,M,N,D0,n)
%HPFILTER Computes freq. domain highpass filters
%		THIS IS NOT A STANDARD MATLAB FUNCTION
%		H = hpfilter (type,M,N,D0,n) creates the
%		transfer function of a highpass filter, H, of
%		the specified type and size MxN. Possible
%		values for type, D0, and n are:
%
%		'ideal'				Ideal highpass filter with
%						cutoff frequency D0. If
%						supplied, n is ignored.
%		'btw'				Butterworth highpass filter
%						of order n, and cutoff D0.
%		'gaussn'			Gaussian highpass filter with
%						cutoff (standard deviation)D0.
%						If supplied, n is ignored.
%		M and N should be even numbers for DFT
%		filtering.
%
%		Class support: double, uint8, uint16
%		The output is of class double

%       The transfer function Hhp of a highpass filter
%       is 1 - Hlp, where Hlp is the transfer function of
%       the corresponding lowpass filter.  Thus, we can
%       use function lpfilter to generate highpass filters

%       If filter is btw, make sure that n is provided
%       Otherwise, pass n=1 as an arbitrary value to
%       prevent error message

if nargin == 4
    n = 1; %default value of n
end

Hlp = lpfilter(type,M,N,D0,n);
H = 1 - Hlp;

%       End of function
```
锐化结果:
1. IHPF
![](https://i.loli.net/2019/07/21/5d341eae3a6b123891.jpg)
2. BHPF
![](https://i.loli.net/2019/07/21/5d341bf1f13e453488.jpg)
3. GHPF
![](https://i.loli.net/2019/07/21/5d341eae4fa8d96779.jpg)


