---
title: 'OpenCV入坑指南:环境搭建篇'
date: 2019-07-25 21:12:11
tags: OpenCV
cover: https://i.loli.net/2019/07/26/5d3a7fcc2bbad35969.jpg
---
# OpenCV
## 什么是OpenCV
[OpenCv](https://opencv.org/about/)是一个基于BSD许可（开源）发行的跨平台计算机视觉库，可以运行在Linux、Windows、Android和Mac OS操作系统上。它轻量级而且高效——由一系列 C 函数和少量 C++ 类构成，同时提供了Python、Ruby、MATLAB等语言的接口，实现了图像处理和计算机视觉方面的很多通用算法。    
OpenCV用C++语言编写，它的主要接口也是C++语言，但是依然保留了大量的C语言接口。该库也有大量的Python、Java and MATLAB/OCTAVE（版本2.5）的接口。这些语言的API接口函数可以通过在线文档获得。如今也提供对于C#、Ch、Ruby,GO的支持。
## OpenCV可以来做什么
使用OpenCV，你几乎可以做任何你能够想到的计算机视觉任务。    
1. 内置数据结构和输入/输出(In-build data structures and input/output)
2. 图像处理操作(Image processing operations)

3. 构建图形用户界面(Build GUI)

4. 视频分析(Video analysis)

5. 3D重建(3D reconstruction)

6. 特征提取(Feature extraction)

7. 目标检测(Object detection)

8. 机器学习(Machine learning)

9. 计算摄影(Computational photography)

10. 形状分析(Shape analysis)

11. 光流算法(Optical flow algorithms)

12. 人脸和目标识别(Face and object recognition)

13. 表面匹配(Surface matching)

14. 文本检测和识别(Text detection and recognition)

---

# Microsoft Visual Studio

宇宙最强IDE(逃),不多说了

---

# OpenCV + VS学习(装×)环境搭建
工欲善其事，必先利其器。当你准备好入坑OpenCV时，你首先要把学习环境搭建起来，光是这一关不知道劝退了多少人，我在搭建的过程中也是踩了很多坑，也遇到了各种各样刁钻的问题，现在总结一下，给自己踩过的雷做一下记录，也希望能够帮到即将入坑的你们。

## 版本选择
为什么把这个放到第一个呢？因为这是我遇到并纠结了一天的问题！我在网上查找教程的时候很多人都没有标注这个问题，当然也是自己蠢，在所有的配置都配置完成之后，还是不能正常跑程序，又重新安装重新配置，反复好几次，都快怀疑人生了，最后才发现是版本号不对。废话不多说，下面是OpenCV版本和VS版本的对应表，选择的时候一定要擦亮眼睛。我选择的环境是VS2017+OpenCV3.4.4    

Visual Studio 版本 | VC 版本
:---: | :---:
VS 6 | vc6
VS 2003 | vc7
VS 2005 | vc8
VS 2008 | vc9
VS 2010 | vc10
VS 2013 | vc12
VS 2015 | vc14
VS 2017 | vc15

OpenCV对VC版本的支持情况(不全)

OpenCV 2.4.10 | vc10、vc11、vc12
:---: | :---:
OpenCV 2.4.13 | vc11、vc12
OpenCV 3.4.0 | vc14、vc15
OpenCv 3.4.1 | vc14、vc15

## VS2017安装
我的VS安装的时间太久了，网上教程一大堆，给你们挑一篇吧，这里就不再多说了

## OpenCV3.4.4下载与安装

### 下载
1. 官网下载
OpenCV官网给我们提供了下载，不过下载速度嘛～自求多福    
[OpenCV下载](https://opencv.org/releases/),里面有各个版本可以选择
2. OpenCV下载驿站    
[OpenCV各版本汇总下载](https://blog.csdn.net/oMoDao1/article/details/80276834)    
感谢这位大哥的总结

### 安装
找一个你能记住名字的路径安装进去就OK了，一定要记住这个路径，非常重要
![](https://i.loli.net/2019/07/27/5d3c0e0f7a6db16095.png)
## 系统环境变量配置

1. 找到此电脑
2. 依次找到 属性->高级->环境变量,找到系统变量里的Path，双击进去编辑，如图所示，添加的路径 **"D:\OpenCV\opencv\bulid\x64\vc15\bin"**,把"D:\OpenCV\"替换成你的安装路径即可

![](https://i.loli.net/2019/07/27/5d3c0e0f9cd5286868.png)
![](https://i.loli.net/2019/07/27/5d3c0e0fa09f948672.png)
![](https://i.loli.net/2019/07/27/5d3c0e0fa634780591.png)
![](https://i.loli.net/2019/07/27/5d3c0e0fa37ce25025.png)

到这就完成了系统环境的配置了

## 一些文件的配置

这一步的目的是为了解决以后可能会出现的关于缺少.dll的问题

将 **"D:\OpenCV\opencv\bulid\x64\vc15\bin"** 里面的三个 **.dll** 文件复制到 **C:\Windows“** 目录下的 **System32**和 **SysWOW64**目录下

![](https://i.loli.net/2019/07/28/5d3d5857adbe920715.png)
![](https://i.loli.net/2019/07/28/5d3d5857bd82f79652.png)

## VS2017配置

1. 新建一个空项目
2. 进入属性管理器  视图->其他窗口->属性管理器
![](https://i.loli.net/2019/07/27/5d3c156f6359d16629.png)
3. 选择Debug | x64 的 Microsoft.Cpp.x64.user
![8.png](https://i.loli.net/2019/07/27/5d3c156ecb8ad31557.png)
4. 选择VC++目录，对包含目录和库目录进行配置    
   ![](https://i.loli.net/2019/07/27/5d3c156f2e22b74192.png)
   - 在包含目录里添加 **D:\OpenCV\opencv\bulid\include**,**D:\OpenCV\opencv\bulid\include\opencv**,**D:\OpenCV\opencv\bulid\include\opencv2**    
![](https://i.loli.net/2019/07/27/5d3c156f2132f35424.png)
   - 在库目录里添加 **D:\OpenCV\opencv\bulid\x64\vc15\lib**   
   ![](https://i.loli.net/2019/07/27/5d3c156f492f430904.png)
5. 选择 链接器->输入->附加依赖项,    
   ![12.png](https://i.loli.net/2019/07/27/5d3c156f13aac74119.png)
   在里面添加 **opencv_world344d.lib**，这个lib文件根据你的OpenCV版本号灵活变动
   ![](https://i.loli.net/2019/07/27/5d3c156f0828190561.png)
6. 点击应用，确定即可
到这里，OpenCV的所有配置工作已经完成了，要注意的是解决方案那一栏要换成x64(因为我们一直在配置的就是x64)
![](https://i.loli.net/2019/07/27/5d3c156ee8b6668598.png)
## 测试
这是一段读取并显示本地图片的代码，测试一下你的OpenCV环境是否配置好
```
#include <iostream>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace cv;

int main()
{
	Mat img = imread("1.bmp");
	namedWindow("图片");
	imshow("图片", img);
	waitKey(6000);
	return 0;
}
```
运行结果如下：
![](https://i.loli.net/2019/07/27/5d3c15857414f57549.png)

## 需要注意的问题
1. 导入包的时候不报错，但运行程序时出现一下错误
![15.png](https://i.loli.net/2019/07/27/5d3c156f55e6b74341.png)
![16.png](https://i.loli.net/2019/07/27/5d3c156f39a4e29681.png)
出现这个问题，目前已知的有两个原因    
  - imread函数读不到图片，即你的图片路径写的有问题    
    这里给出两种基本的路径书写形式    
    1. 绝对路径    
    Mat img = imread("D:\\Pictures\\1.bmp");    
    一定要注意这里是双斜杠
    2. 图片路径    
    Mat img = imread("1.bmp");    
    使用这个的前提是图片的位置在你的工程目录下
  - 链接器的附加依赖项配置的有问题    
    在Debug模式下，附加依赖项添加的是 **opencv_world344d.lib**   
    在Release模式下，附加依赖项添加的是 **opencv_world344.lib**    
    有些教程是两个都添加，这是不对的，一定要注意。
    
---
好了，教程到此结束，    
接下来你就可以放心地去玩耍了，Enjoy Your OpenCV!
