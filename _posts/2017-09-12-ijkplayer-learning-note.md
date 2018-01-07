---
layout: post
title: ijkPlayer 源代码学习
---

## 熟悉一条Git命令 
`git checkout -b branch` Create a new branch with the name branch

## List内容随机乱序（shuffle）

```extension MutableCollection where Index == Int {
	
	mutating func shuffleInPlace() {
		
		if count < 2 { return }
		//将当前位置和后面的随机位置对调
		for i in startIndex ..< endIndex - 1 {
			let j = Int(arc4random_uniform(UInt32(endIndex - i))) + i
			if i != j {
				swap(&self[i], &self[j])
			}
		}
	}
}
```

