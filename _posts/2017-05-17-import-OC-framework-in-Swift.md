---
layout: post
title: 如何在Swift中引入外部ObjectiveC框架并使用
---

## 如何在Swift中引入外部ObjectiveC框架
### 首先注意编译设置
在Swift和ObjectiveC混编时，如果引入ObjectiveC framework，那么首先要将编译设置中` define module = YES`
### 对外部framework在Swift代码中如何使用
对于外部引入的`ObjectiveC framework`会被当作`Swift Module`，在使用时直接引入 `import <ObjectiveC framework>` 即可使用。


 

