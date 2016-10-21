---
layout: post
title:  Masonry的两个关注点
---
##Masonry的几个关注点
1. `make.size.equalTo(self.view)`和`make.size.mas_equalTo(CGSizeMake(20,20))` 中的关键区别，`equalTo`后面跟的值为***对象***和`mas_equalTo`后面跟的是**数值及相关结构**。
2.下面代码的等价性

```
   make.size.mas_equalTo(CGSizeMake(300, 300));  //设置宽高
   make.size.equalTo(@300);    //设置size

```


