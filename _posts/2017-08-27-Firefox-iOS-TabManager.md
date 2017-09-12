---
layout: post
title: Firefox-iOS 源代码学习
---

## TabManager.swift

- TabManager通过对subscript(index:int) subscript(webView:WKWebView)对下标进行了重新定义，第一个可以通过index查找到tab，并且即便index溢出也不crash。第二个通过webView实例查找其是否是tabs元素。


```    
//重载定义了下标函数，如果index越界则返回nil，而不是抛出异常。
    subscript(index: Int) -> Tab? {
        assert(Thread.isMainThread)

        if index >= tabs.count {
            return nil
        }
        return tabs[index]
    }

    //重载定义了下标，支持以元素类型查找元素
    subscript(webView: WKWebView) -> Tab? {
        assert(Thread.isMainThread)

        for tab in tabs where tab.webView === webView {
            return tab
        }

        return nil
    }
```
- 通过Swift的where（循环中）和filter（数组）语法，可以快速查找对应元素

```
//通过tab使用where语句反向查找index
    func getIndex(_ tab: Tab) -> Int? {
        assert(Thread.isMainThread)

        for i in 0..<count where tabs[i] === tab {
            return i
        }

        assertionFailure("Tab not in tabs list")
        return nil
    }

    func getTabForURL(_ url: URL) -> Tab? {
        assert(Thread.isMainThread)

        //用filter通过tab的属性反向查找
        return tabs.filter { $0.webView?.url == url } .first
    }
```

