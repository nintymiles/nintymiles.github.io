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

## Initializer Parameters Without External Names(必须特殊处理，才可以不适用External names)
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


