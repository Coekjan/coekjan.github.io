---	
layout:     post	
title:      『Agile Software Engineering』 Read And Research	
subtitle:   『敏捷软件工程』 阅读与调研    
date:       2022-03-08	   
author:     Coekjan 
header-img: img/post-bg-ASE.jpg	
catalog:    true	
katex:    false    
tags:	
    - Agile Software Engineering  
---

## 读《构建之法》

阅读过程中有下述疑问：
* **A/B 测试**：A/B 测试只设置了一个变量。若为了同时测试多个变量，是否能做多变量的测试呢？
  > 事实上确实有多元测试（[Multivariate Test](https://www.optimizely.com/optimization-glossary/multivariate-test-vs-ab-test/)）。多元测试有一个致命缺陷，需要测试的组合数与变量数之间是存在阶乘关系的，变量增多时开销将急剧变大。因此在“同时测试多个变量”和“开销”之间需要做决策。那么，在做权衡时，“同时测试多个变量”和“开销”之间是怎样的关系呢？能通过恰当的建模来权衡吗？如果能，模型应该是怎么样的呢？
* **软件开发流程**：在 Web 应用开发时，团队往往采用前后端分离的架构。这意味着一些成员开发前端，一些成员开发后端。前端需要与后端交互，是否意味着前端要等后端开发好了再开始开发呢？
  > * 一种较显然的思路是：前后端分离时，决定好后端接口，可根据后端接口的表述，编写若干接口的“假实现”来支撑前端开发。这样，前端和后端可以同步开发。
  > * 目前已有成熟解决方案 [Mock](http://mockjs.com/)：可以通过模拟请求与回应来使得前端独立于后端进行开发。
  > * 是否还有其他的解决方案？
* **工程师的职业发展**：若某人身体有缺陷，他/她还能做一名合格、甚至是优秀的工程师吗？
  > 书上此处给出了两个链接，但这两个链接都已失效。由于一些经历，我也对此问题有兴趣。
* **代码复审**：注意到有 todo/bug 等标记，个人认为这种标记不应该出现在代码里。
  > 目前的代码托管软件（如 Github、Gitlab）等都有 Issue 功能，应当把这些待办事项、软件缺陷以 Issue 的形式管理起来。除此之外，是否有更好的做法呢？
* **代码风格**：注意到书上有关 public/protected/private 的叙述甚少，这里以面向对象的视角，个人对 public/private 的用法都比较了解，也经常使用，但对 protected 的了解不多。在此给出个人的见解：
  > 从模块设计的角度来说，设计者总是要防止自己模块里的部件（或者说状态）被子类非法修改的。否则从父类角度来看，是不知道自己的状态在何时在何处被修改的，这十分危险。因为某些类中的逻辑很可能会假定自己某个状态只由自己本身的方法改变。
  
  从这个角度出发，似乎 protected 就无法适用于可变的成员变量了。那么如果是不可变的成员变量（不可变的成员变量不仅要求成员以只读修饰，若成员变量引用了一个对象，还要求引用的对象也是不可变的）呢？是否有技术做这样的检测？
  > [CheckStyle](https://github.com/checkstyle/checkstyle) 似乎只能做到词法和语法分析，并不能做到上述语义层级的分析。

## 源代码版本管理软件调研

主要调研对比 Github、Gitlab、Coding 三个平台。

### Github

* **人员权限控制**——Read/Triage/Write/Maintain/Admin 五种权限：
  * Read：读、克隆仓库，开启 Issue 和 Pull Request，在 Issue 和 Pull Request 下评论
  * Triage：除了 Read 权限外，还可以管理 Issue 和 Pull Request
  * Write：除了 Triage 权限外，还可以 push 到仓库
  * Maintain：除了 Write 权限外，还可以管理仓库设置
  * Admin：除了 Maintain 权限外，还可以添加仓库合作者
* **协作开发流程**——Issue、Pull Request

### Gitlab

* **人员权限控制**——Guest/Reporter/Developer/Master/Owner 五种权限：
  * Guest：开启 Issue，在 Issue 下评论
  * Reporter：除了 Guest 权限外，还可以读、克隆仓库
  * Developer：除了 Reporter 权限外，还可以 push 到仓库
  * Maintainer：除了 Developer 权限外，还可以创建项目，添加 Tag，设置保护分支，添加项目成员，编辑项目
  * Owner：除了 Maintainer 权限外，还可以设置项目可见性，删除、迁移项目，管理组成员
* **协作开发流程**——Issue、Merge Request

### Coding

* **人员权限控制**——所有者/管理员/普通成员三种默认权限组，可以自定义权限组，设置权限细节：
  * 所有者：默认拥有所有权限
  * 管理员：默认拥有除「变更团队所有者」和「设置管理员」外的所有权限
  * 普通成员：默认拥有「创建项目」、「导入项目」、「访问令牌」、「应用授权」、「应用管理」等权限
* **协作开发流程**——Issue、Merge Request

## CI/CD 技术调研与实践

Function | Github | Gitlab | Coding
:-------:|:-------|:-------|:------
CI/CD 支持 | Github Actions，采用 Github Actions 专用的 Yaml 格式 | Gitlab CI/CD，采用 Gitlab CI/CD 专用的 Yaml 格式 | Coding CI/CD，采用 Jenkins

此处为提供 public 仓库，以 Github Actions 和 Coding CI/CD 来演示。

### Github Actions

* [Trivial-Derivation](https://github.com/Coekjan/Trivial-Derivation)：演示了使用 Github Actions 进行自动化单元测试。
  ![]({{ '/img/ASE-Github-Actions-Trivial-Derivation.png' | prepend: site.baseurl}})
* [Trivial-TimedInput](https://github.com/Coekjan/Trivial-TimedInput)：演示了使用 Github Actions 进行自动编译打包。
  ![]({{ '/img/ASE-Github-Actions-Trivial-TimedInput.png' | prepend: site.baseurl}})
* [Coekjan-Blog](https://github.com/Coekjan/coekjan.github.io)：演示了使用 Github Pages 提供的 Actions 进行自动编译部署。
  ![]({{ '/img/ASE-Github-Actions-Coekjan-Blog.png' | prepend: site.baseurl}})

### Coding CI/CD

* [E-Market](https://coekjan.coding.net/public/database-project-e-market/e-market/git/files)：演示了使用 Coding CI/CD（Jenkinsfile）进行自动推送远端服务器。
  ![]({{ '/img/ASE-Coding-CI-CD-E-Market.png' | prepend: site.baseurl}})

### 在工程实践中的 CI/CD 技术

CI/CD 技术能够帮助团队利用服务器资源进行回归测试、单元测试、自动部署等，利于团队对源代码进行质量管控等操作，解放团队生产力——不再需要人力做测试与部署、代码质量管理等。

在上述两种 CI/CD 工具中（Github Actions 和 Coding CI）：前者提供了丰富的组件市场，搭建方便；后者采用了较通用的 Jenkins，Jenkins 可以在任何源代码管理软件中做内嵌，无需考虑源代码管理软件本身是否支持 CI/CD。
