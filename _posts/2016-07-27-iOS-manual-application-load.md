---
layout: post
title:  Learning Notes - iOS Application如何手动管理RootViewController加载
---

## 使用manual方式在AppDelegate中加载rootViewController


```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];  //第一步，生成window对象，注意IB会自动生成window初始化对象，而不只是ViewControllers。
    
    // Override point for customization after application launch.
    ViewController *rootVC=[[ViewController alloc] init];
    self.window.rootViewController=[[UINavigationController alloc] initWithRootViewController:rootVC];  //第二步，生成rootViewController实例并赋给window的对应属性
    
    [self.window makeKeyAndVisible];  //第三步，让此window成为关键window
    
    return YES;
}
```


