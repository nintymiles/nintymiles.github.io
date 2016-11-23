---
layout: post
title: 静态全局变量没加“;”分号引发的血案-Unexpected token after Objective-C string
---
## 满屏的 `Unexpected token after Objective-C string` 错误
静态全局变量没有“;”分号引发的血案

```
//忘记加分号，导致无数错误
static NSString * kHLAPIURLPrefix = @"api/main/"
```


```
/Users/seanren/Documents/WorkSpace/zbenjia_workspace/iOS/HL_App_Zbenjia/HL_App_Zbenjia/Network/Components/Managers/HLURLResponse.h:13:2: Unexpected token after Objective-C string
```

错误截图1
![Screen Shot 2016-11-23 at 1.58.49 P](media/Screen%20Shot%202016-11-23%20at%201.58.49%20PM.png)

错误截图2
![Screen Shot 2016-11-23 at 1.58.14 P](media/Screen%20Shot%202016-11-23%20at%201.58.14%20PM.png)




