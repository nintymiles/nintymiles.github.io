##CFArrayRef和NSArray
CFArrayRef和NSArray是toll-free bridged的，互用都没有问题。如下：

```
NSString *values[] = {@"hello", @"world"};
CFArrayRef arrayRef = CFArrayCreate(kCFAllocatorDefault, (void *)values, (CFIndex)2, NULL);
NSArray *array = (NSArray *)arrayRef;
NSData *data = [NSKeyedArchiver archivedDataWithRootObject:array]; 
```



