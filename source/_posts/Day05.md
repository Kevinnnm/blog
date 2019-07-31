---
title: Day05
date: 2019-07-19 14:03:12
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
过渡点成为**截止频率**
### 在MATLAB中DFT滤波的步骤:
1. 用函数`tofloat`把输入图像转换成浮点图像
```
>> [f, revertclass] = tofloat(f);
```
2. 用函数`paddedsize`来获得填充参数
```
>> PQ = paddedsize(size(f));
```
3. 得到有填充的Fourier变换
```
>> F = fft2(f,PQ(1), PQ(2));
```
4. 生成大小为PQ(1)×PQ(2)的滤波函数H,函数类型要满足如下图所示,
![](https://i.loli.net/2019/07/19/5d318dcf343b438405.jpg)    
如果是类似这样的
![](https://i.loli.net/2019/07/19/5d318dcf4915275796.jpg)    
```
>> H = lpfilter('gaussian',PQ(1),PQ(2),2*sig);
```
在使用滤波器之前，要先`H = fftshift(H)`
5. 用滤波器乘以FFT变换
```
>> G = H .* F;
```
6. 获得G的逆Fourier变换
```
>> g = ifft2(G);
```
7. 修剪左上部矩形为原始大小
```
>> g = g(1:size(f, 1), 1:size(f, 2));
```
8. 把滤波后的图像变换为输入图像的类
```
>> g = revertclass(g);
```

完整代码
```
f = imread('1.jpg');

f = rgb2gray(f);

%未填充的滤波

[M,N] = size(f);

[f, revertclass] = tofloat(f);

F = fft2(f);

sig = 10;

H = lpfilter('gaussian', M, N, sig);

G = H.*F;

g = ifft2(G);

g = revertclass(g);

figure(1);

subplot(1,2,1);

imshow(g)

title('未填充的滤波');

%已填充的滤波

PQ = paddedsize(size(f));

Fp = fft2(f,PQ(1),PQ(2));

Hp = lpfilter('gaussian',PQ(1),PQ(2),2*sig);

Gp = Hp.*Fp;

gp = ifft2(Gp);

gpc = gp(1:size(f,1),1:size(f,2));

gpc = revertclass(gpc);

subplot(1,2,2);

imshow(gpc);

title('已填充的滤波');
```
这里展示了不填充滤波和填充滤波的两种情况，结果
![](https://i.loli.net/2019/07/19/5d318dcf1e88071166.jpg)
可以观察到未填充滤波处理后图像的垂直边缘未模糊

涉及到的函数    
- paddedsize函数
```
function PQ = paddedsize(AB, CD, PARAM)

if nargin == 1

PQ = 2*AB;

elseif nargin == 2 & ~ischar(CD) %如果CD不为字符串

PQ = AB + CD -1;

PQ = 2 *ceil(PQ / 2);

elseif nargin == 2 %如果CD处为字符串

m = max(AB);

P = 2^nextpow2(2*m); %取2的整数次幂

PQ = [P, P];

elseif nargin == 3

m = max([AB CD]);

P = 2^nextpow2(2*m);

PQ = [P, P];

else

error('wrong number of inputs.')

end
```
- lpfilter函数
```
function [ H, D ] = lpfilter( type,M,N,D0,n )

%LPFILTER creates the transfer function of a lowpass filter.

%   Detailed explanation goes here



%use function dftuv to set up the meshgrid arrays needed for computing

%the required distances.

[U, V] = dftuv(M,N);



%compute the distances D(U,V)

D = sqrt(U.^2 + V.^2);



%begin filter computations

switch type

    case 'ideal'

        H = double(D <= D0);

    case 'btw'

        if nargin == 4

            n = 1;

        end

        H = 1./(1+(D./D0).^(2*n));

    case 'gaussian'

        H = exp(-(D.^2)./(2*(D0^2)));

    otherwise

        error('Unkown filter type');
end
```
- dftuv函数
```
function [ U,V ] = dftuv( M, N )

%DFTUV 实现频域滤波器的网格函数

%   Detailed explanation goes here

u = 0:(M - 1);

v = 0:(N - 1);

idx = find(u > M/2); %找大于M/2的数据

u(idx) = u(idx) - M; %将大于M/2的数据减去M

idy = find(v > N/2);

v(idy) = v(idy) - N;

[V, U] = meshgrid(v, u);
```
### 总结：
1. 图像平滑之后，变得更柔和，但也会更模糊    
2. 会出现的问题:图像的边缘部分往往也处于高频，会被滤除
