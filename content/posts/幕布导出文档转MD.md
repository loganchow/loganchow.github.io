---
title: "幕布导出文档转 Markdown 格式"
date: 2021-10-03T22:33:59+08:00
draft: false
tags: ["效率","笔记"]
categories: ["技术"]
showToc: true
TocOpen: false
disableShare: true
---

幕布作为国产大纲笔记之光，整理思维导图和列写大纲文档非常方便。
但是幕布的导出文件只有一下几种格式：

![](https://raw.githubusercontent.com/loganchow/imgHost/main/202110032235849.png)

然而进行个人知识库整理时， `markdown` 格式会更加方便通用。

记录一下网上找到的 `OPML` 格式转 `markdown` 的一个方法。

## 前置条件
1. `npm` 包管理器

## 基本步骤
1. 安装 [`opml-to-markdown`](https://github.com/azu/opml-to-markdown), 输入命令：
     ```Shell
     npm install opml-to-markdown -g
     ```
2. 直接在 `opml` 文件所在目录下执行
    ```Shell
    opml-to-markdown {filename}.opml
    ```
3. 复制生成的文本，粘贴到合适的 Markdown 编辑器里整理格式即可使用。

> 本文更多是测试新版本 Github Actions 和 PicGo 图床的可用性。