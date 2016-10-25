---
layout: post
title:  Learning Notes -  Mac OS Menu Programing Concepts
---
## The Struture of Mac OS Menu
Menus are constructed from three classes:

### Menu bar 菜单栏
菜单栏位于屏幕的最顶部，是Mac UI最立刻让人认知的部分。一直存在于Mac OS系统
> The menu bar at the top of the screen is the most instantly recognizable part of the Mac user interface. It has been there since the original Mac in 1984 and, although the look has changed several times, is still there on a modern OS X system. 

### Dock Menus
Providing a dock menu is very easy. When you create an application nib file, the file’s owner will be an NSApplication instance. This has three outlets, one of which is called dockMenu. Any menu you connect to this outlet will automatically be shown in the dock.
```
- (NSMenu*)applicationDockMenu: (NSApplication*)sender;
```

### Pop-Up Menus
