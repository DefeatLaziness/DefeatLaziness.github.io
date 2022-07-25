---
title: 算法图解-chapter7
date: 2022-07-21 17:20:33
summary: 算法图解-chapter7
tags: [算法图解]
categories: [算法图解]
---

# 狄克斯特拉算法

##### 权重：图中的每条边都有关联数字，这些数字称为权重

##### 加权图: 带权重的图称为加权图

##### 非加权图: 不带权重的图称为加权图

##### 广度优先搜索: 适合用于计算非加权图的最短路径

##### 狄克斯特拉算法: 适合用于计算加权图（有向无环图）的最短路径，不能用于包含负权边的图。

##### 贝尔曼-福德算法: 在包含负权边的图中找出最短路径

#### 例子如下

![731658722220_.pic](/Users/kim/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/ece56dcd075e8c6770d7246233ebaaa4/Message/MessageTemp/9e20f478899dc29eb19741386f9343c8/Image/731658722220_.pic.jpg)

```tsx
// 狄克斯特拉算法

const graph = {
  start: {
    a: 6,
    b: 2,
  },
  a: {
    end: 1,
  },
  b: {
    a: 3,
    end: 5,
  },
  end: {},
} // 包含所有节点到其邻居的距离

//起点到所有节点到距离（除了邻居外都为无穷大）
const costs = {
  a: 6,
  b: 2,
  end: Number(Infinity),
}
// 所有节点到父元素（除了起点邻居外都为空）
const parents = {
  a: 'start',
  b: 'start',
  end: '',
}
// 记录已处理过的节点
let processed: string[] = []

function find_lowest_cost_node(costs: object) {
  let lowest_cost = Number(Infinity)
  let lowest_cost_node = ''

  for (const n in costs) {
    const cost: number = costs[n as keyof typeof costs]

    if (cost < lowest_cost && !processed.includes(n)) {
      lowest_cost = cost
      lowest_cost_node = n
    }
  }

  return lowest_cost_node
}

function findShortestPath() {
  let node = find_lowest_cost_node(costs)
  while (node != '') {
    let cost = costs[node as keyof typeof costs]
    let neighbors = graph[node as keyof typeof graph]
    for (const n in neighbors) {
      let new_cost = cost + neighbors[n as keyof typeof neighbors]
      if (costs[n as keyof typeof costs] > new_cost) {
        costs[n as keyof typeof costs] = new_cost
        parents[n as keyof typeof parents] = node
      }
    }
    processed.push(node)
    node = find_lowest_cost_node(costs)
  }

  let road: string[] = []
  let last = 'end'
  while (last != 'start') {
    road.push(parents[last as keyof typeof parents])
    last = parents[last as keyof typeof parents]
  }
  return road.join('->')
}

console.log(findShortestPath()) // a->b->start

```

