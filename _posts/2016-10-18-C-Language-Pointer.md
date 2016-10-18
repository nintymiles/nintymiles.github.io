---
layout: post
title:  Learning Notes -  Pointer semantic in C Language
---
## The indirect pointer
Summary: Pointer-Related Operators**The Address Operator**:`&`General Comments:
When followed by a variable name, & gives the address of that variable. Example:
`&nurse` is the address of the variable nurse.**The Indirection Operator**:*General Comments:When followed by a pointer name or an address, * gives the value stored at the pointed-to address.Example:
```
nurse = 22;ptr = &nurse; // pointer to nurseval = *ptr; // assigns value at location ptr to val
```The net effect is to assign the value 22 to val. 

