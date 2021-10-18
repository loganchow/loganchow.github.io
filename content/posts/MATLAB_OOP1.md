---
title: "MATLAB 面向对象编程 小结1"
date: 2021-10-18T20:11:20+08:00
draft: true
tags: ["面向对象"，"OOP","MATLAB"]
categories: ["编程"]
showToc: true
TocOpen: false
disableShare: true
---

### 文件组织

##### \+ 号

\+ 号开头的文件夹代表 package

#### @ 号

@ 号开头的文件夹代表 class

### 类文件夹内的类定义

- 表示类定义的文件名和 `@` 文件夹名字要一样
- 类中方法的实现可以写成单独的函数文件，类体内仅作声明（去掉 `function` 关键词）
- 基类的声明尽量继承自 `handle` 超类
  - `handle` 句柄，其实就是类指针，传指针效率肯定高于传值
- 除非项目内容很多，其实 MATLAB OOP 的效率不高，主要归咎于 MATLAB 的函数入栈机制。因此在保证代码的可维护性和可读性情况下，尽量直接用表达式和 built-in function
- 类方法尽量也使用向量化的函数


### 常用语句
1. 类成员的属性
   ```matlab
    properties(SetAccess = private/protected/public)
         
    end
   ```
2. 类属性的set方法
   ```m
   `function obj = set.ClassName(obj, PropertyContent)
        if (strcmpi(PropertyContent,'允许值'))
            obj.PropertyName = PropertyContent
        else
            error("Invalid");
        end
   end
   ```

3. 类函数
4. 