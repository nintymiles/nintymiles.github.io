---
layout: post
title:  Learning Notes - iOS 启动过程加入广告画面或者启动动画的一般逻辑
---
## iOS 启动过程加入广告画面或者启动动画的一般逻辑

```
- (void)viewDidLoad {
    [super viewDidLoad];
    
    //基本逻辑为：首先在启动VC中从LaunchScreen.xib中获取LaunchView，然后在launchView上叠加广告图片，再后给广告图片加入动画的出入效果或者加入其他动画效果。最后讲launchView从当前ViewController的View层次中去除，恢复正常的VC View层次。
    
    //这一步是获取LaunchScreen.storyboard里的UIViewController,UIViewController 的identifer是LaunchScreen
    UIViewController *viewController = [[UIStoryboard storyboardWithName:@"LaunchScreen" bundle:[NSBundle mainBundle]] instantiateViewControllerWithIdentifier:@"LaunchScreen"];
    UIView *launchView = viewController.view;
    UIImageView  * Imageview= [[UIImageView  alloc]initWithFrame:[UIScreen mainScreen].bounds];
    [launchView addSubview:Imageview];
    [self.view addSubview:launchView];
    
    //这一步是获取上次网络请求下来的图片，如果存在就展示该图片，如果不存在就展示本地保存的名为test的图片
    NSMutableData * data = [[NSUserDefaults standardUserDefaults]objectForKey:@"imageu"];
    if (data.length>0) {
         Imageview.image = [UIImage imageWithData:data];
    }else{
    
     Imageview.image = [UIImage imageNamed:@"Test"];
    }
   

    
 //下面这段代码，是调用AFN下载文件的方法，异步
    NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
    AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
    NSURL *URL = [NSURL URLWithString:@"http://s16.sinaimg.cn/large/005vePOgzy70Rd3a9pJdf&690"];
    NSURLRequest *request = [NSURLRequest requestWithURL:URL];
    NSURLSessionDownloadTask *downloadTask = [manager downloadTaskWithRequest:request progress:nil destination:^NSURL *(NSURL *targetPath, NSURLResponse *response) {

        NSURL *documentsDirectoryURL = [[NSFileManager defaultManager] URLForDirectory:NSDocumentDirectory inDomain:NSUserDomainMask appropriateForURL:nil create:NO error:nil];
        return [documentsDirectoryURL URLByAppendingPathComponent:[response suggestedFilename]];
    } completionHandler:^(NSURLResponse *response, NSURL *filePath, NSError *error) {
        NSLog(@"File downloaded to: %@", filePath);
        
        NSData * image = [NSData dataWithContentsOfURL:filePath];
        [[NSUserDefaults standardUserDefaults]setObject:image forKey:@"imageu"];
        
        
    }];
    [downloadTask resume];
    
    
    //这段代码，可以实现第二张图片有3D的动画效果，动画结束后，进行同步的网络请求，请求的是广告页图片
    [UIView animateWithDuration:6.0f delay:0.0f options:UIViewAnimationOptionBeginFromCurrentState animations:^{
        
        //launchView.alpha = 0.0f;
        launchView.layer.transform = CATransform3DScale(CATransform3DIdentity, 1.5f, 1.5f, 1.0f);//对layer层进行3D缩放，实际只进行了x，y轴的同等规模缩放
    } completion:^(BOOL finished) {
     
     
                NSString * ad_imgUrl  = @"http://www.uimaker.com/uploads/allimg/121217/1_121217093327_1.jpg";
                AppDelegate *appDelegate = (AppDelegate *)[[UIApplication sharedApplication] delegate];
                [BBLaunchAdMonitor showAdAtPath:ad_imgUrl
                                         onView:appDelegate.window.rootViewController.view
                                   timeInterval:5
                               detailParameters:@{@"carId":@(12345), @"name":@"品质生活"}];
                   [launchView removeFromSuperview];  //所有动画执行完毕之后，将launchView层去掉。
                
        

    }];
}
```


