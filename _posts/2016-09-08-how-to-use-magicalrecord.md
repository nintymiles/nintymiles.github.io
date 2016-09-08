---
layout: post
title:  Learning Notes - 使用 MagicalRecord
---

## MagicalRecord的一般初始化
在AppDelegate `- applicationDidFinishLaunching: withOptions: method`中设置

```
//日志级别
[MagicalRecord setLoggingLevel:MagicalRecordLoggingLevelDebug];

//指定存储文件名
[MagicalRecord setupCoreDataStackWithAutoMigratingSqliteStoreNamed:kHanziStoreName];

//setupCOreDataStack...方法族有一系列方法，根据便利调用，无太多区别。
```

> 默认数据文件使用的路径为NSApplicationSupport！十分合理

## 如何初始化已经预置数据（pre-populated)的数据文件（xxx.sqlite）
判断`NSApplicationSupport`目录中是否存在数据文件，如果不存在，则从application bundle等处copy到`NSApplicationSupport`目录。

```
- (void) copyDefaultStoreIfNecessary;
{
    NSFileManager *fileManager = [NSFileManager defaultManager];
    
    NSURL *storeURL = [NSPersistentStore MR_urlForStoreName:kHanziStoreName];
    
    // If the expected store doesn't exist, copy the default store.
    if (![fileManager fileExistsAtPath:[storeURL path]])
    {
        NSString *defaultStorePath = [[NSBundle mainBundle] pathForResource:[kHanziStoreName stringByDeletingPathExtension] ofType:[kHanziStoreName pathExtension]];
        
        if (defaultStorePath)
        {
            NSError *error;
            BOOL success = [fileManager copyItemAtPath:defaultStorePath toPath:[storeURL path] error:&error];
            if (!success)
            {
                NSLog(@"Failed to install default store");
            }
        }
    }
    
}
```
## MagicalRecord如何生成entity并存储

```
+(void)loadStrokesDataAndImportIntoCoreData{
    NSDictionary *preImportData = [StrokeDataParser loadStrokesData];
    
    NSMutableArray *arrangedImportData=[NSMutableArray array];
    
    //获取默认的ManagedContext
    NSManagedObjectContext *defaultContext=[NSManagedObjectContext MR_defaultContext];
    
    for(NSString *key in preImportData.allKeys){
        //entity实体在MagicalRecord中的生成方式
        StrokeData *theStrokeData=[StrokeData MR_createEntity];
        theStrokeData.hanziCharacter=key;
        theStrokeData.pinyinAcronym=pinyinAcronym;
        theStrokeData.strokesData=[strokesData componentsJoinedByString:@";"];
        
    }
    
    //显式调用持久化方法，存储数据。 ps. 自动存储没有成功？如何设置？
    [defaultContext MR_saveToPersistentStoreAndWait];
    
    //通过NSDictionary和entity直接匹配导入的方法没有成功
    
    NSArray *standardCharset=[StrokeDataParser loadTCCharacterSetFromTxt];
    for(NSString *hanziCharset in standardCharset){
        NSString *pinyin=[hanziCharset toPinyin];
        NSString *pinyinAcronym=[hanziCharset toPinyinAcronym];
        
        HanziSet *hanziEntity=[HanziSet MR_createEntity];
        hanziEntity.hanziName=hanziCharset;
        hanziEntity.pinyin=pinyin;
        hanziEntity.pinyinAcronym=pinyinAcronym;
    }
    
    [defaultContext MR_saveToPersistentStoreAndWait];
```

## 如何生成NSFetchedResultsController？
直接使用entity对象可以生成各种条件的NSFetchedResultsController。

```
NSManagedObjectContext *defualtContext=[NSManagedObjectContext MR_defaultContext];

//从entity对象直接生成fetchedcontroller，比较简洁    
    _fetchedResultsController=[HanziSet MR_fetchAllSortedBy:@"pinyin" ascending:YES withPredicate:nil groupBy:@"pinyin" delegate:nil inContext:defualtContext];
```

> `NSFetchedResultsSectionInfo`定义为protocol,使用时需要用`NSObject<NSFetchedResultsSectionInfo> *`或者`id<NSFetchedResultsSectionInfo>` 方式获取NSFetchedResultsController的section数据。


## 使用 MagicalRecord 导入（importing）数据(没有成功)
1. 一个 NSDictionary 只能映射一个具体 entity， NSDcitionary 中使用 key－value 方式包含一个 entity 的 attributeName-attributeValue .
2. 若将单个表（entity list）的数据完全导入则需要构造 NSArray[NSDictionary]

