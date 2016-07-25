---
layout: post
title:  Learning Notes - Swift 条件编译 和 extension
---

## Swift支持类C的条件编译

```
#if os(iOS)
    import UIKit
#elseif os(OSX)
    import AppKit
#endif
```

## Swift extension 类似于 Objective C extension

```
extension CAShapeLayer {
    
    public convenience init(pathString: String) {
        self.init()
        let svgPath = UIBezierPath(pathString: pathString)
        self.path = svgPath.CGPath
    }
    
    public convenience init(SVGURL: NSURL) {
        self.init()
        _ = SVGParser(SVGURL: SVGURL, containerLayer: self)
    }
}
```

