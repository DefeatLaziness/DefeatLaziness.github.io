---
title: 算法图解-chapter4
date: 2022-07-21 12:09:43
summary: 算法图解-chapter4 
tags: [算法图解]
categories: [算法图解]
---

# 快速排序

```tsx
// 快速排序
let arr = [3, 6, 9, 3, 2, 15, 67, 34, 2, 8]

function quickSort(arr: number[], toLarge = true): number[] {
  let length = arr.length
  if (length == 0) return []

  let firstNum = arr[0]
  let left: number[] = []
  let right: number[] = []

  for (let i = 1; i < length; i++) {
    if (arr[i] < firstNum) {
      if (toLarge) left.push(arr[i])
      else right.push(arr[i])
    } else {
      if (toLarge) right.push(arr[i])
      else left.push(arr[i])
    }
  }
  left = quickSort(left, toLarge)
  right = quickSort(right, toLarge)

  left.push(firstNum)
  let result = left.concat(right)

  return result
}

console.log(quickSort(arr)) // [ 2, 2,  3,  3,  6, 8, 9, 15, 34, 67]
console.log(quickSort(arr, false)) // [ 67, 34, 15, 9, 8, 6,  3,  3, 2, 2]

```

