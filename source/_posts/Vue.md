---
title: Vue
date: 2022-10-23 10:12:57
summary: Vue
tags: [Vue]
categories: [Vue]
---

# vue

## 1.基本原理

在初始化实例中，会创建一个Observe对象，在这个对象中会通过Object.defineProperty（vue3 使用的是 proxy）来监视和劫持data中所有层级的属性，它还会为每个属性定义一个响应式的getter和setter，在响应式getter中会创建一个dep对象和watcher对象，dep对象则是收集使用到该属性的所有watcher，而watcher对象相当于对该属性进行订阅。那么在初始化模板编译的时候，响应式的getter会被调用，dep对象收集到相关依赖；之后在属性更新时，响应式的setter会通过dep对象向每个watcher通知，watcher就会执行更新函数进行更新视图。
