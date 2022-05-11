---	
layout:     post	
title:      『Agile Software Engineering』 OSome Alpha	
subtitle:   『敏捷软件工程』 新一代 OS 课程平台（Alpha 版本）    
date:       2022-05-11	   
author:     Coekjan 
header-img: img/post-bg-ASE.jpg	
catalog:    true	
katex:    false    
tags:	
    - Agile Software Engineering  
---

> 本博客旨在总结个人在 Alpha 阶段中的工作。
> - Alpha 阶段的开发历时 3 周，前两周为主要开发期，最后一周为测试稳定期

## 概览

在 OSome 平台的开发中，我们对任务进行了划分。根据划分，我主要负责的是：
- 数据模型的建立与维护
- WEB 后端 API 的设计与实现
- WEB 后端 API 的单元测试
- 评测端设计与实现
- WEB 后端与评测端的自动化构建与部署

主要涉及的技术栈有：
- 数据库：SQL（PostgreSQL）
- WEB 后端 API：Golang（Gin 框架）、Go Test
- 评测端：Golang、Shell、Docker
- 自动化构建与部署：Gitlab CI/CD、Docker

## 工作详情

### 数据模型的建立与维护

此部分由我、[@Guoyp](https://github.com/gyp2847399255)、[@Pantw](https://github.com/GrapeLemonade) 一同完成。开发初期三人共同设计、建立数据模型，中后期由 [@Pantw](https://github.com/GrapeLemonade) 维护数据库的权限问题（评测端使用一个单独的特殊权限用户），由我和 [@Guoyp](https://github.com/gyp2847399255) 维护数据模型的形态（API 设计过程中会对数据模型进行一些小改动）。

Alpha 阶段结束时，数据库中共有 16 个数据模型。

### WEB 后端 API 的设计与实现

此部分由我和 [@Guoyp](https://github.com/gyp284739925) 一同完成。开发初期两人结对编程，共同完成 Golang 与一些包的调研与试用，共同完成 API 的设计；中期持续结对，共同完成部分 API 的实现；后期由 [@Guoyp](https://github.com/gyp284739925) 完成其余 API 的实现，我负责进行代码复审与单元测试。

Alpha 阶段结束时 WEB 后端共有 50 个 API，共 2894 行代码。我参与了大概 1/4 的代码实现。

### WEB 后端单元测试

此部分由我在 API 开发中期时开始构建，主要完成 Go Test 调研、阶段式并行化单元测试框架构建、主要 API 系列单元测试的构建。

Alpha 阶段结束时，完成 24 个重要 API 的单元测试，涉及 68 个测试用例，覆盖率达到 62.6%，共 1874 行代码。

### 评测端设计与实现

此部分由我和 [@Pantw](https://github.com/GrapeLemonade) 一同实现。[@Pantw](https://github.com/GrapeLemonade) 主要负责构建自营 Docker Registry、Docker-In-Docker 技术支持、V-TEST 虚拟机构建等运行维护相关工作，我负责评测机的并发逻辑、Gitlab 交互、数据库（后端）交互、评测环境配置、题目格式设计等。

Alpha 阶段结束时，共 723 行代码。

### WEB 后端与评测端的自动化构建与部署

这部分由我和 [@Pantw](https://github.com/GrapeLemonade) 一同实现。主要涉及 develop+master 双轨制部署、CI 触发单元测试、Docker 部署后端、Docker 构建评测镜像、Docker 发布到自营 Registry 等。我主要负责 develop+master 双轨制部署、CI 触发后端单元测试（给出测试报告）、Docker 部署后端、Docker 构建评测镜像。

Alpha 阶段结束时，后端已触发 185 次 CI/CD，评测端已触发 28 次 CI/CD。

## DevOps 敏捷开发之道

CI/CD 是敏捷开发的关键。我们为学生前端、教师前端、后端、评测端、评测环境都配置了 Gitlab CI/CD，从而自动进行回归测试、构建、部署到服务器，极大提高了团队开发效率——除了运维同学外，自动构建、部署的过程对于开发者来说是近乎透明的，开发者只需要知道有一个机制使得我每一次 push/merge 都能在服务器上看到效果，而不关心这个机制的具体实现。

Docker 容器技术也是敏捷开发与系统运维的关键。容器技术在我们的开发过程中起到了关键作用：
- 我们可以在一个机器上同时运行多个服务且互不冲突
- 我们利用 Docker 镜像来包装、发布分布式评测机，只需在 V-TEST 上运行 docker-compose 即可自动拉取评测机镜像，扩充评测结点
