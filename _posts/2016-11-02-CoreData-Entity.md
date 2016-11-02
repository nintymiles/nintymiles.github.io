---
layout: post
title: 关于Swift Coredata entity实体类及其生成方式
---

## CoreData Entity Class
***The best alternative*** to key-value coding is to create NSManagedObject subclasses for each entity in your data model.

Xcode can **automatically generate** the subclass for you. Make sure you still have xxx.xcdatamodeld open, and go to Editor\Create NSManagedObject Subclass.... Select the data model and then the entities in the next two dialog boxes, then select ***Swift*** as the language option in the final box. If you’re asked, say No to creating an Objective-C bridging header. Click Create to save the file. 

Xcode generated two Swift files for you, one called StrokeData(**entity name**).swift and a second called StrokeData(entity name)+CoreDataProperties.swift.

```
#import "StrokeData.h"

@implementation StrokeData

// Insert code here to add functionality to your managed object subclass

@end
```

```
#import "StrokeData+CoreDataProperties.h"

extension StrokeData {

    @nonobjc public override class func fetchRequest() -> NSFetchRequest {
        return NSFetchRequest(entityName: "StrokeData");
    }  //必须重载此方法？

    @NSManaged public var hanziCharacter: String?
    @NSManaged public var pinyinAcronym: String?
    @NSManaged public var strokesData: String?

}
```

> 由于coredata framework本身也在变化，因而entity的生成最好通过xocde automatic generating方式来进行。

