---
layout: post
title:  Learning Notes - About Optional and  (if let/guard let)
---

## Early Exit(Guard Syntax Statement)
A guard statement, like an if statement, executes statements depending on the Boolean value of an expression. You use a guard statement to require that a condition must be true in order for the code after the guard statement to be executed. Unlike an if statement, a guard statement always has an else clause—the code inside the else clause is executed if the condition is not true.

```
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }
    
    print("Hello \(name)!")
    
    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }
    
    print("I hope the weather is nice in \(location).")
}
 
greet(["name": "John"])
// Prints "Hello John!"
// Prints "I hope the weather is nice near you."
greet(["name": "Jane", "location": "Cupertino"])
// Prints "Hello Jane!"
// Prints "I hope the weather is nice in Cupertino."
```


If the guard statement’s condition is met, code execution continues after the guard statement’s closing brace. Any variables or constants that were assigned values using an optional binding as part of the condition are available for the rest of the code block that the guard statement appears in.

If that condition is not met, the code inside the else branch is executed. That branch must transfer control to exit the code block in which the guard statement appears. It can do this with a control transfer statement such as return, break, continue, or throw, or it can call a function or method that doesn’t return, such as fatalError().

Using a guard statement for requirements improves the readability of your code, compared to doing the same check with an if statement. It lets you write the code that’s typically executed without wrapping it in an else block, and it lets you keep the code that handles a violated requirement next to the requirement.

## Optional Binding
You use optional binding to find out whether an optional contains a value, and if so, to make that value available as a temporary constant or variable. Optional binding can be used with if and while statements to check for a value inside an optional, and to extract that value into a constant or variable, as part of a single action. if and while statements are described in more detail in Control Flow.

Write an optional binding for an if statement as follows:

```
if let constantName = someOptional {
    statements
}
You can rewrite the possibleNumber example from the Optionals section to use optional binding rather than forced unwrapping:

if let actualNumber = Int(possibleNumber) {
    print("\"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("\"\(possibleNumber)\" could not be converted to an integer")
}
// Prints ""123" has an integer value of 123"
```

This code can be read as:

“If the optional Int returned by Int(possibleNumber) contains a value, set a new constant called actualNumber to the value contained in the optional.”

If the conversion is successful, the actualNumber constant becomes available for use within the first branch of the if statement. It has already been initialized with the value contained within the optional, and so there is no need to use the ! suffix to access its value. In this example, actualNumber is simply used to print the result of the conversion.

You can use both constants and variables with optional binding. If you wanted to manipulate the value of actualNumber within the first branch of the if statement, you could write if var actualNumber instead, and the value contained within the optional would be made available as a variable rather than a constant.

You can include multiple optional bindings in a single if statement and use a where clause to check for a Boolean condition. If any of the values in the optional bindings are nil or the where clause evaluates to false, the whole optional binding is considered unsuccessful.

```
if let firstNumber = Int("4"), secondNumber = Int("42") where firstNumber < secondNumber {
    print("\(firstNumber) < \(secondNumber)")
}
// Prints "4 < 42"
```

> Constants and variables created with optional binding in an if statement. are ***available only*** *within the body of the if statement*. In contrast, the constants and variables created with **a guard statement** are available in the lines of **code that follow the guard statement**, as described in Early Exit

	
## `guard let` 和 `if let`描述

```
if let and guard let serve similar, but distinct purposes.

The "else" case of guard must exit the current scope. Generally that means it must call return or abort the program. guard is used to provide early return without requiring nesting of the rest of the function.

if let nests its scope, and does not require anything special of it. It can return or not.

In general, if the if-let block was going to be the rest of the function, or its else clause would have a return or abort in it, then you should be using guard instead. This often means (at least in my experience), when in doubt, guard is usually the better answer. But there are plenty of situations where if let still is appropriate.
```

I'll try to explain the awesomeness of guard statements with some (unoptimized) code.

We'll be validating text fields on a view for user registration with first name, last name, email, phone and password.

If any textField is not containing valid text, make that field firstResponder.


```
//if let pyramid of doom
func validateFieldsAndContinueRegistration() {

    if let firstNameString = firstName.text where firstNameString.characters.count > 0{
        if let lastNameString = lastName.text where lastNameString.characters.count > 0{
            if let emailString = email.text where emailString.characters.count > 3 && emailString.containsString("@") && emailString.containsString(".") {
                if let passwordString = password.text where passwordString.characters.count > 7{
                    // all text fields have valid text
                    let accountModel = AccountModel()
                    accountModel.firstName = firstNameString
                    accountModel.lastName = lastNameString
                    accountModel.email = emailString
                    accountModel.password = passwordString
                    APIHandler.sharedInstance.registerUser(accountModel)
                } else {
                    password.becomeFirstResponder()
                }
            } else {
                email.becomeFirstResponder()
            }
        } else {
            lastName.becomeFirstResponder()
        }
    } else {
        firstName.becomeFirstResponder()
    }
}
```

You can see above, that all the strings (firstNameString, lastNameString etc) are accessible only within the scope of the if statement.

With guard statement (in the code below), you can see that these strings are available and are made use of, if all fields are valid.

```
// guard let no pyramid of doom
func validateFieldsAndContinueRegistration() {

guard let firstNameString = firstName.text where firstNameString.characters.count > 0 else {
            firstName.becomeFirstResponder()
            return
        }
guard let lastNameString = lastName.text where lastNameString.characters.count > 0 else {
            lastName.becomeFirstResponder()
            return
        }
guard let emailString = email.text where emailString.characters.count > 3 && emailString.containsString("@") && emailString.containsString(".") else {
            email.becomeFirstResponder()
            return
        }
guard let passwordString = password.text where passwordString.characters.count > 7 else {
            password.becomeFirstResponder()
            return
        }

// all text fields have valid text
    let accountModel = AccountModel()
    accountModel.firstName = firstNameString
    accountModel.lastName = lastNameString
    accountModel.email = emailString
    accountModel.password = passwordString
    APIHandler.sharedInstance.registerUser(accountModel)
}
```

> if let/guard let是涉及optional的优先语法结构

