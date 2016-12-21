## 如何修改iOS TabBar的顶部边线（分割线）的颜色？
根据HID规范，iOS TabBar风格不能随意修改，并且在SDK进行限定，如果要修改顶部边线的颜色，有一个临时方法，但是需要设置**shadowImage**和**backgroundImage**，而且必须**同时设置**。 **NavigationBar**的设置同理。

```
...
//shadowImage为具体变现的颜色设置
[tabBarController.tabBar setShadowImage:[self imageWithColor:[UIColor colorFromHexString:@"#dcdcdc"] size:CGSizeMake(tabBarController.tabBar.frame.size.width,1.0f)]];
//设置tabBar的背景色为[UIImage new]实例，如果设置为nil则不起作用
[tabBarController.tabBar setBackgroundImage:[UIImage new]];
    
...
    
-(UIImage *)imageWithColor:(UIColor *)color size:(CGSize)size {
    if (!color || size.width <=0 || size.height <=0)
        return nil;

    CGRect rect = CGRectMake(0.0f, 0.0f, size.width, size.height);
    UIGraphicsBeginImageContextWithOptions(rect.size,NO, 0);
    CGContextRef context =UIGraphicsGetCurrentContext();
    CGContextSetFillColorWithColor(context, color.CGColor);
    CGContextFillRect(context, rect);
    UIImage *image =UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    return image;
}
```


