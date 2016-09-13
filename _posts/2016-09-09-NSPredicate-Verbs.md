---
layout: post
title:  Learning Notes - About NSPredicate
---

##NSPredicate(谓词)

Cocoa 提供了NSPredicate 用于指定过滤条件，谓词是指在计算机中表示计算真假值的函数，

它使用起来有点儿像SQL 的查询条件，主要用于从集合中分拣出符合条件的对象，也可以
用于字符串的正则匹配。首先我们看一个非常简单的例子，对谓词有一个认知。

```
#import <Foundation/Foundation.h>
@interface Person: NSObject{
int pid;
NSString *name;
float height;
}
-(void) setPid: (int) pid;
-(void) setName: (NSString*) name;
-(void) setHeight: (float) height;
-(int) pid;
-(NSString*) name;
-(float) height;
@end
@implementation Person
-(void) setPid: (int) p{
pid=p;
}
-(void) setName: (NSString*) n{
[n retain];
[name release];
name=n;
}
-(void) setHeight: (float) h{
height=h;
}
-(int) pid{
return pid;
}
-(NSString*) name{
return name;
}
-(float) height{
return height;
}
-(void) dealloc{
[name release];
[super dealloc];
}
@end

int main (int argc , const char * argv[]){
NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
//实例化三个Person，并放入数组。
NSMutableArray *array=[NSMutableArray arrayWithCapacity: 5];
Person *person1=[[Person alloc] init];
[person1 setPid: 1];
[person1 setName: @"Name1"];
[person1 setHeight: 168];
[array addObject: person1];
Person *person2=[[Person alloc] init];
[person2 setPid: 2];
[person2 setName: @"Name2"];
[person2 setHeight: 178];
[array addObject: person2];
Person *person3=[[Person alloc] init];
[person3 setPid: 3];
[person3 setName: @"Name3"];
[person3 setHeight: 188];
[array addObject: person3];
```
//创建谓词，条件是pid>1 并且height<188.0。其实谓词也是基于KVC 的，也就是如
果pid 在person 的成员变量xxx 中，那么此处要写成xxx.pid>1。

```
NSPredicate *pre = [NSPredicate predicateWithFormat:
@" pid>1 and height<188.0"];
int i=0;
for(;i<[array count];i++){
Person *person=[array objectAtIndex: i];
//遍历数组，输出符合谓词条件的Person 的name。
if ([pre evaluateWithObject: person]) {
NSLog(@"%@",[person name]);
}
}
[person1 release];
[person2 release];
[person3 release];
[pool release];
return 0;
}
Shell 窗口输出如下所示：
2011-04-01 16:51:18.382 Predicate[2400] Name2
```

我们看到创建谓词使用类方法`predicateWithFormat: (NSString*) format`，format 里的东西真的和SQL 的where 条件差不多。另外，参数format 与NSLog 的格式化模版差不多，如果1 和188.0 是传递过来的参数，你可以写成如下的形式：
`@"pid>%d and height<%f",1,188.0`

(1.) 逻辑运算符：AND、OR、NOT
这几个运算符计算并、或、非的结果。

(2.) 范围运算符：BETWEEN、IN
例：
@”pid BETWEEN {1,5}”
@"name IN {'Name1','Name2'}"

(3.) 占位符：
NSPredicate *preTemplate = [NSPredicate predicateWithFormat:@"name==$NAME"];
NSDictionary *dic=[NSDictionary dictionaryWithObjectsAndKeys:
@"Name1", @"NAME",nil];
NSPredicate *pre=[preTemplate predicateWithSubstitutionVariables: dic];
占位符就是字段对象里的key，因此你可以有多个占位符，只要key 不一样就可以了。

(4.) 快速筛选数组：
前面我们都是使用谓词逐个判断数组内的对象是否符合，其实数组本身有更为便捷的方法，
直接筛选出一个符合谓词的新数组。

```
NSPredicate *pre = [NSPredicate predicateWithFormat:@"pid>1"];
NSMutableArray *arrayPre=[array filteredArrayUsingPredicate: pre];
NSLog(@"%@",[[arrayPre objectAtIndex: 0] name]);
```

(5.) 字符串运算符：
BEGINSWITH、ENDSWITH、CONTAINS 分别表示是否以某字符串开头、结尾、包含。
他们可以与c、d 连用，表示是否忽略大小写、是否忽略重音字母（字母上方有声调标号）。
例：
@”name BEGINSWITH[cd] ‘He’”
判断name 是否以He 开头，并且忽略大小写、忽略重音字母。

(6.) LIKE 运算符：
LIKE 使用?表示一个字符，*表示多个字符，也可以与c、d 连用。
例：
@”name LIKE ‘???er*’” 与Paper Plane 相匹配。

(7.) SELF：
前面的数组中放的都是对象，如果数组放的都是字符串（或者是其他没有属性的类型），该
怎么写谓词呢？这里我们使用SELF。
例：

```
NSArray *arrays=[NSArray arrayWithObjects: @"Apple", @"Google", @"MircoSoft", nil];
NSPredicate *pre2 = [NSPredicate predicateWithFormat:@"SELF=='Apple'"];
```

(8.) 正则表达式：
NSPredicate 使用MATCHES 匹配正则表达式，正则表达式的写法采用international components for Unicode (ICU)的正则语法。
例：

```
NSString *regex = @"^A.+e$";//以A 开头，以e 结尾的字符。
NSPredicate *pre= [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];
if([pre evaluateWithObject: @"Apple"]){
printf("YES\n");
}else{
printf("NO\n");
} 
```

