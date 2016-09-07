---
layout: post
title:  Learning Notes - About coredata／sqlite persistent file store path
---
## iOS 中使用 coredata 或 FMDB（sqlite） 等持久存储时的关注点
1. 数据文件可以存放在 `NSApplicationSupportDirectory` 目录中，如果存放在 `NSCachesDirectory` （／library/cache） 中则会面临系统清空释放空间时被删除的威胁
2. 数据文件放在 `NSDcoumentDirectory` 则违反苹果icoud data storage规范，发布会被拒。但是也可以采用在Application中设置禁止icloud备份的方式（iOS 5之后提供API支持），将数据文件放置在此目录中。

	```
	/** Apps must follow the iOS Data Storage Guidelines or they will 
	be rejected On launch and content download, your app stores xx.xx 
	MB, which does not comply with the iOS Data Storage Guidelines.       
	*/

	
	- (BOOL)addSkipBackupAttributeToItemAtURL:(NSURL *)URL {
	    assert([[NSFileManager defaultManager] fileExistsAtPath: [URL path]]);
	
	    NSError *error = nil;
	    BOOL success = [URL setResourceValue: [NSNumber numberWithBool: YES]
	                                  forKey: NSURLIsExcludedFromBackupKey error: &error];
	    if(!success){
	        NSLog(@"Error excluding %@ from backup %@", [URL lastPathComponent], error);
	    }
	    return success;
	}
	```
	
3. coredata 中用户数据备份支持在cloudkit中有相关API支持。

