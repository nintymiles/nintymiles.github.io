---
layout: post
title:  Learning Notes - Swift Enum's Associcated Values
---
## Swift Enum's Associcated Values

It is sometimes useful to be able to store associated values of other types alongside these case values in enum. This enables you to store additional custom information along with the case value, and permits this information to vary each time you use that case in your code.

You can define Swift enumerations to store associated values of **any given type**, and the value types **can be different for each case** of the enumeration if needed. Enumerations similar to these are known as discriminated unions, tagged unions, or variants in other programming languages.

### how to use associatetd values in enum?
the associated values can be extracted as part of the switch statement. You extract each associated value as a constant (with the let prefix) or a variable (with the var prefix) for use within the switch case’s body:

```
switch productBarcode {
case .UPCA(let numberSystem, let manufacturer, let product, let check):
    print("UPC-A: \(numberSystem), \(manufacturer), \(product), \(check).")
case .QRCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP. 
```
###speical style of using associated values in enum
If **all** of the associated values for an enumeration case are extracted as constants, or if **all** are extracted as variables, you can place a **single var** or **let** annotation **before** the **case** name, for brevity:

```
switch productBarcode {
case let .UPCA(numberSystem, manufacturer, product, check):
    print("UPC-A: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .QRCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP.
```


