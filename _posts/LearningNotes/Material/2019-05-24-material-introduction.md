---
layout: post
title:  Learning Notes - Foundation of Computer Graphics 
---

# Material Introduction

## fragment shader的真正职责、fragment可访问哪些（主要是两类）数据，以及这些数据所承载的内容。fragment shader的具体工作方式描述。本章和下一章所设计的都是fragment shader相关的两类计算
### 关键概念
1. fragment shader的工作职责？ 在pipeline中相当于图像一个pixel所对应的三角形的点上，决定其色彩。
2. fragment shader可访问哪两类数据？ 1、被插了值的varying variables 2、uniform variable方式由程序所传入的数据
3. uniform variable可以承载的数据种类？  1、light source 2、matrices
4. varying variable可承载的数据种类？ 1、point position 2、point normal 3、parameters of material 
5. fragment shader的具体工作？ 接收这两种类型的数据，并模拟光线从这些材料反弹开的情形，最终产生一个图像颜色。
6. fragment shader的两类计算？ 1、光方程式模拟计算 2、纹理计算

### 基础知识点
1. correspond to 相当于
2. bounce off 反弹开

## fragment shader在使得渲染更细致和真实方面有着丰富的内容，需要学习比本书所涉及到的更多相关主题。列举了一些参考书目
### 概念
1. fragment shader在CG渲染出更真实细致的场景方面有着丰富巨量的主题
2. fragment shader是CG渲染的**核心主题**，需要学习更多的内容

### 基础知识点
1. more detailed and realistic
2. mostly

