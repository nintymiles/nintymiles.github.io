---
layout: post
title:  Learning Notes - Foundation of Computer Graphics 
---

# Diffuse Material

## 漫射材料上光的反射特征，计算的关键量
### 基础概念
1. 漫射材料的基本光散射特征？ 从各个方向观看，材料都显示出同样的外观，计算材料的散射颜色时完全不需要光矢量的参与
2. 当材料不同部位被照射时的表现？ 当光线在材料的上方照射时，外观显得更亮；而当以略过的方式照射时，则显得更暗
3. 漫射材料入射光数量的决定因素？ 当照射在一小块、固定尺寸的材料块时，入射光的光子总量成比例于法线和光矢量的角度theta的cosine值![Screen Shot 2019-05-24 at 4.36.15 P](media/Screen%20Shot%202019-05-24%20at%204.36.15%20PM.png)

### 基础知识
1. rough
2. grazing
3. equally
4. 计算cosine值时，法线和光矢量都必须是标准norm

## 漫射的Fragment Shader实例![Screen Shot 2019-05-24 at 11.13.46 P](media/Screen%20Shot%202019-05-24%20at%2011.13.46%20PM.png)

### 基础概念
1. 标准化normalize()确保矢量具备unit norm，这是计算cosine的前提
2. max()确保在背离光源的点不产生负的色彩，确保不出现光线从材料下方照射的情形
3. diffuse值被作为因子用来调和表面固有色彩的值

### 基础知识
1. moudlate 调和

## diffuse着色器的例子可以有很多方式来扩展
### 关键概念
1. 扩展1-光本身的颜色，很容易地建模为与另一个uniform variable的加法
2. 扩展2-多光源可以很容易地添加到代码中(每个光源单独计算，最后计算intensity之和？)
3. 扩展3-环绕色的添加，可以粗暴地建模为环绕色+intensity（普通漫射值）
4. 扩展4-directional light，light vector作为uniform variable传入shader即可，其余计算相同

### 基础知识
1. 环绕光的形成？ 通常光线会在一个场景中多次反弹，导致每个表面都会有一些来自每个方向的光照射到
2. crudely
3. ovoid


