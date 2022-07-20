---
title: 算法图解-chapter1
date: 2022-07-20 17:11:23
summary: 算法图解-chapter1
tags: [算法图解]
categories: [算法图解]
---

# 算法简介

## 二分查找

```tsx
//  有序数组二分法查找

let arr: number[] = [4, 5]

function createArr(arr: number[]) {
  for (let index = 1; index <= 100; index++) {
    arr.push(index)
  }
}
createArr(arr)

// 基本使用
function dichotomy(arr: number[], ele: number) {
  let left = 0
  let right = arr.length - 1

  while (left <= right) {
    let mid = Math.floor((left + right) / 2)
    let midEle = arr[mid]
    if (midEle == ele) return `该元素对应的下标为: ${mid}`
    else if (midEle > ele) right = mid - 1
    else left = mid + 1
  }
  return '没找到该元素对应的下标'
}

console.log(dichotomy(arr, 66)) //  该元素对应的下标为: 65
console.log(dichotomy(arr, 12)) //  该元素对应的下标为: 11
console.log(dichotomy(arr, -1)) //  没找到该元素对应的下标

```



