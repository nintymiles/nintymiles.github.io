---
layout: post
title: Swift2.3中如何设置NavigationItem
---
## 如何设置NavigationItem

```
let rightBarItem = UIBarButtonItem.init(image: UIImage(named: "money_account_icon"), style: .Plain, target: self, action: #selector(self.doRightNaviMenuAction(_:))) //使用init(...)方法初始化BarButtonItem in Swift2.3
self.navigationItem.rightBarButtonItem = rightBarItem
        
let leftBtn = UIButton.init(type: .Custom)
leftBtn.setImage(UIImage(named: "header"), forState: .Normal)
leftBtn.frame=CGRectMake(0, 0, 30, 30)   //button的大小由此处决定
let leftBarBtn = UIBarButtonItem.init(customView: leftBtn)  
navigationItem.leftBarButtonItem = leftBarBtn 
```

