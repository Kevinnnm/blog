---
title: Day14
date: 2019-07-28 01:51:51
tags: 实习
cover: https://i.loli.net/2019/07/17/5d2e73bb14bd344648.png
---

# High-quality-ellipse-detection

高精度椭圆检测    
参考[High-quality-ellipse-detection](https://github.com/AlanLuSun/High-quality-ellipse-detection)

关于椭圆检测的部分，实在是看不懂，我只做了得到椭圆参数绘制椭圆的部分

## OpenCV椭圆绘制

### ellipse函数

语法

```
void cvEllipse( CvArr* img, CvPoint center, CvSize axes, double angle,
                double start_angle, double end_angle, CvScalar color,
                int thickness=1, int line_type=8, int shift=0 );
```
参数:    
img:图像。    
center:椭圆圆心坐标。    
axes:轴的长度。    
angle:偏转的角度。    
start_angle:圆弧起始角的角度。    
end_angle:圆弧终结角的角度。    
color:线条的颜色。    
thickness:线条的粗细程度。    
line_type:线条的类型,见CVLINE的描述。    
shift:圆心坐标点和数轴的精度。    

C++代码实现
```C++
#include<opencv2/opencv.hpp>#include<opencv2/core/core.hpp> // 核心组件
#include<opencv2/highgui/highgui.hpp>  // GUI
#include<opencv2/imgproc/imgproc.hpp>  // 图像处理
using namespace cv;
using namespace std;
// 定义存储椭圆参数的数据结构
struct Ellipse {
	int x0, y0, a, b;
	double alpha;
};
int drawEllipse(Ellipse ellipses_para, Mat im);
int drawEllipse(Ellipse ellipses_para, Mat im) {
// Draw Ellipse after detection
//x0 - x coordinate of the center of the ellipse
//y0 - y coordinate of the center of the ellipse
//a - length of semimajor axis
//b - length of semiminor axis
//alpha - angle of orientation of semimajor axis
	if (im.empty()) {
		printf("imread error!");
		return -1;
	}
	int x0, y0, a, b;
	double alpha;
	int thickness = 3;
	int lineType = 8;
	x0 = ellipses_para.x0;
	y0 = ellipses_para.y0;
	a = ellipses_para.a;
	b = ellipses_para.b;
	alpha = ellipses_para.alpha;
	ellipse(im, Point(x0, y0), Size(a, b), alpha, 0, 360, Scalar(255, 255, 0), thickness, lineType);
	imshow("原图", im);
	waitKey();}
int main() {
	Ellipse ellipses_para;
	Mat im = imread("11.bmp");
	/*test
        ellipses_para.x0 = 100;
	ellipses_para.y0 = 100;
	ellipses_para.a = 90;
	ellipses_para.b = 60;
	ellipses_para.alpha = 80.0;*/
	drawEllipse(ellipses_para, im);
	return 0;
}

```


