---
layout: post
title:  Learning Notes - Foundation of Computer Graphics 
---

# 两种图形模式

## 连续图像函数定义，域及值区间说明
### 关键概念
1. continuous image 是什么？ 被定义为双变量函数I(x,y),函数变量域为实数值化的2D窗口坐标，具体为[-.5..W-.5]X[-.5..H-.5],函数值范围为color space，一般使用RGB色彩空间

### 基础知识
1. bivariate

## 具体（电子/数码）图像定义，相关值空间说明。以及与continuous image的关联方式
### 关键概念
1. discrete image 是什么？ 被定义为一个颜色值的2维数组I[i][j]。
2. discrete image 和 continuous image的关联方式？ 每对discrete image的索引和continuous image的一个window coordinate[x,y]关联上，只不过这个window coordinate[x,y]恰好为整数坐标。每个元素值都为一个3元组实数，可理解为颜色空间的一个值，颜色空间一般使用RGB色彩空间


### 基础知识
1. discrete image的pixel概念：数组中的每个元素条目值被称为一个**pixel**
2. discrete image数组的说明：1、数组的尺寸为WxH，而数组的每个维度的索引分别为i = [0..W-1],j = [0..H-1] 2、数组中的元素为颜色值，为颜色空间中的一个值
3. RGB色彩空间的R、G、B坐标都是数值形式表示

## 在OpenGL各阶段中，两种图形概念的使用
### 关键概念
1. OpenGL中如何形成一个continuous image？由于投射projection过程，将场景中的shaded三角形映射为continuous image中上色的三角形
2. 使用discrete image的场景？ 当现实在screen上，或者保存在image file中时，需要discrete image格式
3. OpenGL的工作机理使得两种图形之间的有效转换很重要

### 基础知识
1. OpenGL中3D场景是通过着色的三角形被表达的
2. **under** 

# 失真问题的具体描述

## 点采样概念及公式表达
### 关键概念
1. 点采样point sampling？ 从continuous image中整数域地址（i，j）直接函数值作为I[i][j]像素
2. point sampling的公式表达？I[i][j] <- I(x,y)
3. 作用评价？最简单直接的从continuous image到discrete image的转换

## 采用point sampling方式所造成的多种失真类型描述及原因分析
### 关键概念
1. Jaggies型失真描述？ 一种情形是在由黑白三角形组成的场景中，由于点采样导致在图像中三角形边缘出现阶梯状的图样，称为锯齿形的失真jaggies
2. Crawling Jaggies动画失真？在动画中，一些像素突然从白变为黑，三角形边缘图像模式会显示为爬行的锯齿crawling jaggies
3. moire型失真？在非常小的三角形上所产生的糟糕失真，例子情形，比如远处一群斑马，其中一只逐渐远离，直至在图像中只显示为一个像素，点采样时如果落在白色上则显示为白色，落在黑色上则显示为黑色，类似情形导致pixel colors在图像上组成“云纹”图案
4. Flickering型失真？ 在动画中，上面moire像素会呈现黑白不断变换的情形，被称作“flickering”。 **动画型失真都是从像素颜色不稳定变化的角度来考虑**
5. 上述失真类型所产生的原因分析？ 由点采样方式所导致的问题在原因上有其相似型，被统称为aliasing。 所产生的原因都是由于在小区域内continuous image包含太多的细节信息所导致。
6. 具体失真类型的原因详析？（斑马例子中，很小的像素范围，continuous可以包含整群斑马，锯齿例子中，三角形在continuous image中有完全锋利的边缘且过度立即完成而不是渐进式的）

### 基础知识
1. 多种失真的直接原因，由点采样方式所造成的多种失真统称为 aliasing-锯齿形失真
2. jaggy
3. moire 【mwa：】
4. take on 呈现
5. a whole log of 一大堆/许许多多的



