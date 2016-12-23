---
layout: post
title: iOS中当点击回车键时光标如何自动跳到下一输入框
---
## iOS中当点击回车键时光标自动跳到下一输入框
假设有两个textfield，指定得了尕特，然后分别设置tag为101，102，在标准键盘时，如何通过return自动跳光标。代码如下：

```
-(BOOL)textFieldShouldReturn:(UITextField *)textField{
    [self textFieldSwitchNextWhenReturn:textField];

    return YES;
}

-(void)textFieldSwitchNextWhenReturn:(UITextField *)textField{
    NSUInteger tag = textField.tag;
    tag++;
    if(tag > 102){
        tag = 101;
    }

    UITextField *nextField=[self.view viewWithTag:tag];
    [nextField becomeFirstResponder];
}
``` 

