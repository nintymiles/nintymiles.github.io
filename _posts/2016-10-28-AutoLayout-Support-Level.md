---
layout: post
title:  XCode中IB对autolayout的支持，以及import swift into objective c
---
## `xib` 和 `stroyboard` 对 `autolayout` 的支持程度
`xib` 中使用autolayout时无法使用 `top/bottom layout guide`,并且强制 left／right margin 视觉缩进

`storyboard`中使用autolayout时则 top／bottom layout guide 可正常使用，并且 margin 显示正常。

## How to To import Swift code into Objective-C from the same framework
1. Under Build Settings, in Packaging, make sure the Defines Module setting for that framework target is set to “Yes”.
2. Search `Product Module Name`.Change the value to the name of your project.
3. Import the Swift code from that framework target into any Objective-C .m file within that framework target using this syntax and substituting the appropriate names:
```
	//It doesn't work in this style
	#import <ProductName/ProductModuleName-Swift.h> 
	//It works in my project
	#import "ProductModuleName-Swift.h"
```

## UITableViewCellAccessoryType
There are three options you're discussing: Disclosure Indicator, Detail Disclosure Button, and Detail Button.

The chevron only (UITableViewCellAccessoryDisclosureIndicator) indicates you should be able to tap on the row to navigate to a new view (commonly called a "detail view").

The two button options (UITableViewCellAccessoryDetailDisclosureButton and UITableViewCellAccessoryDetailButton):

Detail button will show a blue "i" button only.
Detail disclosure button will also show the chevron.



