---
title: "ROS 学习笔记1"
date: 2020-06-18T22:24:42+08:00
draft: false
tags: ["ROS"]
categories: ["智能驾驶"]
showToc: true
TocOpen: false
disableShare: true
disableHLJS: false
---

> 个人学习过程中的笔记，如有不足敬请指正。

# 1. ROS 的基本概念
ROS (机器人操作系统) 简单来说就是提供了通讯框架，工具和现成的功能的平台，我们最主要用的是通讯功能和工具套件。通讯功能主要包括同步 RPC 通讯 (Service)和异步通讯 (Topic)；工具包括 rviz, Gazebo 等。ROS 的分布式框架设计方便智能驾驶和机器人这种多机通讯，数据流分散的场合。

## 1.1 计算图级
其实就是 ROS 通讯的拓扑网络，是一种点对点的通讯形式。

### 1. Node
节点，进程，或者说功能单元，是代码模块的最小单位，或者说是最小的单个可执行文件。

ROS 的通讯其实就是节点和节点之间 (不一定直接)的通信。

一个节点既可以实现订阅，也可以实现发布，也能同时实现。

### 2. Master
~~主人？~~ 节点管理器，所有的节点都要找它进行注册才能被其他节点发现。因此要先启动节点管理器 (`roscore`)再启动节点 (`rosrun`)。

### 3. Message
消息，节点之间通讯的内容或媒介，可以自定义格式，就和 C 语言里的结构体一样，只不过 Message 更像是具体的对象，抽象的 `*.msg` 定义在对应 Topic 的文件夹中，或是单独作为一个程序包并被其他程序包所包括。

### 4. Topic
话题，或者叫主题，可以看成是一个频道，或者说是一个广场，单个或多个节点可以向一个话题发送消息，单个或多个节点也可以去订阅这个话题来获取消息。因此，话题的存在实现了消息发布者和消息订阅者之间的解耦，两者无需直接通信，无需知道对方的存在。

一个话题对应一种格式类型的 Message。

Topic 在代码层面是字符串。

### 5. Service
服务，相对于话题的通讯模式是广播式的，像UDP，它更像是TCP，是一端到另一端的直接通讯，并针对一次请求给予一次回应。因此没有频繁的消息传递，没有高系统资源占用。

Service通信是一对一通信，信息流是双向的，而且客户端与服务端之间的执行是同步的 (有一定顺序的)。

### 6. Bag
包 (为了区别下文中的程序包`package`一般叫它`rosbag`)，用于保存并回放 ROS 运行中所有消息数据的特殊格式，在 debug 和数据后处理阶段非常有用。

## 1.2 文件系统
直观的说，就是我们写的代码和其他附加文件的组织结构。由于 ROS 是基于 CMake 编译系统组织的，因此 ROS 的文件系统就是 catkin workspace 的组织形式。

### 1. Package
功能包，ROS 的基本组织形式，CMake 的基本编译单元，用于组织 ROS 中不同的功能模块。一个功能包可以包含多个可执行文件 (节点)。

ROS 的实现就是通过包的形式把各种节点、依赖库、驱动、配置文件和数据库联系起来 (涉及编译与链接原理)。

### 2. Stack
堆，单个或多个包的集合，一般实现一个完整功能，与发行有关，不常用。

### 3. Workspace
工作空间，组织一个完整 ROS 项目的总文件夹，一个 ROS workspace 就是一个 catkin workspace。

# 2. ROS 的基本开发步骤
## 2.1 初始化工作空间
创建一个 catkin 工作空间并初始化：

```Shell
mkdir catkin_ws
cd catkin_ws
catkin_make
```

一个 catkin workspace 中的文件夹主要包括：
* `src` ：代码源文件
* `build` ：中间文件
* `devel` ：目标文件、头文件与链接库

## 2.2 编写代码
在 `src` 文件夹里创建所需的 `package`： 
```shell
cd src
# catkin_create_pkg <new_package_name> <package_deps>
catkin_create_pkg package_name rospy std_msgs
```
一个功能包中的主要文件包括：

* `CMakeLists.txt` (必须): 规定本功能包的编译规则
* `package.xml` (必须): 描述文件，定义 package 的属性信息，如包名、版本、作者等。一般只需要改`<build_depend>`和`<run_depend>`这两个字段。
* `scripts`: 存放 shell 或 python 脚本
* `include`: 源代码的头文件
* `src`: cpp 源代码
* `msg`: 自定义消息格式，*.msg
* `srv`: 自定义服务，*.srv
* `action`：自定义动作，*.action
* `config`: 参数、设置文件等，如*.yaml
* `launch`: 启动文件 `*.launch`，配合 `roslaunch` 命令可以一次运行多个节点

## 2.3 编译工作空间
编译完整的工作空间，包括内部所有功能包
```
cd ~/catkin_ws
catkin_make
```
CMake编译系统的流程包括：
1. 首先在工作空间 `catkin_ws/src/` 下递归的查找其中每一个ROS package
2. CMake 编译系统依据 `CMakeLists.txt` 文件，生成makefiles(放在catkin_ws/build/中)
3. 然后make刚刚生成的makefiles等文件，编译链接生成可执行文件（放在 `catkin_ws/devel` 中）

## 2.4 为当前 Terminal 刷新环境变量
在要执行 ROS 节点的终端中执行：
```shell
source ~/catkin_ws/devel/setup.bash
```
将新编译的相关环境变量加载到当前终端窗口。

## 2.5 检查环境变量（可选）


# 参考资料
1. [ROS 培训教程](https://tr-ros-tutorial.readthedocs.io/zh_CN/latest/index.html)
2. [古月居](https://www.guyuehome.com/Blog/index/category/11/p/1)
3. [ROS Wiki](http://wiki.ros.org/)
4. [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials)
5. [创客](https://www.ncnynl.com/)