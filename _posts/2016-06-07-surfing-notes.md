---
layout: post
title: Mac快速锁屏及其它
---

## Mac如何快捷锁屏的实现方式
Mac快速锁屏的设置步骤：

1. 在security&privacy->general中启用‘required password immediately after sleep or screen save launched
2. 使用command+option+power(eject)快捷键进入睡眠

> 注：ctrl＋shift＋power（eject）是休眠快捷键

## Jekyll post如何使用草稿模式
草稿是没有日期的文章。它们是你还在创作中而暂时不想发表的文章。想要开始使用草稿，你需要在网站根目录下创建一个名为 _drafts 的文件夹（如在目录结构章节里描述的），并新建你的第一份草稿：


```
|-- _drafts/
|   |-- a-draft-post.md
```

为了预览你拥有草稿的网站，运行 jekyll serve 或者带有 --drafts 配置选项的 jekyll build。此两种方法皆会将 Time.now 的值赋予草稿文章，作为其发布日期，所以你将看到草稿文章作为最新文章被生成。