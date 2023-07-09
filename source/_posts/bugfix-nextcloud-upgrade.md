---
title: Nextcloud 升级故障修复
date: 2023-07-09 14:34:06
tags:
  - nextcloud
  - self-hosted
categories: 故障修复
top:
---

经过这次事故本人深刻地体会到了那一句话 —— **硬盘有价，数据无价。**

<!-- more -->

## 0. 事故起因

昨晚快凌晨的时候，正在调试一些自托管服务的功能，出于尝鲜新特性的想法升级了该服务。于是在这个大脑不怎么清醒的时刻产生了一个更不清醒的想法，将 Nextcloud 服务一起升级。

Nextcloud 是我常用的几个自托管服务之一，我在 PC 端、手机端均安装了该客户端软件，其中存放了许多个人数据，在 PC 与手机之间同步确实是十分方便，由此产生了将该服务也一并升级的想法。

再此之前我先描述一下服务器环境，本人的自托管服务均在 docker 上进行搭建，使用 docker-compose 进行容器编排管理，绝大多数有价值数据均使用容器卷映射在服务器硬盘上。但是，服务器上绝大多数数据都没有做备份处理，只有 MySQL 有每天的全量冷备份 —— 这又是一个悲惨的故事。

于是乎，在 **没有进行数据备份** 的情况下，我停止了 Nextcloud 服务，并将其版本号由 `20.0.14` 升级至了 `25.0.8` 然后进行了重新启动，再次访问时就出现了以下页面。

![image-20230709150400539](https://images.orkva.com/images/2023/07/09/image-20230709150400539.png)

## 1. 至暗时刻

此时我还没有意识到此时发生了什么，只把它当成升级时的正常过程，当后台升级完成之后就可以恢复。

于是为了了解升级的进度，我去查看了 Nextcloud 的 docker 日志。

```shell
➜  nextcloud docker-compose logs -f
nextcloud  | Initializing nextcloud 25.0.8.2 ...
nextcloud  | Upgrading nextcloud from 20.0.14.2 ...
nextcloud  | => Searching for scripts (*.sh) to run, located in the folder: /docker-entrypoint-hooks.d/pre-upgrade
nextcloud  | ==> but the hook folder "pre-upgrade" is empty, so nothing to do
nextcloud  | Nextcloud or one of the apps require upgrade - only a limited number of commands are available
nextcloud  | You may use your browser or the occ upgrade command to do the upgrade
nextcloud  | Setting log level to debug
nextcloud  | Turned on maintenance mode
nextcloud  | Exception: Updates between multiple major versions and downgrades are unsupported.
nextcloud  | Update failed
nextcloud  | Maintenance mode is kept active
nextcloud  | Resetting log level
nextcloud  | => Searching for scripts (*.sh) to run, located in the folder: /docker-entrypoint-hooks.d/before-starting
nextcloud  | ==> but the hook folder "before-starting" is empty, so nothing to do
```

重点在于这一句 `Exception: Updates between multiple major versions and downgrades are unsupported.` ，翻译过来就是 `不支持多个主要版本之间的更新和降级` 。

那既然不支持多个版本间的升级，我降回初始版本就好了吧。

降级之后，发现连网页都无法正常打开，此时我感到了一丝不妙。

```shell
➜ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED              STATUS                          PORTS                                                           NAMES
29534c77714d   nextcloud:20.0.14                     "/entrypoint.sh apac…"   About a minute ago   Restarting (1) 18 seconds ago                                                                   nextcloud
➜ docker-compose logs
nextcloud  | Can't start Nextcloud because the version of the data (25.0.8.2) is higher than the docker image version (20.0.14.2) and downgrading is not supported. Are you sure you have pulled the newest image version?
nextcloud  | Can't start Nextcloud because the version of the data (25.0.8.2) is higher than the docker image version (20.0.14.2) and downgrading is not supported. Are you sure you have pulled the newest image version?
nextcloud  | Can't start Nextcloud because the version of the data (25.0.8.2) is higher than the docker image version (20.0.14.2) and downgrading is not supported. Are you sure you have pulled the newest image version?
```

难道我升级了以后，即使在没有升级成功的情况下，服务的版本号也进行了变更？为了解决这个问题，我还是先查询一下正常的升级流程。

> 来源：
>
> # [How to upgrade](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/upgrade.html#how-to-upgrade)
>
> There are three ways to upgrade your Nextcloud server:
>
> - With the [Updater](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/update.html).
> - [Manually upgrading](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/manual_upgrade.html) with the Nextcloud `.tar` archive from our [Download page](https://nextcloud.com/install/).
> - [Upgrading](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/package_upgrade.html) via the snap packages.
> - Manually upgrading is also an option for users on shared hosting; download and unpack the Nextcloud tarball to your PC. Delete your existing Nextcloud files, except `data/` and `config/` files, on your hosting account. Then transfer the new Nextcloud files to your hosting account, again preserving your existing `data/` and `config/` files.
>
> When an update is available for your Nextcloud server, you will see a notification at the top of your Nextcloud Web interface. When you click the notification it brings you here, to this page.
>
> **It is best to keep your Nextcloud server upgraded regularly**, and to install all point releases and major releases. Major releases are 18, 19 or 20. Point releases are intermediate releases for each major release. For example 18.0.4 and 19.0.2 are point releases.
>
> - Nextcloud must be upgraded step by step:
>
>   Before you can upgrade to the next major release, Nextcloud upgrades to the latest point release.Then run the upgrade again to upgrade to the next major release’s latest point release.**You cannot skip major releases.** Please re-run the upgrade until you have reached the highest available (or applicable) release.Example: 18.0.5 -> 18.0.11 -> 19.0.5 -> 20.0.2
>
> **Wait for background migrations to finish after major upgrades**. After upgrading to a new major version, some migrations are scheduled to run as a background job. If you plan to upgrade directly to another major version (e.g. 24 -> 25 -> 26) you need to make sure these migrations were executed before starting the next upgrade. To do so you should run the `cron.php` file 2-3 times, for example:
>
> ```
> $ sudo -u www-data php -f /var/www/nextcloud/cron.php
> ```
>
> For more information about background jobs see [Background jobs](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/background_jobs_configuration.html).
>
> **Upgrading is disruptive**. Your Nextcloud server will be put into maintenance mode, so your users will be locked out until the upgrade is completed. Large installations may take several hours to complete the upgrade. Nevertheless usual upgrade times even for bigger installations are in the range of a few minutes.
>
> ## Prerequisites
>
> You should always maintain [regular backups](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/backup.html) and make a fresh backup before every upgrade.
>
> Then review third-party apps, if you have any, for compatibility with the new Nextcloud release. Any apps that are not developed by Nextcloud show a 3rd party designation. **Install unsupported apps at your own risk**. Then, before the upgrade, all 3rd party apps must be disabled. After the upgrade is complete you may re-enable them.

OK，升级前应该先对 `/data` 和 `/config` 进行备份，此时我已经进行了两次错误的操作，但是还是先将这两个文件夹进行备份。在这个时候我对 Nextcloud 的完整文件存储方式进行了充分的赞许，Nextcloud 储存文件是使用完整文件的形式进行存储，而不是将完整的文件进行拆分后再存储，以至于当我备份了 `/data` 之后有了些许的心安 —— 即使最后服务没有成功恢复，我也可以将文件数据全量转译后，清空数据后重新安装 Nextcloud ，再将数据恢复进去。

## 2. 黎明曙光

在查阅了诸多数据恢复资料后，我找到了一些具有参考价值的文章。

> 来源：
>
> # [Restoring backup](https://docs.nextcloud.com/server/latest/admin_manual/maintenance/restore.html#restoring-backup)
>
> To restore a Nextcloud installation there are four main things you need to restore:
>
> 1. The configuration directory
> 2. The data directory
> 3. The database
> 4. The theme directory

这个时候我十分庆幸由于以前的一些脑残操作，我对 MySQL 进行了每日备份，接下来就要开始进行数据恢复。

0. 备份 `/data` 和 `/config` 文件夹，Nextcloud 数据库数据全量备份
1. 删除所有数据及数据库表
2. 重新运行 `Nextcloud:20.0.14` ，初始化服务
3. 停止服务，删除 `/data` 和 `/config` ，删除 Nextcloud 数据库全量数据
4. 拷贝备份数据值 Nextcloud 工作文件夹，恢复 Nextcloud 数据库备份
5. 重新启动 Nextcloud
6. 执行 `docker-compose exec --user www-data nextcloud php occ files:scan --all` 重新扫描文件数据

```shell
➜ docker-compose exec --user www-data nextcloud php occ files:scan --all
Starting scan for user 1 out of 2 
Starting scan for user 2 out of 2 
+---------+-------+--------------+
| Folders | Files | Elapsed time |
+---------+-------+--------------+
| 31      | 395   | 00:00:01     |
+---------+-------+--------------+
```

## 3. 赛后总结

这个事件为我带来了几点经验和教训：

1. 服务升级之前先查看升级文档，尽量避免跨版本升级；
2. 所有重要数据以及在进行一些有可能影响数据的操作之前，一定要 **备份数据、备份数据、备份数据。**
