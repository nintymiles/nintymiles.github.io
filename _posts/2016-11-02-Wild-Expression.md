---
layout: post
title: Swift Excerpt - Wildcard Expression
---
## Wildcard Expression
A wildcard expression is used to explicitly ignore a value during an assignment. For example, in the following assignment 10 is assigned to x and 20 is ignored:

```
(x, _) = (10, 20)
// x is 10, and 20 is ignored
GRAMMAR OF A WILDCARD EXPRESSION
```

```
‌ wildcard-expression → _
```

