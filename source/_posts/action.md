---
title: hexo操作指南
date: 2019-12-03 21:13:56
tags: hexo
---

### 新建文章
```
hexo new post artical
```
### 发布
```
hexo g // 生成静态页面
hexo s // 启动本地服务，查看新增静态页面
hexo d // 发布至coding及github pages页面
hexo clean // 删除本地生成的静态页面
```
### 剩余步骤
在github中我们新增了一个Hexo分支用来保存我们的源代码，方便我们在更换电脑或公司时进行迁移，所以每次在发布新文章后，都要将代码发生的改动推至Hexo分支，切记是Hexo分支，不要推至master主分支。