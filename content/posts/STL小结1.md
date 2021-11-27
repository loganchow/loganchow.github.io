---
title: "STL小结1"
date: 2021-10-16T23:46:20+08:00
draft: true
tags: ["STL"]
categories: ["C++"]
showToc: true
TocOpen: false
disableShare: true
---

## 顺序容器
可以顺序访问(`std::iterator`)



| 容器                | 存储结构                         | 查改         | 增删             |
| ------------------- | -------------------------------- | ------------ | ---------------- |
| `std::vector`       | 数组，连续内存空间               | 随机访问     | 除尾部之外都很慢 |
| `std::deque`        | 多个缓冲区相接，由中央控制器管理 | 随机访问     |                  |
| `std::list`         | 双向链表                         | 不能随机访问 | 很快             |
| `std::forward_list` | 单向链表                         | 不能随机访问 | 很快             |



## 关联容器
随机访问

| 容器            | 存储结构 | 查改 | 增删 |
| --------------- | -------- | ---- | ---- |
| `std::map`      |          |      |      |
| `std::set`      |          |      |      |
| `std::multimap` |          |      |      |
| `std::multiset` |          |      |      |



## 适配器

一共三种

`std::stack`

`std::queue`

`std::priority_queue`

