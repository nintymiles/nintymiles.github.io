---
layout: post
title: 为什么`self.navigationController?.popViewController(animated: true)`在 Swift 3 会有编译器警告？
---
# 为什么`self.navigationController?.popViewController(animated: true)`在 Swift 3 会有编译器警告？
警告信息`"Expression of type "UIViewController?" is unused".`，这是因为popViewController方法有返回值，而代码中没有对返回值进行捕获导致的编译警告。这是由于Swift 3语法的改变引起的。

下面引用为stackoverflow的解释：

```
TL;DR

popViewController(animated:) returns UIViewController?, and the compiler is giving that warning since you aren't capturing the value. The solution is to assign it to an underscore:

_ = navigationController?.popViewController(animated: true)
Swift 3 Change

Before Swift 3, all methods had a "discardable result" by default. No warning would occur when you did not capture what the method returned.

In order to tell the compiler that the result should be captured, you had to add @warn_unused_result before the method declaration. It would be used for methods that have a mutable form (ex. sort and sortInPlace). You would add @warn_unused_result(mutable_variant="mutableMethodHere") to tell the compiler of it.

However, with Swift 3, the behavior is flipped. All methods now warn that the return value is not captured. If you want to tell the compiler that the warning isn't necessary, you add @discardableResult before the method declaration.

If you don't want to use the return value, you have to explicitly tell the compiler by assigning it to an underscore:

_ = someMethodThatReturnsSomething()
Motivation for adding this to Swift 3:

Prevention of possible bugs (ex. using sort thinking it modifies the collection)
Explicit intent of not capturing or needing to capture the result for other collaborators
The UIKit API appears to be behind on this, not adding @discardableResult for the perfectly normal (if not more common) use of popViewController(animated:) without capturing the return value.

Read More

SE-0047 Swift Evolution Proposal
Accepted proposal with revisions
shareimprove this answer
edited Aug 25 '16 at 18:11
answered Jun 15 '16 at 19:00
```

```
tktsubota
5,52131235
13	 	
This is (in my opinion) definitely a step back from Swift 2, especially when there are methods like this that, even though they do return a value, there are perfectly valid use cases where you just don't use it. – Nicolas Miari Jun 16 '16 at 3:26
13	 	
1. You don't need the let: you can just assign to _ without preceding it with let or var. – rickster Jun 16 '16 at 4:34
1	 	
@rickster Did not know that will add to answer. – tktsubota Jun 16 '16 at 4:36
4	 	
2. @NicolasMiari File a bug. There's an annotation (@discardableResult) for functions that do return a value but where it's expected that one might ignore the return value. UIKit just hasn't applied that annotation to their API. – rickster Jun 16 '16 at 4:36
22	 	
This is horrible syntax. Why would they do this? Yuck. – David Shaw Jul 12 '16 at 21:18
```

