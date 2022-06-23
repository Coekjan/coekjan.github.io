---	
layout:     post	
title:      『Agile Software Engineering』 OSome Beta	
subtitle:   『敏捷软件工程』 新一代 OS 课程平台（Beta 版本）    
date:       2022-06-21	   
author:     Coekjan 
header-img: img/post-bg-ASE.jpg	
catalog:    true	
katex:    false    
tags:	
    - Agile Software Engineering  
---

> 本博客旨在总结个人在 Beta 阶段中的工作。
> - Beta 阶段的开发历时 3 周，前两周为主要开发期，最后一周为测试稳定期

## 概览

在 OSome 平台的 Beta 阶段开发中，我们对任务进行了划分。我主要负责：
- 评测端设计与实现
- WEB 后端 API 与评测机对接
- 数据模型设计
- 运维学习，熟悉平台整体

主要涉及的技术栈有：
- 数据库：SQL（PostgreSQL）
- WEB 后端 API：Golang（Gin 框架）、Go Test
- 评测端：Golang、Shell、Docker、Python
- 自动化构建与部署：Gitlab CI/CD、Docker
- 项目整体技术栈初步了解：
  - WEB 前端：Vue.js、HTML、CSS
  - 后端：Golang、PostgreSQL
  - 服务器：Openresty（Nginx）、Minio、Docker、Docker Regsitry
  - 评测端：Golang、Python
  - 教程前端（静态）：MkDocs

## 工作详情

### 评测机设计与实现

在 Beta 阶段，考虑到对于一个分布式评测系统，需要有一个机制得知分布式结点的健康情况——“心跳”机制。因此评测机不再直接与数据库交互，而是轮询后端 API 接口，使用后端的中间件来记录“心跳”。

### WEB 后端 API 设计、测试与评测机对接

这部分由我、[@Guoyp](https://github.com/gyp2847399255)、[@Fengxj](https://github.com/aalex1945) 一同完成。我主要负责评测机对接部分与单元测试。

### 数据模型设计

这部分由我、[@Guoyp](https://github.com/gyp2847399255)、[@Fengxj](https://github.com/aalex1945)、[@Pantw](https://github.com/GrapeLemonade) 一同完成。

根据 [@Pantw](https://github.com/GrapeLemonade) 的维护方案，API 设计与实现过程中对数据模型的改动，需要留存用于数据库迁移的 SQL 语句。

### 运维学习，熟悉平台整体

我初步熟悉了 Vue、HTML、CSS 在前端开发的基础知识，了解了反向代理、对象存储服务、镜像存储服务等的基本配置。

