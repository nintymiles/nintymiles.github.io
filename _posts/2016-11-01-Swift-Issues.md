---
layout: post
title: Swift Issues - Swift Array/Dictionary Definition,as?/as! (force)coverter,AnyObject/Any,Using cocoapods to import swift library
---
## How to use import SnapKit using CocoaPods?
An example to integrate SnapKit into your Xcode project using CocoaPods, specify it in your Podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '10.0'
use_frameworks!  # for swift library to be used as embed_frameworks 

target '<Your Target Name>' do
    pod 'SnapKit', '~> 3.0.2'
end  
```

## How to create an array or a dictionary
To create an empty array or dictionary, use the initializer syntax.

```
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

If type information **can be inferred**, you can write an empty array as [] and an empty dictionary as [:]—for example, when you set a new value for a variable or pass an argument to a function.

```
shoppingList = []
occupations = [:]
```

声明数组时选择的类型造成的数据使用方式的区别（给table view cell赋值）

```
//property
var tableData : [[String:String]]?   //注意具体数组类型的声明写法

...
//cell value fill
cell?.imageView?.image = UIImage(named: itemData["icon"]!)
cell?.textLabel?.text=itemData["title"]
cell?.detailTextLabel?.text=itemData["amount"]
```

```
//property
var tableData : [AnyObject]?   //任意类型数组

...
//cell value fill
cell?.imageView?.image = UIImage(named: itemData["icon"] as! String)  //注意强制转型操作符  as！
cell?.textLabel?.text=itemData["title"] as? String 
cell?.detailTextLabel?.text=itemData["amount"] as? String
```

## as?/as! 的区别  
***Type cast operator (as? or as!)***

Because downcasting can fail, the type cast operator comes in two different forms. **The conditional form, as?**, returns an optional value of the type you are trying to downcast to. **The forced form, as!**, attempts the downcast and force-unwraps the result as a single compound action.

Use the conditional form of the type cast operator (as?) when **you are not sure if the downcast will succeed**. This form of the operator will always return an optional value, and the value will be nil if the downcast was not possible. This enables you to check for a successful downcast.

Use the forced form of the type cast operator (as!) only when **you are sure that the downcast will always succeed**. This form of the operator ***will trigger a runtime error*** if you try to downcast to an incorrect class type.

## Any and AnyObject
Swift provides two special type aliases for working with non-specific types:

- **AnyObject** can represent an instance of any class type.
- **Any** can represent an instance of any type at all, including function types.

