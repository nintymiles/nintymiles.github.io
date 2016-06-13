---
layout: post
title: Learning Notes
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

