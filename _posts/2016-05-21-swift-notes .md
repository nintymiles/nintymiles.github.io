---
layout: post
title: Swift Learning Notes  - Closure
---

## Closure Definition
*Closures are **self-contained blocks of functionality** that can be **passed around** and **used** in your code.*

- Closures are similar to blocks in C and Objective C
- Closures can **capture** and **store** **references** to ***any*** constants and variables *from the context in which they are defined*.
- Global and nested functions are special cases of closures,not all kinds of functions.
	- Global functions are closures that have a name and do not capture any values
	- Nested functions are closures that have a name and can capture values from their enclosing function
	- Closure expressions are unnamed closures written in a lightweight syntax that can capture values from their surrounding context.
- **Closure syntax optimisation**
	- Inferring parameter and return value types from context Implicit returns from single-expression closures (*two key point here*)
	- Shorthand argument name
	- Trailing closure syntax

