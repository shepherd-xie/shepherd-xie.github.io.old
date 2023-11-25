---
title: Docker 代理
date: 2023-11-24 12:34:50
tags: 
  - 代理
categories:
  - linux
  - docker
top:
---

`/etc/docker/daemon.json`

```yaml
{
  "proxies": {
    "http-proxy": "http://proxy.example.com:3128",
    "https-proxy": "https://proxy.example.com:3129",
    "no-proxy": "localhost,127.0.0.0/8,1"
  }
}
```

