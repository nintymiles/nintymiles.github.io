---
layout: post
title: Swift Operators
---

## Nil Coalescing operator (??)
The **nil coalescing operator** (*a ?? b*) unwraps an optional a if it contains a value, or returns a default value b if a is nil. The expression a is always of an optional type. *The expression b must **match** the type that is stored inside a.*

**So it is a special tenary operator case for optional value operating.**

The nil coalescing operator is shorthand for the code below:


```
a != nil ? a! : b
```


