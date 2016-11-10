## 如何从 `instance methods` 内部更改 `value types` 的值
默认情况下，值类型的属性不能被内部的实例方法更改。如果需要更改，需要选择使用方法的 `Mutating` 行为。

Structures and enumerations are value types. By default, the properties of a value type cannot be modified from within its instance methods.

However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to mutating behavior for that method. The method can then mutate (that is, change) its properties from within the method, and **any changes that it makes are written back to the original structure when the method ends**.

```
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// Prints "The point is now at (3.0, 4.0)
```

## Assigning to self Within a Mutating Method 
Mutating methods can assign **an entirely new instance** to the implicit self property.

```
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

通过 `Mutating methods` 可以隐含的给自身赋一个全新的值通过`self`关键字。


