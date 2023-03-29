---
title: "使用Hugo搭建Fix it主题博客"
subtitle: "记录搭建这个博客过程的一些细节和坑点"
draft: true
descriprion: this is a test md
date: 2023-03-06T23:55:17+08:00
type : posts
keywords: abc
tags:
- Hugo
- Fix it
- 技术

categories:
- 环境搭建

resources:
    - name: laptop
      src: laptop.jpg

lightgallery : true

---

## 页眉选项
* lightgallery true可以在外面显示avatar

## 踩坑点记录
* 用edge没有正确显示icon,调试了很多次才发现。最后决定一直用chrome了
* hugo的post目录下index是直接映射到文件目录的，可以直接引用目录下的文件。但是如果不是index的文档，就必须要从static里面引用了。这个很类似rust的crate设计。