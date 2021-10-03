---
title: "NVIDIA 驱动更新后 Chrome 黑屏故障排除记录"
description: "NVIDIA 驱动更新后 Chrome 黑屏故障排除记录"
date: 2020-06-10T01:58:30+08:00
draft: false
ShowToc: true
TocOpen: true
categories: ["技术"]
tags: ["故障排除"]
---

## 问题描述：

Nvidia Geforce 驱动更新，由441更新为446，更新完后按照提示直接重启。

重启后 Chrome 浏览器打开为全黑，且不是网页显示部分黑屏，而是包括标题栏、地址栏、收藏栏在内全黑，中间有一条白色竖线。

鼠标移动至特定位置可变为超链接状态，即操作逻辑正常，判断为界面渲染问题。

## 问题分析：

初步判断为显卡故障，运行其他浏览器正常，运行仿真软件正常，运行大型三维游戏正常。

再判断为 Chrome 硬件加速问题，尝试关闭，但未找到安全模式。

## 问题解决：

查找相关资料，有认为是驱动更新后 Chrome 浏览器的 gpu shader cache 未清理造成。

尝试删除以下路径内文件：

` C:\Users\Administrator\AppData\Local\Google\Chrome\User Data\ShaderCache\GPUCache `

重新打开浏览器后故障解决。

## 反思：

闭源驱动害死个人。