---
title: "Test Post"
subtitle: "this is a subtitle"
draft: true
descriprion: this is a test md
keywords: abc
tags:
- test
categories:
- test
---

## 发布文章流程
hugo new path/filename
然后添加如下内容,注意hugo new 时会 **生成date** ，不然可以从terminal中拷贝一个最新的时间。
``` markdown
---
title: "Test Post"
subtitle: "this is a subtitle"
draft: true
type: post
descriprion: this is a test md
featuredImage: inWinter.jpg
featuredImagePreview: laptop.jpg
keywords: abc
tags:
- test
categories:
- test
---
```