---
layout: post
title: iOS中倒计时程序，考虑线程暂停场景
---
# iOS中倒计时程序，考虑线程暂停场景。
iOS App进入后台时，GCD线程也会跟着暂停。当程序进入前台后，GCD线程恢复。因而倒计时程序需要考虑这一点，通过加入时间的比对来实现。

```
+ (void)countDownWithLapseTime:(int)lapseTime andBlock:(void(^)(int timeLapse)) countDownBlock{
    __block dispatch_source_t timer;
    NSTimeInterval timeInterval=lapseTime;
    NSDate *startTime = [NSDate date];
    
    if (timer==nil) {
        __block int timeout = timeInterval; //倒计时时间
        
        if (timeout!=0) {
            dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
            timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0,queue);
            dispatch_source_set_timer(timer,dispatch_walltime(NULL, 0),1.0*NSEC_PER_SEC, 0); //每秒执行
            dispatch_source_set_event_handler(timer, ^{
                if(timeout<=0){ //倒计时结束，关闭
                    dispatch_source_cancel(timer);
                    timer = nil;
                    dispatch_async(dispatch_get_main_queue(), ^{
                        countDownBlock(timeout);
                    });
                }else{
                    timeout--;

                    NSDate *currentDate = [NSDate date];
                    int leftTime = timeInterval - ((int)(currentDate.timeIntervalSince1970 - startTime.timeIntervalSince1970));
                    if((timeout - leftTime) > 1) //如果真实计时和倒计数计时差别大于1秒，则同步。
                        timeout = leftTime;

                    
                    //回主线程执行countDownBlock
                    dispatch_async(dispatch_get_main_queue(), ^{
                        countDownBlock(timeout);
                    });
                    
                }
            });
            dispatch_resume(timer);
        }
    }
}
```


