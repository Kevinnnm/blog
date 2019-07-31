---
title: Day15
date: 2019-07-28 23:49:44
tags: 实习
cover: https://i.loli.net/2019/07/17/5d2e73bb14bd344648.png
---

# High-quality-ellipse-detection

[High-quality-ellipse-detection　Github传送门](https://github.com/AlanLuSun/High-quality-ellipse-detection)

个人环境:Matlab 2019a、VS2017、Opencv3.4.4，64位Windows操作系统

## OpenCV环境配置请看这篇
[OpenCV入坑指南:环境搭建篇](https://kevinnnm.github.io/2019/07/25/OpenCV%E5%85%A5%E5%9D%91%E6%8C%87%E5%8D%97-%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E7%AF%87/)
## MatLab和C++混合编程环境配置

Matlab的安装这里不再说    
1. 在命令行里输入 `mex -setup`，选择vs就行了
2. 执行 `mex -setup C++`完成配置
![](https://i.loli.net/2019/07/29/5d3ea6561215b10575.png)
![](https://i.loli.net/2019/07/29/5d3eaa19b300f42041.png)

## 下载Github文件

download Zip即可


## 导入依赖文件

将 **D:\OpenCV\opencv\build\x64\vc15\lib**下的 **opencv_world344.lib**文件复制到你的Matlab安装路径下的 **microsoft**文件夹下,我的是 **D:\MATLAB\R2019a\extern\lib\win64\microsoft**

![](https://i.loli.net/2019/07/29/5d3ea6564f0b271051.png)
![](https://i.loli.net/2019/07/29/5d3ea6564ed8016435.png)
## 在Matlab中导入文件

## 在命令行执行以下命令

注意作者的命令为:

```
mex generateEllipseCandidates.cpp -IF:\OpenCV\opencv2.4.9\build\include -IF:\OpenCV\opencv2.4.9\build\include\opencv -IF:\OpenCV\opencv2.4.9\build\include\opencv2 -LF:\OpenCV\opencv2.4.9\build\x64\vc11\lib -IF:\Matlab\settlein\extern\include -LF:\Matlab\settlein\extern\lib\win64\microsoft -lopencv_core249 -lopencv_highgui249 -lopencv_imgproc249 -llibmwlapack.lib
```
把OpenCV和Matlab的相关文件的路径改成你的安装路径    
我这里版本号为OpenCV3.4.4,安装路径如下,
![](https://i.loli.net/2019/07/29/5d3ea6561fbd474647.png)
故修改为`D:\OpenCV\opencv\build...`,    
由于OpenCV3.4.4只有 **opencv_world344.lib** 这一个lib文件，故将 `LF:\Matlab\settlein\extern\lib\win64\microsoft -lopencv_core249 -lopencv_highgui249 -lopencv_imgproc249 -llibmwlapack.lib`　修改为 `LD:\Matlab\R2019a\extern\lib\win64\microsoft -lopencv_world344 -llibmwlapack.lib`

完整的命令为:
```
mex generateEllipseCandidates.cpp -ID:\OpenCV\opencv\build\include 
-ID:\OpenCV\opencv\build\include\opencv -ID:\OpenCV\opencv\build\include\opencv2 
-LD:\OpenCV\opencv\build\x64\vc15\lib 
-ID:\Matlab\R2019a\include -LD:\Matlab\R2019a\extern\lib\win64\microsoft -lopencv_world344 -llibmwlapack.lib

```
![](https://i.loli.net/2019/07/29/5d3ea65639bdf67747.png)
编译成功之后生成`generateEllipseCandidates.mexw64`文件
![](https://i.loli.net/2019/07/29/5d3ea6564e0c183020.png)
之后再运行`LCS_ellipse.m`
![7.png](https://i.loli.net/2019/07/29/5d3ea657cb9e659639.png)

##　报错解决办法
1. 如图
![](https://i.loli.net/2019/07/29/5d3eabf1c942443319.png)
在.cpp文件中添加
```C++
using namespace std
```
2. 如图

![](https://i.loli.net/2019/07/29/5d3eabf1d457769964.png)
缺少lib文件，检查一下是不是配置出错了
