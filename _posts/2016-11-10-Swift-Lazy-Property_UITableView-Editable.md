---
layout: post
title: Swift - 关于Lazy Stored Properties的Mutable设置以及UITableView的编辑状态及刷新
---
## 关于 Lazy Stored Properties 的一些细节 

```
lazy var tableData:[[String:String]]? = {
//当使用var关键字时则tableData为mutable，若使用let关键字时则tableData为immutable
        var tableData = [["title":"修改支付密码成功","message":"尊敬的刘世轶用户，您于2016-32-30 10:32:06成功修改了支付密码。如果非本人操作，请联系客服400-8282-255","timeStamp":"2016-08-30 12:22:43"],["title":"修改支付密码成功","message":"尊敬的刘世轶用户，您于2016-32-30 10:32:06成功修改了支付密码。如果非本人操作，请联系客服400-8282-255","timeStamp":"2016-08-30 12:22:43"],["title":"修改支付密码成功","message":"尊敬的刘世轶用户，您于2016-32-30 10:32:06成功修改了支付密码。如果非本人操作，请联系客服400-8282-255","timeStamp":"2016-08-30 12:22:43"]]

        return tableData
    }()
```

## 如何对UItableView进行编辑（滑动cell进行`插入`，`删除`等操作）以及局部更新
第一步，重载tableView(tableView:canEditRowAtIndexPath)->Bool函数,打开支持模式。

```
override func tableView(tableView: UITableView, canEditRowAtIndexPath indexPath: NSIndexPath) -> Bool {// let the controller to know that able to edit tableView's rowreturn true }
```

第二步，重载func tableView(tableView:editActionsForRowAtIndexPath:indexPath) -> [UITableViewRowAction]? 此处可定制设置向左滑动后出现的按钮（比如 `more`，`delete`），如果不重载此函数，则默认只有删除按钮（并且动作的响应要在重载函数tableView(tableView:commitEditingStyle:editingStyle:forRowAtIndexPath:)来实现）。

```
func tableView(tableView: UITableView, editActionsForRowAtIndexPath indexPath: NSIndexPath) -> [UITableViewRowAction]? {
        let moreAction: UITableViewRowAction = UITableViewRowAction(style: UITableViewRowActionStyle.Default, title: "Cancel", handler:{(tableViewRowAction:UITableViewRowAction!, index:NSIndexPath!) -> Void in
            print("Cancel")
            tableView.reloadRowsAtIndexPaths([indexPath], withRowAnimation: .None)  //重新加载某些cell（恢复或者刷新这些cell等）
        })
        moreAction.backgroundColor = UIColor.lightGrayColor()

        let deleteAction: UITableViewRowAction = UITableViewRowAction(style: UITableViewRowActionStyle.Default, title: "Delete", handler: {(tableViewRowAction:UITableViewRowAction!, index:NSIndexPath!) -> Void in
            print("Delete")
            self.tableData?.removeAtIndex(indexPath.section)
            tableView.deleteSections(NSIndexSet.init(index: indexPath.section), withRowAnimation: .Fade) //去掉某些section
//            tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: .Fade)  //去掉某些cell
        })
        deleteAction.backgroundColor = UIColor.redColor()

        return [deleteAction, moreAction] //包含两个按钮
    }
```

注意NSIndexSet的用法：

```
public class NSIndexSet : NSObject, NSCopying, NSMutableCopying, NSSecureCoding {
    
    public init(indexesInRange range: NSRange)  //indexset可以包含多个index
    public init(indexSet: NSIndexSet)
    
    public convenience init(index value: Int)  //也可以包含一个index

	 ... ...
}
```


