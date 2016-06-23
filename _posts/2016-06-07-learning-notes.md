---
layout: post
title: Learning notes - 计数排序的理解
---

## 计数排序的理解
1. 先找出数组中的最大元素，然后以最大元素值加1建立计数数组
2. 将实际元素在计数数组作为索引值，并将此位置存储出现次数计数
3. 把从头到尾的索引值的出现次数一次当前一个和后一个相加（从1开始），然后得到每个元素的其实际索引位置。最后在1:1的新数组中将是实际值赋值到实际索引位置。
4. 因为实际计数每个数值的出现都计算在内，而数组索引从零开始，所以计算出的索引值实际使用时都要减1？

## Counting sort
To understand the algorithm let's walk through a small example.

Consider the array: `[ 10, 9, 8, 7, 1, 2, 7, 3 ]`

### Step 1:

The first step is to count the total number of occurrences for each item in the array. The output for the first step would be a new array that looks as follows:


```
Index 0 1 2 3 4 5 6 7 8 9 10
```

Here is the code to accomplish this:


```swift
  let maxElement = array.maxElement() ?? 0
  
  var countArray = \[Int](count: Int(maxElement + 1), repeatedValue: 0)
  for element in array {
     countArray[element] += 1
  }
```



### Step 2:

In this step the algorithm tries to determine the number of elements that are placed before each element. Since, you already know the total occurrences for each element you can use this information to your advantage. The way it works is to sum up the previous counts and store them at each index.

The count array would be as follows:



```
Index 0 1 2 3 4 5 6 7 8 9 10
Count 0 1 2 3 3 3 3 5 6 7 8
```



The code for step 2 is:


```swift
  for index in 1 ..< countArray.count {
    let sum = countArray[index] + countArray[index - 1]
    countArray[index] = sum
  }
```


### Step 3:

This is the last step in the algorithm. Each element in the original array is placed at the position defined by the output of step 2. For example, the number 10 would be placed at an index of 7 in the output array. Also, as you place the elements you need to reduce the count by 1 as those many elements are reduced from the array.

The final output would be:


```
Index  0 1 2 3 4 5 6 7
Output 1 2 3 7 7 8 9 10
```


Here is the code for this final step:



```swift
var sortedArray = \[Int](count: array.count, repeatedValue: 0)
  for element in array {
    countArray[element] -= 1
    sortedArray[countArray[element]] = element
  }
  return sortedArray
```



## Performance

The algorithm uses simple loops to sort a collection. Hence, the time to run the entire algorithm is **O(n+k)** where **O(n)** represents the loops that are required to initialize the output arrays and **O(k)** is the loop required to create the count array.

The algorithm uses arrays of length **n + 1** and **n**, so the total space required is **O(2n)**. Hence for collections where the keys are scattered in a dense area along the number line it can be space efficient.