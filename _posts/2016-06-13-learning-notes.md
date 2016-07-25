---
layout: post
title: Learning Notes - Swift Basic Syntax
---

## Swift中大括号和括号的用法（how curly brackets and parentheses are used in Swift）

- In Swift, unlike other C-like languages, curly brackets are ***required*** for *conditional statements* and *loops*.
- Unlike other C-like languages, the parentheses **around** *conditional expressions* in Swift are ***optional***. 

## Repeat-While
The difference between the while and repeat-while loops is that the while loops check the conditional statement prior to executing the block of code the  rst time; therefore, all the variables in the conditional statements need to be initialized prior to executing the while loop. The repeat-while loop will run through the loop block prior to checking the conditional statement for the  rst time; this means that we can initialize the variables in the conditional block of code. Use of the repeat-while loop is preferred when the conditional statement is dependent on the code in the loop block. The repeat-while loop takes the following format:

```
//syntax
repeat {
      block of code
   } while condition
   
//example
var ran: Int
   repeat {
       ran = Int(arc4random() % 5)
   } while ran < 4
   
```

## Switch Statement
The switch statement takes a value and then compares it to the several possible matches, and executes the appropriate block of code based on the  rst successful match. The switch statement is an alternative to using the if-else statement when there could be several possible matches. The switch statement takes the following format:

```
switch value {
     case match1 :
       block of code
     case match2 :
       block of code
     ...... as many cases as needed
     default :
       block of code
   }
```

***Unlike*** the switch statements in most other languages, in Swift, it **does not fall through to the next case statement**; therefore, we **do not need** to use a **break** statement to prevent the fall through. This is** another safety feature that is built into Swift** since one of the most common programming mistakes, with the switch statement, made by beginner programmers is to forget the break statement at the end of the case statement. 

It is possible to include multiple items in a single case. To set multiple items within a single case, we would need to separate the items with a comma. Let's look at how we would use the switch statement to tell us if a character was a vowel or a consonant:
   
```
var char : Character = "e"
   switch char {
   case "a", "e", "i", "o", "u":
       print("letter is a vowel")
   case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
   "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
       print("letter is a consonant")
   default:
       print("unknown letter")
   }
```

It is also possible to check the value of a switch statement to see whether it is included in a ***range***. To do this, we would use a range operator in the case statement, as shown in the following example:

```
var grade = 93
   switch grade {
   case 90...100:
       print("Grade is an A")
   case 80...89:
       print("Grade is a B")
   case 70...79:
       print("Grade is an C")
   case 60...69:
 print("Grade is a D")
[ 94 ]
  
case 0...59:
    print("Grade is a F")
default:
    print("Unknown Grade")
}
```

In Swift, any case statement can contain** an optional guard condition** that can provide an additional condition to validate. The guard condition is defined with the **where** keyword. Let's say, in our preceding example, we had students who were receiving special assistance in the class and we wanted to de ne a grade of D for them in the range of 55 to 69. The following example shows how to do this:
   
```
var studentId = 4
   var grade = 57
   switch grade {
   case 90...100:
       print("Grade is an A")
   case 80...89:
       print("Grade is a B")
   case 70...79:
       print("Grade is an C")
   case 55...69 where studentId == 4:
       print("Grade is a D for student 4")
   case 60...69:
       print("Grade is a D")
   case 0...59:
       print("Grade is a F")
   default:
       print("Unknown Grade")
   }
```

One thing to keep in mind with the guard expression is that Swift will attempt to match the value starting with the  rst case statement and working its way down checking each case statement in order. This means that if we put the case statement with the guard expression after the Grade F case statement, then the case statement with the guard expression would **never be reached**.

> Every switch statement must have a match for all the possible values. This means that unless we are matching against an enum, each switch statement must have a **default** case.

Switch statements are also **extremely useful** for evaluating **enumerations**. Since an enumeration has a  nite number of values, if we provide a case statement for all the values in the enumeration, we do not need to provide a default case. The following example shows how we can use a switch statement to evaluate an enumeration:
   
```
Product {
       case Book(String, Double, Int)
       case Puzzle(String, Double)
   }
   var order = Product.Book("Mastering Swift 2", 49.99, 2015)
   switch order {
   case .Book(let name, let price, let year):
       print("You ordered the book \(name) for \(price)")
   case .Puzzle(let name, let price):
       print("You ordered the Puzzle \(name) for \(price)")
   }
```

## Control Transfer Statement - Swift
Control transfer statements are used to **transfer control to another part of the
code**. Swift offers  ve control transfer statements; these are continue, break, fallthrough, guard, throws, and return. 

### The fallthrough statement ("强行通过")
In Swift, the switch statements do not fall through like other languages; however,
we can use the fallthrough statement to **force them to fall through**. The fallthrough statement can be very ***dangerous*** because once a match is found, **the next case defaults to true** and that* code block is executed*. The following example illustrates this:
  
```
var name = "Jon"
   var sport = "Baseball"
   switch sport {
   case "Baseball":
       print("\(name) plays Baseball")
       fallthrough
   case "Basketball":
       print("\(name) plays Basketball")
       fallthrough   //fallthrough告诉代码执行下面的case代码段
   default:
       print("Unknown sport")
   }
```

### Guard Statement
With Swift 2, Apple introduced the new guard statement. The guard statement focuses on performing a function if a condition is false; this allows us to trap errors and perform the error conditions early in our functions:

```
var x = 9
   guard x > 10 else {
     // Do error condition
return }
   // Functional code here
   
//下面为if condition实现，对比一下优劣
var x = 9
   if x > 10 {
     // Functional code here
   } else {
      // Do error condition
   }

```

Let's look at some more examples of the guard statement. The following example shows ***how we would use the guard statement to verify that an optional contains a valid value***:
   
```
func guardFunction(str: String?) {
       guard let goodStr = str else {
            print("Input was nil")
            return 
        }
        print("Input was \(goodStr)")
   }
```

