---
title: 算法图解-chapter2
date: 2022-07-20 17:39:30
summary: 算法图解-chapter2
tags: [算法图解]
categories: [算法图解]
---

# 选择排序

## 数组

###### 排序是相连的

1. 数组不定长的缺点：在新增元素时，需重新寻找空间
2. 数组定长的缺点：额外多余的位置可能用不上，浪费内存

###### 查找元素：

| 读取 | 插入 | 删除 |
| :--: | :--: | :--: |
| O(1) | O(n) | O(n) |

比链表效率高

## 链表

###### 元素可存储在内存的任何地方，通过指向来连接所有元素

###### 查找元素：

| 读取 | 插入 | 删除 |
| :--: | :--: | :--: |
| O(n) | O(1) | O(1) |

## 代码

```tsx
// 数组的选择排序

let arr = [5, 3, 6, 2, 10]
let arr1 = [5, 3, 6, 2, 10]

function findIndex<T>(arr: T[], toLarge: boolean): number {
  let target = arr[0]
  let target_index = 0
  for (let index = 0; index < arr.length; index++) {
    if ((toLarge && arr[index] < target) || (!toLarge && arr[index] > target)) {
      target = arr[index]
      target_index = index
    }
  }
  return target_index
}

function selectionSort(arr: number[], toLarge = true): number[] {
  let newArr: number[] = []
  let length = arr.length
  while (length != 0) {
    let target_index = findIndex(arr, toLarge)
    newArr.push(arr.splice(target_index, 1)[0])
    length = arr.length
  }
  return newArr
}

console.log(selectionSort(arr)) //  [ 2, 3, 5, 6, 10 ]
console.log(selectionSort(arr1, false)) //  [ 10, 6, 5, 3, 2 ]

```

