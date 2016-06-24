---
layout: post
title:  Learning Notes - Swift Properties
---

## Properties
**Properties** associate values with a ***class*** or a ***structure***. There are *two types of properties*, which are as follows:
- **Stored properties**: They store **variable** or **constant** values as part of an instance of a class or structure. Stored properties can also have **property observers** that can monitor the property for changes and respond with custom actions when the value of the *property changes*.
- **Computed properties**: They ***do not store*** a value **themselves**, but **retrieve** and possibly **set *other properties***. The value returned by a computed property can also be calculated *when it is requested*.

### Stored properties
A stored property is a **variable** or **constant** that is stored as part of an instance of **a class or structure**. We can provide a default value for stored properties. ~~These are de ned with the **var** keyword.~~

### Computed properties
Computed properties are properties **that do not have backend variables** that are used to store the values associated with the property. The values of **a computed property are usually computed when code requests it**. You can think of a computed property as a function disguised(伪装) as a property. Let's take a look at how we would de ne a read-only computed property:

```
var salaryWeek: Double {
   get{
    return self.salaryYear/52 
   }
}
```

We can **simplify** the definition of the read-only computed property by removing the get keyword. We could rewrite the salaryWeek function like this:

```
var salaryWeek: Double {
   }
```
Computed properties are not limited to being read-only, we can also **write** to them. To enable the salaryWeek property to be writeable, we would need to add a setter method. The following example shows how we would add a setter method that will set the salaryYear property, based on the value being passed into the salaryWeek property:

```
var salaryWeek: Double {
     get {
       return self.salaryYear/52
     } 
     set (newSalaryWeek){
       self.salaryYear = newSalaryWeek*52
    } 
} 
```
We can **simplify** the setter definition by **not defining a name for the new value**. In this case, the value would be assigned to **a default variable name**, ***newValue***. The salaryWeek computed property could be rewritten like this:

```
var salaryWeek: Double {
     get{
     return self.salaryYear/52
     } 
     set{
     self.salaryYear = newValue*52
     } 
   }
```


### Property observers
Property observers are called every time the value of the property is set. We can add property observers to **any non-lazy stored property**. We can also add property observers to** any *inherited* stored or computed property** by overriding the property in the subclass.

There are **two property observers** that we can set in Swift—***willSet*** and ***didSet***. The willSet observer is called right **before** the property is set, and the didSet observer is called right **after** the property is set.

One thing to note about property observers is that **they are not called when the value is set during initialization**. Let's look at how we would add a property observer to the salary property of our EmployeeClass class and EmployeeStruct structure:
   
```
var salaryYear: Double = 0.0 {
     willSet(newSalary) {
     } print("About to set salaryYear to \(newSalary)")
     didSet {
       if salaryWeek > oldValue {
       } print("\( rstName) got a raise")
       else {
   } } } print("\( rstName) did not get a raise")
```
