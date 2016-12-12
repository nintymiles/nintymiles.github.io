## NSDateFormatrer的格式
First, your date format is wrong -- the second line should be [dateFormatter setDateFormat:@"yyyy-MM-dd'T'HH:mm:ss.000Z"]; -- that is, you need the ss. after the mm: but before the 000Z. This will give you the correct NSDate.

Next, you need to create a new date formatter with the format @"dd.M.yyyy" (or @"d.M.yyyy" if you want to remove the leading zero if the day is a single digit as well) and call stringFromDate: on that for the NSLog.

Just to recap, the whole chunk of code would be

```
NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
[dateFormatter setDateFormat:@"yyyy-MM-dd'T'HH:mm:ss.000Z"];
NSDate *theDate = [dateFormatter dateFromString:stringDate];

NSDateFormatter *printFormatter = [[NSDateFormatter alloc] init];
[printFormatter setDateFormat:@"dd.M.yyyy"];
NSLog(@"%@",[printFormatter stringFromDate:theDate]); 
```

实现例子：传入一个date string （`2016-12-12 23:59:12.0`），如何与当日比较？

```
-(Boolean)dateGreaterThanToday:(NSString*)dateString{
    NSDateFormatter * dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd HH:mm:ss.0"];
    NSDate *dateA = [dateFormatter dateFromString:dateString];
    NSDate *dateB = [NSDate date];
    NSTimeInterval timeInterval = [dateA timeIntervalSinceDate:dateB];
    return timeInterval > 0;
}
```

Some Apple Doc

```
let RFC3339DateFormatter = DateFormatter()
RFC3339DateFormatter.locale = Locale(localeIdentifier: "en_US_POSIX")
RFC3339DateFormatter.dateFormat = "yyyy-MM-dd'T'HH:mm:ssZZZZZ"
RFC3339DateFormatter.timeZone = TimeZone(forSecondsFromGMT: 0)
 
/* 39 minutes and 57 seconds after the 16th hour of December 19th, 1996 with an offset of -08:00 from UTC (Pacific Standard Time) */
let string = "1996-12-19T16:39:57-08:00"
let date = RFC3339DateFormatter.dateFromString(string)
```

