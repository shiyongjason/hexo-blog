---
title: CSS3新特性
date: 2023-01-11 14:58:08
tags:
---
### [](#CSS3的新特性大致分为以下六类 "CSS3的新特性大致分为以下六类")CSS3的新特性大致分为以下六类

# [](#1-CSS3选择器 "1.CSS3选择器")1.CSS3选择器

基本选择器，属性选择器，伪类选择器，nth选择器

# [](#2-CSS3边框与圆角 "2.CSS3边框与圆角")2.CSS3边框与圆角

1.CSS3圆角border-radius

```auto
定义：可以为元素添加圆角边框（块元素，行内块元素，行内元素）
```

属性：  

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br></pre></td><td class="code"><pre><span class="line">border-top-left-radius 左上角</span><br><span class="line">border-top-right-radius 右上角</span><br><span class="line">border-bottom-right-radius 右下角</span><br><span class="line">border-bottom-left-radius 左下角</span><br></pre></td></tr></tbody></table>

复合属性：border-radius：  
属性值

四个值：左上角 右上角 右下角 左下角  
三个值：左上角 右上角和左下角 右下角  
两个值：左上角和右下角 右上角和左下角  
一个值：4个角都生效

border-radius中的属性值由两个参数值构成: value1 / value2，值之间用/分隔，value1代表圆角的水平半径，value2代表圆角的垂直半径。

2.盒阴影box-shadow

```auto
定义：可以控制一个或多个下拉阴影的框

语法：box-shadow: 水平方向的偏移量 垂直方向的偏移量 模糊程度 扩展程度 颜色 是否具有内阴影
```

# [](#3-CSS3背景与渐变 "3.CSS3背景与渐变")3.CSS3背景与渐变

1.CSS3背景

background-image  
语法：  
backgroundimage:url(‘1.jpg),url(‘2.jpg’)  
使用逗号把图片分开  
注意：元素引入多个背景图片，前面图片会覆盖后面的图片  
background-cilp  
定义：指定背景的绘制区域（裁剪）  
语法：  
background-cilp：border-box / padding-box / content-box  
属性介绍：  
border-box：背景被裁剪到边框盒（从边框开始绘制背景图片）—默认  
padding-box：背景被裁剪到内边距框（从内边距开始绘制背景图片）  
content-box：背景被裁剪到内容框  
background-origin  
定义：设置背景图像的原始起始位置  
语法：  
background-origin：border-box / padding-box / content-box(背景图片坐标原点与这三个有关系)  
属性的介绍：  
border-box：相对于边框来定位  
padding-box：相对于内边距来定位  
content-box：相对于内容框来定位  
另外有一个需要了解  
background-position:定义背景图片的位置，水平与垂直方向上面的偏移量(参考点)  
background-repeat  
定义：设置是否及如何重复背景图像，默认地，背景图像在水平和垂直方向上重复。

属性值：  
repeat 默认。背景图像将在垂直方向和水平方向重复。  
repeat-x 背景图像将在水平方向重复。  
repeat-y 背景图像将在垂直方向重复。  
no-repeat 背景图像将仅显示一次。  
inherit 规定应该从父元素继承 background-repeat 属性的设置

background-size  
定义：指定背景图像的大小  
语法：  
background-size：number / % / cover / contain  
属性介绍：  
number: 宽度 高度（如果只写一个数值，第二个数值默认auto）  
百分比： 0% - 100% 之间的任何值，此时的百分比参照于元素div的大小  
cover：将背景图片等比缩放以填满整个容器（最远边），如果高度达到一定比例100%，宽度多出的会溢出，但是，具体那部分溢出取决于定位  
contain：将背景图片等比缩放至某一边紧贴容器边缘为止（最近边），如果图片高度比较小，高度就会有空白区域出现  
复合属性background  
定义：可以在一个声明中设置所有的背景属性  
语法：  
background：color position size repeat origin clip attachment image; background: #abc center 50% no-repeat content-box content-box fixed url(‘1.jpg’) ,url(‘2.jpg’)…

2.CSS3渐变

定义：可以在两个或者多个指定颜色之间显示平移的过渡

线性渐变  
定义：是沿着一根轴线改变颜色，从起点到终点进行顺序渐变（从一边拉向另一边）  
语法：background:linear-gradient(方向，开始颜色，结束颜色)

方向介绍：

1.方向从上到下（默认）  
background: linear-gradient(red,blue);  
2.方向从左到右  
background: linear-gradient(to right,red,blue);  
3.对角  
background: linear-gradient(to right bottom,red,blue);  
4.角度(单位deg)  
background: linear-gradient(角度,red,blue);  
角度说明：0deg 将创建一个从下到上的渐变，90deg将创建一个从左到右的渐变

颜色结点：默认每个颜色均匀分布

background: linear-gradient(red 10%,blue 20%,green 30%,yellow 40%);  
从0%到10%，为红色，从10%到20%为红色到蓝色的渐变，从20%到30%为蓝色到绿色的渐变，从30%到40%，为绿色到黄色的渐变,从40%到100%为黄色  
background: linear-gradient(red 10%,blue);  
从0%到10%，为红色，从10%到100%为红色到蓝色的渐变  
最后如果不写具体数值，默认到100%  
background: linear-gradient(red,blue 30%);  
从0%到30%，为红色到蓝色的渐变  
如果第一个不写，默认数值是 0%  
background:lineargradient(rgba(255,0,0,0),rgba(255,0,0,1));  
由透明色变为不透明色

重复渐变

示例：background: repeating-linear-gradient(90deg,red 0%,blue 20%);或者 background: repeating-linear-gradient(90deg,red 0%,blue 10%,red 20%);  
注意：把元素的整体宽度看成100%

径向渐变  
定义：从起点到终点，颜色从内向外进行圆形渐变  
语法：background:radial-gradient(形状尺寸，开始颜色，结束颜色)  
形状分类：  
circle — 圆形  
ellipse — 椭圆形  
注意：当元素的高和宽一样时，参数无论设置哪个，都是圆形

尺寸大小：  
closest-side最近边  
background: radial-gradient(closest-side circle,red , blue);  
farthest-side 最远边  
background: radial-gradient(farthest-side circle,red , blue);  
closest-corner最近角  
background: radial-gradient(closest-corner circle,red , blue);  
farthest-corner最远角  
background: radial-gradient(farthest-corner circle,red , blue);

颜色结点：  
例：  
background:radial-gradient(circle,red 50% ,blue 70%);  
注意：此时的百分比,指的是圆心到元素最远端的距离（角度）

重复渐变 ：  
示例： background: repeating-radial-gradient(red 0%,blue 20%);  
background: repeating-radial-gradient(red 0%,blue 10%,red 20%);

# [](#4-CSS3动画 "4.CSS3动画")4.CSS3动画