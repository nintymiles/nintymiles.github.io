---
layout: post
title: Swift 3 class支持类计算属性不支持类存储属性
---

## `Swift 3`class支持类计算属性不支持类存储属性


```
class MyClass {
	 //使用class关键字，则子类可以重载此类计算属性。若用static，则子类无法重载
    class var myClassVar:String{ 
        return "7777"
    }
}

let aa=MyClass.myClassVar   //aa=7777

class MySubClass:MyClass{
	 //swift中重载时需明确标注。
    override class var myClassVar:String{
        return "7778"
    }
}

let bb=MySubClass.myClassVar //bb=7778
```



