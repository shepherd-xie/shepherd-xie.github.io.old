---
title: ssh免密登录
date: 2020-03-25 15:47:12
tags:
categories:
top:
---

- 使用 `ssh-keygen` 生成密钥对

  `ssh-keygen -o -t rsa -b 4096 -C "email@example.com"`

- 使用 `ssh-copy-id` 将公钥发送至目标主机

  `ssh-copy-id -i ~/.ssh/id_rsa.pub user@host`

- 使用 `ssh` 免密登录

  `ssh user@host`