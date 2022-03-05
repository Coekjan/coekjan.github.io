---	
layout:     post	
title:      『MOS』 Local Development	
subtitle:   『MOS』 本地开发    
date:       2022-03-05	   
author:     Coekjan 
header-img: img/post-bg-MOS.jpg	
catalog:    true	
katex:    true    
tags:	
    - MOS  
---

本文给出了一种搭建 MOS 的本地开发环境的方法。

## 安装 Docker

首先安装 Docker，Linux 和 MacOS 下都很容易安装好，Windows 下估计问题挺多的。

> ~~所以建议这就逃离 Windows~~

## 使用 MOS 的开发环境 Dockerfile

参考[环境仓库](https://github.com/Coekjan/BUAA-OS-Docker-Environment)，将此仓库克隆到本地：

```shell
$ git clone git@github.com:Coekjan/BUAA-OS-Docker-Environment.git
```

然后进入仓库内，使用：

```shell
$ docker build -t oslab:latest .
```

构建镜像。假设你的开发工作目录在 `host-dir` 中，可以通过：

```shell
$ docker run -d -p 3372:22 --mount type=bind,source="host-dir",target="/home/scse/mos" oslab:latest
```

运行镜像，并将开发目录挂载到容器中的 `/home/scse/mos` 目录中。随后可以通过：

```shell
$ ssh -p 3372 scse@localhost
```

与容器建立连接，输入密码 `scse` 即可进入容器。在容器内的 `$HOME` 中，通过 `ls` 即可看见 `mos` 文件夹，其中的内容与宿主机中的开发目录是同步的。

## VSCode 连接 Docker 容器

首先需要下载 Remote SSH 这一插件，然后使用此插件连接 `scse@localhost` 的 `3372` 端口，输入密码 `scse` 即可连接上容器。在 VSCode 中，可以安装各类插件，以辅助开发。

### 免密

首先要通过 `ssh-keygen` 来生成自己的密钥对，密钥对一般为 `~/.ssh` 目录下的 `id_rsa.pub` 和 `id_rsa`，随后：

```shell
$ ssh-copy-id -i ~/.ssh/id_rsa.pub -p 3372 scse@localhost
```

输入密码，即可将公钥上传到容器。接下来即可免密直接连接。
