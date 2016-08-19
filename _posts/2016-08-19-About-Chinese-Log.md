## ［Objective-C］如何从 NSArray 和 NSDictionary 中直接 print 中文字符？
下面这种方式较为简洁

```
#import <Foundation/Foundation.h>

@interface NSArray (Log)

@end

@interface NSDictionary (Log)

@end
```


```
@implementation NSArray (Log)

-(NSString *)descriptionWithLocale:(id)locale {
    NSMutableString *retString = [[NSMutableString alloc]init];
    
    [retString appendString:@"\n(\n"];
    [self enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        if (idx != 0) {
            [retString appendFormat:@",\n"];
        }
        [retString appendFormat:@"\t%@",obj];
    }];
    [retString appendString:@"\n)\n"];
    [retString appendFormat:@"Total %zd item(s).",self.count];
    return retString;
}

@end

@implementation NSDictionary (Log)

-(NSString *)descriptionWithLocale:(id)locale {
    __block NSInteger count = 0;
    NSMutableString *retString = [[NSMutableString alloc]init];
    
    [retString appendString:@"\n{\n"];
    [self enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
        [retString appendFormat:@"\t%@ = %@;\n",[self checkNSStringWithObj:key],[self checkNSStringWithObj:obj]];
        count ++;
    }];
    [retString appendString:@"}\n"];
    [retString appendFormat:@"Total %zd Key-Value(s).",count];
    return retString;
}

-(NSString *)checkNSStringWithObj:(id)obj {
    if ([obj isKindOfClass:[NSString class]]) {
        return [NSString stringWithFormat:@"\"%@\"",obj];
    }
    else {
        return [NSString stringWithFormat:@"%@",obj];
    }
}

@end 
```

另一种方式

```
@implementation NSArray (UnicodeTransfer)
+ (void)load {
    // 该方法会在加载这个类的时候执行（APP启动时会加载，只执行一次）
    // 此处交换descriptionWithLocale:与自己写的my_descriptionWithLocale:的方法指针
    [self exchangeSelector:@selector(descriptionWithLocale:) andNewSelector:@selector(my_descriptionWithLocale:)];
}

- (NSString *)my_descriptionWithLocale:(id)locale {
    NSString *desc = [self my_descriptionWithLocale:locale];
    desc = [self replaceUnicode:desc];
    return desc;
}

- (NSString *)replaceUnicode:(NSString *)unicodeStr {
    NSString *tempStr1 = [unicodeStr stringByReplacingOccurrencesOfString:@"\\u" withString:@"\\U"];
    NSString *tempStr2 = [tempStr1 stringByReplacingOccurrencesOfString:@"\"" withString:@"\\\""];
    NSString *tempStr3 = [[@"\"" stringByAppendingString:tempStr2] stringByAppendingString:@"\""];
    NSData *tempData = [tempStr3 dataUsingEncoding:NSUTF8StringEncoding];
    NSString* returnStr = [NSPropertyListSerialization propertyListFromData:tempData
                                                           mutabilityOption:NSPropertyListImmutable
                                                                     format:NULL
                                                           errorDescription:NULL];
    return [returnStr stringByReplacingOccurrencesOfString:@"\\r\\n" withString:@"\n"];
}
@end
```

