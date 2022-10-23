---
title: Vue
date: 2022-10-23 10:12:57
summary: Vue
tags: [Vue]
categories: [Vue]
---

# vue

## 1.基本原理

在初始化实例中，会有一个initData()方法被调，它遍历data中的每个属性，vue2使用Object.defineProperty转为getter和setter（vue3使用proxy），而后创建watcher对象，目的是记录使用到同一个属性的不同watcher， 为data中的每个属性定义一个响应式的getter和setter，在响应式的getter中，会创建一个叫dep的依赖对象，记录每个使用到属性的watcher；而当setter被调用时，会由dep通知相应的watcher执行更新函数更新视图。
