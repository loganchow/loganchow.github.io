---
title: "ROS学习笔记2"
date: 2022-01-03T16:18:53+08:00
draft: false
tags: ["ROS"]
categories: ["智能驾驶"]
showToc: true
TocOpen: false
disableShare: true
---

> 记录一次在 ROS 学习过程中遇到的问题

## `message_filter` 的使用方法
#### 问题背景
> 现有一节点需要获取两不同传感器的数据，需要同时订阅两个话题，并将数据整合为一个 JSON 格式的数据帧发送至指定地址的指定端口；要求两帧数据时间戳尽量接近，socket 发送频率不低于 5Hz。

#### 问题剖析
由于数据包不大，且发送频率无上限，故使用 UDP 即可，通过 socket 实现很容易。 JSON 的处理也有第三方库可用。问题关键是：
1. 怎么保证 JSON 包数据的来源话题尽可能时间上接近
2. 什么时候调用 socket 把数据发出去
##### 解决方法一：`ros::MultiThreadedSpin()` + `pthread::rwlock`
定义 JSON 数据包为全局变量，分别订阅两个话题，使用两个回调函数，在其中一个回调函数结束写入消息数据时，为全局变量设置读写锁；而另一个回调函数先解锁，再写入消息，再加锁，依次类推。

定义套接字为全局变量，在其中一个回调函数中调用 `sendto()` 方法发送 JSON 包。

使用`ros::MultiThreadedSpin()` 来分别调用两话题的回调函数。
##### 解决方法二：`message_filter::Subscriber` + `message_filters::sync_policies::ApproximateTime`
使用 `message_filter::Subscriber` 同时订阅两个话题，使用 `ApproximateTime` 作为消息订阅的策略，实现两话题的消息时间尽可能接近。

使用 `registerCallback` 来定义一个总的回调函数来处理两个话题的数据。

#### 实操问题单: `boost::bind()` 方法的问题
按照教程订阅完话题，设置完消息过滤策略后，设置回调函数时，按照教程写下以下代码：
```cxx
// 回调函数声明
void ROSHandler::callbackSocket(const glosa::qt_GUIConstPtr& qtSource, const msgs_record::glosaConstPtr& msgSource);
...
// 绑定回调函数
sync.registerCallback(boost::bind(&ROSHandler::callbackSocket, _1, _2));
```

编译时遇到如下错误(节选)：
```
/usr/include/boost/bind/bind.hpp:69:37: error: ‘void (ROSHandler::*)(const glosa::qt_GUI_<std::allocator<void> >&, const msgs_record::glosa_<std::allocator<void> >&)’ is not a class, struct, or union type
     typedef typename F::result_type type;
                                     ^
```

编译错误提示原文巨长，不知其所以然也。

后查阅资料与论坛，发现 `boost::bind` 对于绑定**类的成员函数**需要占用一个 `placeholder` 来传入对象指针，因此，添加 `this` 指针来指向对象本体，代码修改如下：
```cxx
sync.registerCallback(boost::bind(&ROSHandler::callbackSocket, this, _1, _2));
```
编译通过。

或者创建指向对象的类成员指针变量 `obj_` 亦可达到同样效果。
```cxx
ROSHandler::ROSHandler() {
    ROSHandler* obj_ = new ROSHandler();
}
ROSHandler::~ROSHandler() {
    delete obj_;
}
...
sync.registerCallback(boost::bind(&ROSHandler::callbackSocket, obj_, _1, _2));
```