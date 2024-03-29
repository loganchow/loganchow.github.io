---
title: "MATLAB 面向对象编程 小结2"
date: 2021-11-27T15:31:23+08:00
draft: false
tags: ["面向对象","OOP","MATLAB"]
categories: ["编程"]
showToc: true
TocOpen: false
disableShare: true
---

### 类的组合
类的组合通过在组合类的构造函数中直接调用子类的构造函数，并将生成的对象作为组合类的成员加入到 `properties` 中。

### 类的继承
在 MATLAB 中，父类叫做超类。子类继承超类首先需要声明继承。
接着需要在构造函数中用`@`调用超类的构造函数来创造子类的成员与方法。

```matlab
classdef A < B
    methods
        function obj = A(~)
            obj@B(~);
        end
    end
end
```

### MATLAB 性能优化
#### 内存角度
MATLAB 中基本单元是矩阵（向量），在内存中是连续存储的，类似于数组。但是 MATLAB 是列优先级的。

为了节约分配内存的开销，应该提前分配好矩阵的空间，并同时声明好所存储的变量的类型。例如：
```matlab
a = zeros(500, 100, 'single');
```
还要一些其他的注意事项：
* 避免创建临时的数据副本。避免创建临时的数据副本，使用嵌套函数来传递参数。
  * 因为 MATLAB 有类似常量池的概念。即常量数据右值一旦生成就一直保存在内存中。观察以下代码：
    ```matlab
    a = 1;
    b = a;
    ```
    MATLAB 不会创建一个新的 `b` 变量，而是让 `b` 数值句柄指向 `1` 的内存空间。此时进行下列操作：
    ```matlab
    b = 2;
    ```
    此时会创建一个新的常量为 `2`，并让 `b` 指向这个新的内存空间。而 `a` 依然指向 `1` 的变量空间。
* 定期回收内存。把大数据、大变量保存到文件中，并使用 `matfile` 函数去访问 `mat` 文件中的变量，而不必将文件全部加载到内存中；使用 `pack` 函数对 MATLAB 的内存空间进行整理。

#### 向量化运算
老生常谈了，少用`for`循环，多用内置函数。
> 例子：消除冗余元素
> 有许多不同的方法可以用来查找向量的冗余元素。一种方法涉及到函数 `diff`。在对向量元素进行排序后，当您对该向量使用 `diff` 函数时，相等的邻接元素会产生零项。因为 `diff(x)` 生成的向量的元素数比 `x` 少一个，所以您必须添加一个不等于该集中的任何其他元素的元素。`NaN` 始终满足此条件。最后，您可以使用逻辑索引来选择该集中的唯一元素：
```matlab
x = [2 1 2 2 3 1 3 2 1 3];
x = sort(x);
difference  = diff([x,NaN]);
y = x(difference~=0)
```

#### 特例：动态编译 JIT 与向量化的对比

考察以下代码：
```matlab
clear;
y = [3; 4; 1; 10; 9; 9; 4; 2; ];
tic
m = length(y);
Y = zeros(m, 10);
for i = 1:m
    Y(i, y(i)) = 1;
end
toc
```

`历时 0.005438 秒。`

```matlab
clear("Y","m","i");
tic
Y = full(sparse(1:length(y), y, ones(length(y),1)));
toc
```

`历时 0.021157 秒。`

**结论**

按照一般性的认知，第二段代码采用了向量化的方法，与第一段使用 `for` 循环的代码相比，应当拥有更好的性能。但实际上，第一段代码的耗时是第二段的四分之一。原因是 MATLAB 会针对某些循环语句采用 JIT 优化，此时在编译层面上对代码的性能进行了优化，因此运行速度反超了向量化代码。[^1]

同时这也解释了为什么每当我们打开一些旧代码， MATLAB 会在 `clear all` 代码下方打波浪线，并提示这样写会降低性能。因为 `clear` 只会清理工作区的变量，而 `clear all` 同时也会清理掉已经编译过的代码，使得下次运行到可加速的代码段时需要重新编译，这是一笔不小的性能开销。

MATLAB JIT 是从 R2015a 版本开始附带的，打开自己 MATLAB 的 JIT 与 加速器的方法是：
```matlab
feature JIT on
feature accel on
```


[^1]: [Run Code Faster With the New MATLAB Execution Engine](https://blogs.mathworks.com/loren/2016/02/12/run-code-faster-with-the-new-matlab-execution-engine/)