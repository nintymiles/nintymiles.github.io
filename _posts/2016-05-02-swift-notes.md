---
layout: post
title: Swift Learning Notes
---

# Swift Learning Notes
---
>Swift can be thought of as Objective-C reimagined using modern concepts and safe programming patterns. In Apple's own words, Swift is like Objective-C without
the C. Chris Lattner, the creator of Swift, said Swift took language ideas from Objective-C, Rust, Haskell, Ruby, Python, C#, CLU, and far too many others to list. At WWDC 2014, Apple really stressed that Swift was safe by default. Swift was designed to eliminate many common programming errors, making applications more secure and less prone to bugs. Swift 2 added two additional core features to the language—availability and error handling—which are designed to make it even easier to write safe code.

## Getting started with Playgrounds

>Playgrounds are interactive work environments that let us write code and see
the results immediately.

## How does Playgournds open the debug area

You can open it manually by pressing the shift + command + Y keys together. The debug area is so userful.

## Swfit Language Syntax

### Comments

Writing comments in Swift code is a little different from writing comments in Objective-C code. 

**Double slash \\\\** for single line comments
**/\* and \*/** for multi line comments

What has changed is how we document the parameters and the return value. To document any parameter, we use the :parm:  eld, and for the return value, we use the :return:  eld.

### Semicolons
The semicolons are optional in Swift;

### Parentheses
The parentheses around conditional statements are optional

### Curly Braces
The the curly bracket is required after statements

### An assignment operator does not return a value
In Swift, this statement is not valid. Using an assignment operator (=) in a conditional statement (if and while) will throw an error. 

### Spaces are optional in conditional and assignment statements
For both conditional (if and while) and assignment (=) statements, the white spaces are optional.

### Powerful Print() function
>Official Definition:Writes the textual representations of items, separated by separator and terminated by terminator, into the standard output.

>The textual representations are obtained for each item via the expression String(item).

Prior to Swift 2, we had two separate print functions: print() and println(). Now both of these functions have been combined into the single print() function.

There are two ways to use print() function:

- We can include the value of variables and/or constants using a special sequence of characters, \\( )
- by separating the values within the print() function with commas
   
```swifty
var name = "Jon"
var language = "Swift"
var message1 = " Welcome to the wonderful world of "
var message2 = "\(name) Welcome to the wonderful world of \(language)!"

print(name, message1, language, "!")
print(message2)
```

The usage for **seperator** and **terminator** parameters:


```swifty
var name1 = "Jon"
var name2 = "Kim"
var name3 = "Kailey"
var name4 = "Kara"
print(name1, name2, name3, name4, separator:", ",terminator:"")
```

The usage for **toStream** parameter:

```
var name1 = "Jon"
var name2 = "Kim"
var name3 = "Kailey"
var name4 = "Kara"
var line = ""

print(name1, name2, name3, name4, separator:", ",terminator:"", toStream:&line)
```
### Strings and Characters

- A *string* is a series of characters
- A string is an ordered collection of characters, such as Hello or Swift. 
- Swift has the ***String*** type to represent strings
- *String* can be accessed in various way,including as a collection of *Character* values
- Swift's *String* and *Character* types privode a fast,Unicode-compliant way to work with text in your code,Swift's *String* type is a fast,modern string implementation

>Note

>Swift’s String type is bridged with Foundation’s NSString class. If you are working with the Foundation framework in Cocoa, the entire NSString API is available to call on any String value you create when type cast to NSString, as described in AnyObject. You can also use a String value with any API that requires an NSString instance.


#### String Literals

a string iteral is a fixed sequence of textual characters that must be surrounded by a pair of *double quotes* (**""**)

```
Use a string literal as an initial value for a constant or variable:

let someString = "Some string literal value"
```

#### Initializing an Empty String
Intitialize syntax:

```
var emptyString = "" // empty string literal
var anotherEmptyString = String() // initializer syntax
// these two strings are both empty, and are equivalent to each other
```

How to decide whether a **String** is empty:

```
if emptyString.isEmpty {
print("Nothing to see here")
}
// Prints "Nothing to see here"
```

#### Constants and Variables
Constants and variables associate an identi er (such as myName or currentTemperature) with a value of a particular type (such as String or Int), where the identi er can be used to retrieve the value
-  a variable can be updated or changed
-  a constant cannot be changed once a value is assigned to it

>The use of constants is encouraged in Swift. If we do not expect or want the value to change, we should declare it as a constant. This adds a very important safety constraint to our code that ensures that the value never changes.

You can use almost any character in the identi er of a variable or constant (even Unicode characters); however, there are a few rules that you must follow:
- An identifier must not contain any whitespace
- An identifier must not contain any mathematical symbols
- An identifier must not contain any arrows
- An identifier must not contain private use or invalid Unicode characters
- An identifier must not contain line- or box-drawing characters
- An identifier must not start with a number, but they can contain numbers
- If you use a Swift keyword as an identifier, surround it with back ticks

#### Type safety
Swift is a type-safe language. In a type-safe language, we are required to be clear
on the types of values we store in a variable.

#### Numberic types
Swift contains many of the standard numeric types that are suitable for storing various integer and  oating-point values.

#### Swift REPL
You can set up and run a REPL — read, eval, print, loop — in order to write interactive Swift code in the command line. To enable this capability, open the Terminal app from your /Applications/Utilities folder and type *xcrun swift* (or *lldb --repl*) at the command prompt and press return.


