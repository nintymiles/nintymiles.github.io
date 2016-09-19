---
layout: post
title:  Learning Notes - Swift Speical loop syntax
---
## 基于 enumberate 方法的 for－in 语法

Let’s use the for-in statement in with the enumerate method:

```
let numbers = [“zero”, “one”, “two”, “three”, “four”] 
for (i, numberString) in numbers.enumerate() {

print(“Number at index \(i) is \(numberString)”) } 
// Number at index 0 is zero // Number at index 1 is one // etc….
``` 

This is much clearer, and you would use it when you require an array element and its accompanying **index**. You can grab the index of the loop and the item being iterated over! 

## Switching it up: Switch 语句
check **multiple values at once**（ 一次检验多个值 ）. This is similar to using the or operator (||) in if else statements. Here’s how you do it:

```
var num = 5 
switch num { 
	case 2,3,4:print(“It’s two”) // is it 2 or 3 or 4? 
	case 5,6:print(“It’s five”) // is it 5 or 6? default:print(“It’s something else”) }
```

check within ranges（case 语句中使用 ranges）. The following example determines whether a number is something between 2 and 6:

```
var num = 5 
switch num { 
// including 2,3,4,5,6 
case 2…6:print(“num is between 2 and 6”) 
default:print(“None of the above”) }
```

use tuples in switch statements（switch 语句中使用一个活着多个 tuple）. You can use the underscore character (_) to tell Swift to “match everything.” You can also check for ranges in tuples. Here’s how you could match a geographic location:

```
var geo = (2,4) 
switch geo { 
//(anything, 5) 
case (_,5):print(“It’s (Something,5)”) 
case (5,_):print(“It’s (5,Something)”) 
case (1…3,_):print(“It’s (1 or 2 or 3, Something)”) 
case (1…3,3…6):print(“This would have matched but Swift already found a match”) 
default:print(“It’s something else”) 
}
```

（Switch 语句中的值绑定）Remember the **value binding** example from earlier? You can use this same idea in switch statements. Sometimes it’s necessary to grab values from the tuple. You can even add in a where statement to make sure you get exactly what you want. Here is the kitchen-sink example of switch statements:

```
var geo = (2,4) 
switch geo { 
case (_,5):print(“It’s (Something,5)”) case (5,_):print(“It’s (5,Something)”) 
case (1…3,let x):print(“It’s (1 or 2 or 3, \(x))”) 
case let (x,y):print(“No match here for \(x) \(y)”) 
case let (x,y) where y == 4:print(“Not gonna make it down here either for \(x) \(y)”) 
default:print(“It’s something else”) 
}
```


