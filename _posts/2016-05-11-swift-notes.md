---
layout: post
title: Swift Learning Notes - Optional and Enumeration
---


>Swift can be thought of as Objective-C reimagined using modern concepts and safe programming patterns. In Apple's own words, Swift is like Objective-C without
the C. Chris Lattner, the creator of Swift, said Swift took language ideas from Objective-C, Rust, Haskell, Ruby, Python, C#, CLU, and far too many others to list. At WWDC 2014, Apple really stressed that Swift was safe by default. Swift was designed to eliminate many common programming errors, making applications more secure and less prone to bugs. Swift 2 added two additional core features to the language—availability and error handling—which are designed to make it even easier to write safe code.

## Optional variables
In Swift,normally,the variables are considered to be nonoptional. an optional variable is a variable that we are able to assign nil (no value) to. **Optional variables and constants are defined using *?* (question mark).** 

>Optional variables were added to the Swift language as a safety feature. 

To use force unwrapping, we must  rst make sure that the optional has a non-nil value and then we can use the explanation point to access that value. 

```
var name: String?
Name = "Jon"
if name != nil {
    var newString = "Hello " + name!
}
```

We use **optional binding** to check whether an optional variable or constant has a non-nil value, and, if so, assign that value to a temporary variable. For optional binding, we use the **if-let** or **if-var** keywords together. If we use if-let, the ***temporary*** value is a constant and cannot be changed, while the if-var keywords puts the temporary value into a variable that allows us to change the value. 

```
if let temp = myOptional {
    print(temp)
    print("Can not use temp outside of the if bracket")
} else {
    print("myOptional was nil")
}
```

We can also test multiple optional variables in one line. We do this by separating each optional check with a comma. The following example shows how to do this:

```
   if let myOptional = myOptional, myOptional2 = myOptional2,
   myOptional3 = myOptional3 {
     // only reach this if all three optionals
     // have non-nil values
   }
```

>Within the optional binding zone,value type variable's changing is **independent**.so it doesn't affect the value of the outter variable,even the variable is declared outter the scope. But reference type variable's changing is **relative**.

**Optional chaining** allows us to call properties, methods, and subscripts on an optional that might be nil. If any of the chained values return nil, the return value will be nil. 

```
var s = car?.tires?.tireSize
```

## Enumerations
Enumerations (otherwise known as enums) are a **special** data type that enables us
to **group** related types together and use them in a **type safe** manner.

enums in Swift are **not tied to integer values**. We can define an enum with a type (string, character, integer, or floating-point) and then it's actual value (known as the raw value) will
be the assigned value.

>Enums' member can have two kind of value,one is **hasValue** (the value is ***herent***),the other is **rawValue** (the value is  ***optional***)


```
enum Planets{
    case Mercury  //only have "hashValue",hashValue=0
    case Venus
    case Earth
    case Mars
    case Jupiter
    case Saturn
    case Uranus
    case Neptune
}
```

Enums can come prepopulated with raw values, which are required to be of **the same type**. The raw values can be **string**, **character**, **integer**, or **floating**-point values.

```
enum Devices: String {
       case iPod = "iPod"  //the member now have two values,one is "hashValue"-0,the other is "rawValue"-"iPod"
       case iPhone = "iPhone"
       case iPad = "iPad"
   }
print("We are using an " + Devices.iPad.rawValue)
```

If integers are used for the raw values of an enum, we do not have to assign a value to each member. If no value is present, the raw values will be auto-incremented.

```
enum Planets: Int  {
       case Mercury = 1   //hasValue=0,rawValue=1
       case Venus   //hashValue=1,rawValue=2
       case Earth
       case Mars
       case Jupiter
       case Saturn
       case Uranus
       case Neptune
   }
print("Earth is planet number \(Planets.Earth.rawValue)")
```

**enums can also have *associated* values. Associate values allow us to store additional information along with member values. This additional information can vary each time we use the member. It can also be of any type, and the types can be different for each member.**

```
enum Product {
       case Book(Double, Int, Int)
       case Puzzle(Double, Int)
   }
   var masterSwift = Product.Book(49.99, 2015, 310)
   var worldPuzzle = Product.Puzzle(9.99, 200)
   switchmasterSwift {
   case .Book(let price, let year, let pages):
       print("Mastering Swift was published in \(year) for the price
           of \(price) and has \(pages) pages")
   case .Puzzle(let price, let pieces):
       print("Master Swift is a puzze with \(pieces) and sells for
       \(price)")
}
   switchworldPuzzle {
   case .Book(let price, let year, let pages):
       print("World Puzzle was published in \(year) for the price of
       \(price) and has \(pages) pages")
   case .Puzzle(let price, let pieces):
       print("World Puzzle is a puzze with \(pieces) and sells for
\(price)") }
```

Enums have a **shorter** version.This shorter version lets us de ne multiple members in a single line, separated by *commas*.

```
enum Planets {
       case Mercury, Venus, Earth, Mars, Jupiter
       case Saturn, Uranus, Neptune
}
```


Once the enum variable type is inferred or defined, we can then assign a new value without the enum pre x, as shown here:

```
planetWeLiveOn = .Mars
```

You can match an enum value using the traditional equals (==) operator or use a switch statement. 

```
// Using the traditional == operator
   if planetWeLiveOn == .Earth {
       print("Earth it is")
   }
   // Using the switch statement
   switch planetWeLiveOn {
   case .Mercury:
       print("We live on Mercury, it is very hot!")
   case .Venus:
       print("We live on Venus, it is very hot!")
   case .Earth:
       print("We live on Earth, just right")
   case .Mars:
       print("We live on Mars, a little cold")
   default:
       print("Where do we live?")
   }
```
   
   


