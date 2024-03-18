---
title: docker远程连接
date: 2023-01-13 12:56:51
tags: docker
categories: docker
top:
---

在 IDEA 中我们可以直接安装 docker 插件添加 docker 作为运行环境，但是通常来说 docker 都是部署在 Linux 机器上。就个人习惯而言，一般是将 docker 安装在 Debian 虚拟机中，毕竟 IDEA 主要是在 Windows 下运行，而在 Windows 下安装 docker 实在是有些不合适，所以就需要配置 docker 的远程连接。

<!-- more -->

docker 本身的模型就是 客户端 + 服务端 的模式，所以远程连接应该不会太麻烦，但是网上的许多教程都是复制粘贴的，所以打算自己研究一下这个问题。

根据官方文档 [Configure remote access for Docker daemon](https://docs.docker.com/config/daemon/remote-access/) 我们对 docker 做如下配置。

1. 运行 `sudo systemctl edit docker.service` 编辑 `docker.service` 的覆盖文件

2. 在文件中添加如下代码，远程连接 docker 需要将 `tcp://127.0.0.1:2375` 更换为 `tcp://0.0.0.0:2375`

   ```
   [Service]
   ExecStart=
   ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
   ```

3. 保存文件

4. 重启 `systemctl` 配置

   ```
   sudo systemctl daemon-reload
   ```

5. 重启 docker

   ```
   sudo systemctl restart docker.service
   ```

6. 检查变更是否生效

   ```
   sudo netstat -lntp | grep dockerd
   tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
   ```

在第一步编辑代码的时候，Debian 默认使用 nano 进行编辑，感觉有些别扭还是换成了 vim，更换方式如下。

1. 在 `~/.bashrc` 中添加

   ```
   export SYSTEMD_EDITOR=vim
   ```

2. 使用 `source ~/.bashrc` 更新配置

更多修改方式参考[Change default editor to vim for _ sudo systemctl edit \[unit-file\] _](https://unix.stackexchange.com/questions/408413/change-default-editor-to-vim-for-sudo-systemctl-edit-unit-file)

当然 docker 官方还提供了另一种修改 `daemon.json` 的方法，但是我始终是没有成功过。

1. 在 `/etc/docker/daemon.json` 中添加如下配置

   ```
   {
     "hosts": ["unix:///var/run/docker.sock", "tcp://127.0.0.1:2375"]
   }
   ```

2. 重启 docker

3. 检查变更

   ```
   sudo netstat -lntp | grep dockerd
   tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
   ```

docker 官方对这两种方式的提醒是一起使用会引起 docker 无法启动，感兴趣的可以自行研究一下使用 daemon.json 的方式。

> Configuring Docker to listen for connections using both the systemd unit file and the `daemon.json` file causes a conflict that prevents Docker from starting.
