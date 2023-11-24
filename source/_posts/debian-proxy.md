---
title: Debian 全局代理
date: 2023-11-24 12:22:06
tags: 
  - 代理
categories:
  - linux
top:
---

Linux 的代理主要是依靠 `HTTP_PROXY` `HTTPS_PROXY` 等几个全局变量实现。

在 `/etc/profile.d` 目录下新建 `proxy.sh` ，开机时会自动运行该目录下文件。

```shell
MY_PROXY_URL="<protocol>://<username>:<password>@<address>:<port>"

HTTP_PROXY=$MY_PROXY_URL
HTTPS_PROXY=$MY_PROXY_URL
FTP_PROXY=$MY_PROXY_URL
NO_PROXY=localhost,127.0.0.0/8,10.0.0.0/8,192.168.0.0/16
http_proxy=$MY_PROXY_URL
https_proxy=$MY_PROXY_URL
ftp_proxy=$MY_PROXY_URL
no_proxy=$NO_PROXY

export HTTP_PROXY HTTPS_PROXY FTP_PROXY NO_PROXY http_proxy https_proxy ftp_proxy no_proxy
```

