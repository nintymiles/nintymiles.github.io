---
layout: post
title:  Learning Notes - A few of Swift Initializers
---

## Default Initializer
Swift provides a **default** initializer **for any structure or class** that provides **default** values for **all of its properties** and does **not provide** at least **one initializer** itself. 

The default initializer simply creates a new instance with all of its properties set to their default values.

## Structure's **Memberwise** Initializers (Structure额外拥有默认成员相关初始化方法）
Structure types **automatically** receive a ***memberwise*** initializer if they do not define any of their own custom initializers. 

**Unlike** a **default** initializer, the structure **receives a memberwise initializer** even if it has stored properties that **do not have default values**.

## Initializer's Local and External Parameter Names (调用init时，何时必须使用external name，何时不需要?）
As with function and method parameters, initialization parameters can have both a local name for use within the initializer’s body and an external name for use when calling the initializer.

Initializers do not have an **identifying** function name **before their parentheses** in the way that functions and methods do. Therefore, the names and types of an initializer’s parameters play a particularly important role in identifying which initializer should be called. Because of this, Swift provides an automatic external name for every parameter in an initializer if you don’t provide an external name yourself.

Note that it is not possible to call these initializers without using external parameter names. **External names must always be used in an initializer** if they are defined, and omitting them is a compile-time error


```
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}

...

let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

## Initializer Parameters Without External Names(必须特殊处理，才可以不使用External names)
If you ***do not want*** to use an external name for an initializer parameter, write an **underscore (_)** instead of an explicit external name for that parameter to override the default behavior.

```
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}
let bodyTemperature = Celsius(37.0)
```
## Optional Property types during initialization 
Properties of optional type are *automatically initialized* with a value of **nil**, indicating that the property is **deliberately(故意的)** intended to have “no value yet” during initialization. 

## Assigning Constant Properties During Initialization
***You can assign a value to a constant property at any point during initialization***, as long as it is set to a **definite**(明确的) value by the time initialization finishes. Once a constant property is assigned a value, **it can’t be further modified**.

## Failable Initializers(应对初始化可能失败设计的初始化方法机制，初始化后生成optioanl类型，以此包容初始化失败。表现形式为init?(...))
It is *sometimes* useful to define a class, structure, or enumeration for which initialization can fail. This failure might be triggered by invalid initialization parameter values, the absence of a required external resource, or some other condition that prevents initialization from succeeding.

```
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}

```

### The init! Failable Initializer(另一种形式的Failable Initializer,如果为nil则初始化为Optional类型,如果为非nil则初始化为非Optional类型)
You typically define a failable initializer that creates an optional instance of the appropriate type by placing a question mark after the init keyword (init?). Alternatively, you can define a failable initializer that creates an **implicitly unwrapped optional instance** of the appropriate type. Do this by placing an exclamation mark after the init keyword (init!) instead of a question mark.

## Required Initializers
Write the **required** *modifier* before the definition of a class initializer to ***indicate that every subclass of the class must implement that initializer***:

```
class SomeClass {
    required init() {
        // initializer implementation goes here
    }
}
```

You must also write the **required modifier before every subclass implementation** of a required initializer, to indicate that the initializer **requirement applies to further subclasses** in the *chain*. You *do not write the **override** modifier* when overriding a required designated initializer:

```
class SomeSubclass: SomeClass {
    required init() {
        // subclass implementation of the required initializer goes here
    }
}
```

## Setting a Default Property Value with a Closure or Function（可以理解为property的默认值初始化方法）
If a stored property’s **default value** requires *some customization or setup*, you can use a ***closure*** or ***global function*** to provide a customized default value for that property. 

Whenever **a new instance of the type** that the property belongs to is **initialized**, the closure or function is **called**, and its return value is assigned as the property’s default value.(property初始化过程发生的时机)

These kinds of closures or functions typically create a temporary value of the same type as the property, tailor that value to represent the desired initial state, and then return that temporary value to be used as the property’s default value.


```
struct Chessboard {
    let boardColors: [Bool] = {   //property closure initializer
        var temporaryBoard = [Bool]()
        var isBlack = false
        for i in 1...8 {
            for j in 1...8 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
    }()
    func squareIsBlackAtRow(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}

...

let board = Chessboard()  
print(board.squareIsBlackAtRow(0, column: 1))
// Prints "true"
print(board.squareIsBlackAtRow(7, column: 7))
// Prints "false”


func globalInit() ->[Bool] {
	var temporaryBoard = [Bool]()
        var isBlack = false
        
        ...
        
        return temporaryBoard
}

struct Chessboard2 {
    let boardColors: [Bool] = globalInit()  //property global function initializer
    
    func squareIsBlackAtRow(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}

```

## 初始化过程中加入了各种语言意图，渗透于编译过程中。swift语法偏复杂，光初始化就有多种形态和细则。

