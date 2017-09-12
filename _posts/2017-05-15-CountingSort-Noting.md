# 

```
import Foundation


// *** Simple but inefficient version of quicksort ***

func quicksort<T: Comparable>(_ a: [T]) -> [T] {
  guard a.count > 1 else { return a }

  let pivot = a[a.count/2]
  let less = a.filter { $0 < pivot }
  let equal = a.filter { $0 == pivot }
  let greater = a.filter { $0 > pivot }

  // Uncomment this following line to see in detail what the
  // pivot is in each step and how the subarrays are partitioned.
  //print(pivot, less, equal, greater)  return quicksort(less) + equal + quicksort(greater)

  return quicksort(less) + equal + quicksort(greater)
}

let list1 = [ 10, 0, 3, 9, 2, 14, 8, 27, 1, 5, 8, -1, 26 ]
quicksort(list1)



// *** Using Lomuto partitioning ***

/*
  Lomuto's partitioning algorithm.

  The return value is the index of the pivot element in the new array. The left
  partition is [low...p-1]; the right partition is [p+1...high], where p is the
  return value.
*/
func partitionLomuto<T: Comparable>(_ a: inout [T], low: Int, high: Int) -> Int {
  let pivot = a[high]

  var i = low
  for j in low..<high {
    if a[j] <= pivot { //比支点数值小的，和低位索引处互换值
      (a[i], a[j]) = (a[j], a[i])  //利用tuple批量赋值，相当于i，j处互换位置
      i += 1   //如果比支点数值小，则i（低位索引）值加1
    }
  }

  (a[i], a[high]) = (a[high], a[i])
  return i
}

var list2 = [ 10, 0, 3, 9, 2, 14, 26, 27, 1, 5, 8, -1, 8 ]
partitionLomuto(&list2, low: 0, high: list2.count - 1)
list2

func quicksortLomuto<T: Comparable>(_ a: inout [T], low: Int, high: Int) {
  if low < high {
    let p = partitionLomuto(&a, low: low, high: high)
    quicksortLomuto(&a, low: low, high: p - 1)
    quicksortLomuto(&a, low: p + 1, high: high)
  }
}

quicksortLomuto(&list2, low: 0, high: list2.count - 1)



// *** Hoare partitioning ***

/*
  Hoare's partitioning scheme.

  The return value is NOT necessarily the index of the pivot element in the
  new array. Instead, the array is partitioned into [low...p] and [p+1...high],
  where p is the return value. The pivot value is placed somewhere inside one
  of the two partitions, but the algorithm doesn't tell you which one or where.
*/
func partitionHoare<T: Comparable>(_ a: inout [T], low: Int, high: Int) -> Int {
  let pivot = a[low]
  var i = low - 1
  var j = high + 1

  while true {
    repeat { j -= 1 } while a[j] > pivot  //满足while条件时循环继续，否则退出
    repeat { i += 1 } while a[i] < pivot

    if i < j {
      a[i]
      a[j]
      swap(&a[i], &a[j])
      a[i]
      a[j]
    } else {
      return j
    }
  }
}

var list3 = [ 8, 0, 3, 9, 2, 14, 10, 27, 1, 5, 8, -1, 26 ]
partitionHoare(&list3, low: 0, high: list3.count - 1)
list3

func quicksortHoare<T: Comparable>(_ a: inout [T], low: Int, high: Int) {
  if low < high {
    let p = partitionHoare(&a, low: low, high: high)
    quicksortHoare(&a, low: low, high: p)
    quicksortHoare(&a, low: p + 1, high: high)
  }
}

quicksortHoare(&list3, low: 0, high: list3.count - 1)


```

