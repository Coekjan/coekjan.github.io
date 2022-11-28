---	
layout:     post	
title:      『Manjaro Linux』 Docker Easyconnect	
subtitle:   『Manjaro Linux』 在 docker 中运行 easyconnect    
date:       2022-11-29	   
author:     Coekjan 
header-img: img/post-bg-MJ.jpg	
catalog:    true	
katex:    false    
tags:	
    - Manjaro Linux  
---

## 为什么不直接运行 Easyconnect？

Easyconnect（内网 VPN）与 Clash（科学上网代理）同时运行时，浏览器访问（或 cURL、dig）内网网站（使用域名）时会出现 DNS 解析失败的问题，但是 nslookup 可以解析域名。根据抓包报文分析，初步判断是 Easyconnect 无法很好处理 DNS 请求报文中的 Additional Records。理由是：nslookup 发送的 DNS 请求中没有 Additional Records，而 cURL、dig 发送的请求中带有 Additional Records。

![]({{ '/img/MJ-easyconnect-dns-bug.png' | prepend: site.baseurl }})

## 将 Easyconnect 放到 Docker 容器中

参考 [docker-easyconnect](https://github.com/Hagb/docker-easyconnect)，可以在本地 `docker pull hagb/docker-easyconnect:cli` 拉取镜像。然后编写并运行脚本：

```shell
#!/bin/bash

set -e

EASYCONNECT_VERSION='your easyconnect version: 7.6.3 or 7.6.7 or 7.6.8'
SERVER='your easyconnect server domain'
USERNAME='your username'
PASSWORD='your password'
CONTAINER_NAME='your container name'

[ -z $SOCKS5_PORT ] && SOCKS5_PORT=1080
[ -z $HTTP_PORT ] && HTTP_PORT=8888

docker stop "$CONTAINER_NAME" && docker rm "$CONTAINER_NAME" || true
docker run -d \
        --name "$CONTAINER_NAME" \
        --device /dev/net/tun \
        --cap-add NET_ADMIN -ti \
        -p 127.0.0.1:"$SOCKS5_PORT":1080 \
        -p 127.0.0.1:"$HTTP_PORT":8888 \
        -e EC_VER="$EASYCONNECT_VERSION" \
        -e CLI_OPTS="-d $SERVER -u $USERNAME -p $PASSWORD" \
        hagb/docker-easyconnect:cli
```

随后可以在 `docker logs $CONTAINER_NAME` 中看到登录等信息。下一步是在 Clash 中加入相应规则以将合适的流量转发到 Docker 内的 Easyconnect，以北航校内服务为例：

```yml
proxies:
  - name: BUAA
    type: socks5
    server: 127.0.0.1
    port: # socks5 port
    udp: true
  # ...
# ...
rules:
  - DOMAIN-SUFFIX,mail.buaa.edu.cn,DIRECT
  - DOMAIN-SUFFIX,d.buaa.edu.cn,DIRECT
  - DOMAIN-SUFFIX,vpn.buaa.edu.cn,DIRECT
  - DOMAIN-SUFFIX,app.buaa.edu.cn,DIRECT
  - DOMAIN-SUFFIX,sso.buaa.edu.cn,DIRECT
  - DOMAIN-SUFFIX,buaa.edu.cn,BUAA
  # ...
```

Rules 中先将可以在公网访问的资源列出给予 DIRECT 策略，再将其他资源给予 BUAA 策略（proxies 中定义）。

### SSH 流量代理

首先下载并安装 OpenBSD netcat 工具，比如可以使用 `yay -S openbsd-netcat` 安装。随后在 `~/.ssh/config` 中配置：

> 假设你的 socks5 代理端口为 1080

```conf
Host your-host
        # ...
        ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p

# ...
```

即可使得 `ssh your-host` 的流量经过 easyconnect 代理。
