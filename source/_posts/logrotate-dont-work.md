---
title: Logrotate 配置失效
date: 2024-08-30 16:24:10
tags:
  - linux
  - docker
categories:
top:
---

在使用 `logrotate` 时，不要将日志文件放在 `/usr` 路径下，否则定时任务执行时会出现异常，并且在手动执行 `logrotate -f` 时不会出发该异常。

```shell
➜  journalctl -u logrotate.service --since '7 days ago'
Aug 24 00:00:06 local systemd[1]: Starting logrotate.service - Rotate log files...
Aug 24 00:00:06 local logrotate[1975731]: error: error creating output file /usr/local/docker/nginx/log/nginx.log.20240823.gz: Read-only file system
Aug 24 00:00:06 local systemd[1]: logrotate.service: Main process exited, code=exited, status=1/FAILURE
Aug 24 00:00:06 local systemd[1]: logrotate.service: Failed with result 'exit-code'.
Aug 24 00:00:06 local systemd[1]: Failed to start logrotate.service - Rotate log files.
Aug 25 00:00:11 local systemd[1]: Starting logrotate.service - Rotate log files...
Aug 25 00:00:11 local logrotate[2016378]: error: error creating output file /usr/local/docker/nginx/log/nginx.log.20240823.gz: Read-only file system
Aug 25 00:00:11 local systemd[1]: logrotate.service: Main process exited, code=exited, status=1/FAILURE
Aug 25 00:00:11 local systemd[1]: logrotate.service: Failed with result 'exit-code'.
Aug 25 00:00:11 local systemd[1]: Failed to start logrotate.service - Rotate log files.
Aug 26 00:00:19 local systemd[1]: Starting logrotate.service - Rotate log files...
Aug 26 00:00:20 local logrotate[2056792]: error: error creating output file /usr/local/docker/nginx/log/nginx.log.20240823.gz: Read-only file system
Aug 26 00:00:20 local systemd[1]: logrotate.service: Main process exited, code=exited, status=1/FAILURE
Aug 26 00:00:20 local systemd[1]: logrotate.service: Failed with result 'exit-code'.
Aug 26 00:00:20 local systemd[1]: Failed to start logrotate.service - Rotate log files.
```

> [logrotate succeeds when manually run as root, but fails with "Read-only file system" when run by logrotate.service](https://askubuntu.com/questions/1275668/logrotate-succeeds-when-manually-run-as-root-but-fails-with-read-only-file-sys)
>
> Takes a boolean argument or the special values "full" or "strict". **If true, mounts the /usr and the boot loader directories (/boot and /efi) read-only for processes invoked by this unit.** If set to "full", the /etc directory is mounted read-only, too. If set to "strict" the entire file system hierarchy is mounted read-only, except for the API file system subtrees /dev, /proc and /sys (protect these directories using PrivateDevices=, ProtectKernelTunables=, ProtectControlGroups=). This setting ensures that any modification of the vendor-supplied operating system (and optionally its configuration, and local mounts) is prohibited for the service. It is recommended to enable this setting for all long-running services, unless they are involved with system updates or need to modify the operating system in other ways. If this option is used, ReadWritePaths= may be used to exclude specific directories from being made read-only. This setting is implied if DynamicUser= is set. This setting cannot ensure protection in all cases. In general it has the same limitations as ReadOnlyPaths=, see below. Defaults to off.
