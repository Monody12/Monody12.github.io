title: Hexo博客搭建（本地）
author: John Doe
tags:
  - blog
categories:
  - blog
date: 2021-02-16 19:35:00
---
## 博客搭建学习（本地）

---

在Windows操作系统本地搭建Hexo静态博客，花费了几个小时的时间，查阅了许多资料。由于是博客小白，也不太懂php和数据库知识，决定从静态博客入手。在众多生成工具模板中我选择了Hexo。下面是一些搭建时的学习过程和参考资料。

---

### 一、安装 Hexo以及必要文件Node.js和Git

安装过程中官网介绍足够详细，只参见这个[官方文档]( https://hexo.io/zh-cn/docs/ )就已足够。

---

### 二、安装Hexo管理工具Hexo-admin

在官方文档中找到插件栏点击即可找到这个[Git Hub开源项目](https://github.com/jaredly/hexo-admin)

注意：在输入安装命令时的位置应该在本地博客的位置空白处 右键打开Git Bash命令行中 （而不是在cmd或者power shell）

![blog中的git bash.jpg](https://i.loli.net/2021/02/16/FGPn1uI7LwAiEm8.jpg)

提示：在输入安装命令npm install --save hexo-admin时报错解决方法

报错原因：缺少相关依赖，输入以下命令安装依赖即可 [参见](https://albenw.github.io/posts/4ffa5bc6/)

```
npm install minimatch@"3.0.2"  
npm update -d
```



