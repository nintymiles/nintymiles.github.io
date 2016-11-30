---
layout: post
title: NSDateComponents和NSCalendar组合获取日期的相关组成部分
---
## 如何用NSDateComponents和NSCalendar获取日期的各个部分
- NSCalendar对世界上现存的常用历法进行了封装，既提供了不同历法的时间信息，又支持日历的计算。
- NSDateFomatter表示的时间默认以公历（即阳历）为参考，可以通过设置calendar属性变量获得特定历法下的时间表示。
- NSDate独立与任何历法的，它只是时间相对于某个时间点的时间差；NSDate是进行日历计算的基础。
- NSDateComponents将时间表示成适合人类阅读和使用的方式，通过NSDateComponents可以快速而简单地获取某个时间点对应的“年”，“月”，“日”，“时”，“分”，“秒”，“周”等信息。当然一旦涉及了年月日时分秒就要和某个历法绑定，因此NSDateComponents必须和NSCalendar一起使用，默认为公历。
- NSDateComponents除了像上面说的表示一个时间点外，还可以表示时间段，例如：两周，三个月，20年，7天，10分钟，50秒等等。时间段用于日历的计算，例如：获取当前历法下，三个月前的某个时间点。
- NSDateComponents返回的day, week, weekday, month, year这一类数据都是从1开始的。因为日历是给人看的，不是给计算机看的，从0开始就是个错误。

NSDateComponents实例化的方式

```    
//  先定义一个遵循某个历法的日历对象
NSCalendar *greCalendar = [[NSCalendar alloc] initWithCalendarIdentifier:NSGregorianCalendar];
//  通过已定义的日历对象，获取某个时间点的NSDateComponents表示，并设置需要表示哪些信息（NSYearCalendarUnit, NSMonthCalendarUnit, NSDayCalendarUnit等）
NSDateComponents *dateComponents = [greCalendar components:NSYearCalendarUnit | NSMonthCalendarUnit | NSDayCalendarUnit | NSHourCalendarUnit | NSMinuteCalendarUnit | NSSecondCalendarUnit | NSWeekCalendarUnit | NSWeekdayCalendarUnit | NSWeekOfMonthCalendarUnit | NSWeekOfYearCalendarUnit fromDate:[NSDate date]];
NSLog(@"year(年份): %i", dateComponents.year);
NSLog(@"quarter(季度):%i", dateComponents.quarter);
NSLog(@"month(月份):%i", dateComponents.month);
NSLog(@"day(日期):%i", dateComponents.day);
NSLog(@"hour(小时):%i", dateComponents.hour);
NSLog(@"minute(分钟):%i", dateComponents.minute);
NSLog(@"second(秒):%i", dateComponents.second);

//  Sunday:1, Monday:2, Tuesday:3, Wednesday:4, Friday:5, Saturday:6
NSLog(@"weekday(星期):%i", dateComponents.weekday);

//  苹果官方不推荐使用week
NSLog(@"week(该年第几周):%i", dateComponents.week);
NSLog(@"weekOfYear(该年第几周):%i", dateComponents.weekOfYear);
NSLog(@"weekOfMonth(该月第几周):%i", dateComponents.weekOfMonth); 

```


