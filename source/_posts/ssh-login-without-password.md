---
title: ssh免密登录
date: 2020-03-25 15:47:12
tags:
categories:
  - linux
top:
---

- 使用 `ssh-keygen` 生成密钥对

```shell
# ssh-keygen -o -t rsa -b 4096 -C "your_email@example.com"
# ed25519 加密算法运算速度更快
ssh-keygen -t ed25519 -C "your_email@example.com"
```

- 使用 `ssh-copy-id` 将公钥发送至目标主机

```shell
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host
```

- 使用 `ssh` 免密登录

`ssh user@host`
