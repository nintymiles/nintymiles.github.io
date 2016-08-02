---
layout: post
title:  Learning Notes - A few of Swift Initializers
---

## Default Initializer
Swift provides a **default** initializer **for any structure or class** that provides **default** values for **all of its properties** and does **not provide** at least **one initializer** itself. 

The default initializer simply creates a new instance with all of its properties set to their default values.

## Structure's **Memberwise** Initializers (Structure额外拥有默认成员相关初始化方法）
Structure types **automatically** receive a ***memberwise*** initializer if they do not define any of their own custom initializers. 

**Unlike** a **default** initializer, the structure **receives a memberwise initializer** even if it has stored properties that **do not have default values**.




