---
title: 七日杀服务器搭建
date: 2023-07-26 11:39:48
tags:
  - self-hosted
  - 七日杀
categories: self-hosted
top:

---

七日杀是一款自 2013 年发售至今仍然在进行 Alpha 测试的 _**开放世界生存恐怖游戏**_ ，目前最新版本为 A21 。作为一款测试了这么久的游戏，在六月份的 A21 版本发布之后，同时在线人数一度来到 Steam 实时在线榜单前十。

![image-20230726124047941](https://images.orkva.com/images/2023/07/26/image-20230726124047941.png)

![image-20230726124436198](https://images.orkva.com/images/2023/07/26/image-20230726124436198.png)

<!-- more -->

## 前言

这款游戏可以追溯到我的大学时期，在一众室友寻找联机游戏时无意中发现了这个宝藏。彼时游戏还处于“盗版满天飞，随处可下载”的环境，七日杀作为一款小众游戏并没有庞大的玩家群体，以至于游戏汉化极度不完全，对于彼时英语懵懵懂懂的我来说卡关则是常有的事。

在工作之后这款游戏也是理所应当的进入了我的 Steam 库存中，并且在相当久的一段时间内成为了我们几个刚步入工作岗位小伙伴们下班之后消遣的游戏。

最早大学时联机都处于局域网联机阶段，两台电脑在同一个局域网环境下即可进行联机。工作后则是使用 Steam 平台进行联机，但是不可避免的会出现作为主机的用户没有下班，其他人聊天等他的情况。在 A21 更新之后，出于随时游玩的目的，便在思考如何搭建一个属于自己的游戏服务器。

## 如何搭建一个游戏服务器

游戏服务器对于部分人来说可能会有些陌生，但是对于私服，大多数的老游戏玩家都不会感到陌生。在那些年流传许久、顶顶大名的传奇私服，还有各种各样的网游私服，其实都属于是游戏服务器。

如果你想搭建一个属于自己的服务器，有以下几种方式：

1. 到专门的游戏服务器网站购买。（省心省力，但是贵）
2. 自建服务器。（复杂，需要一定程度专业知识）
3. ~~**找一个有服务器的朋友，然后抢过来。**~~

当然，对于我们来说肯定是自己来搭建，否则的话这篇文章不是白写了。

## 七日杀服务器搭建

对于绝大多数想要搭建服务器的用户，我的建议都是在服务器提供商处购买专门的游戏服务器。

[Sersers | 7 days to die](https://7daystodie.com/servers/) 提供了一些建议供应商。

![image-20230726131449295](https://images.orkva.com/images/2023/07/26/image-20230726131449295.png)

如果你想要搭建自己的服务器，那么下面会提供两种方式。

### 基于 Steam (Windows) 环境下的服务器搭建

#### 环境要求

* CPU：4 核 8 线程 _以上_（推荐）
* 内存：16 GB _以上_（推荐）
* 操作系统：Windows 10 / Windows Server 2022 （推荐）
* 应用：Steam （必须）

#### 下载服务器工具

在 Steam 上下载安装 [7 Days to Die Dedicated Server](https://store.steampowered.com/app/294420/7_Days_to_Die_Dedicated_Server/) 。

![image-20230726132514421](https://images.orkva.com/images/2023/07/26/image-20230726132514421.png)

#### 参数配置

配置文件位于 `steamapps\common\7 Days to Die Dedicated Server\serverconfig.xml` ，详细配置文件参考 [配置文件](#配置文件) 。

#### 启动服务器

![img](https://images.orkva.com/images/2023/07/26/cec9e53b2a846f51c9e360e9110a2754f4050358.png)

出现 `GameServer.LogOn successful` 即启动成功。

![img](https://images.orkva.com/images/2023/07/26/1db616a5c44242f86f1b5fda98b1f47ad56f2889.png)

#### 防火墙配置

如果你使用的是云服务器，则需要在对应的云服务器防火墙允许服务器端口入站，参考 [服务器端口](#服务器端口) 。

### 基于 Docker (Debian) 环境下的服务器搭建

#### 环境要求

* CPU：4 核 8 线程 _以上_（推荐）
* 内存：16 GB _以上_（推荐）
* 操作系统：Debian 10 / Ubuntu 20.04.6 LTS（推荐）
* 应用：Docker （必须）

#### Docker Compose 配置

我们使用的镜像是 [vinanrra/7dtd-server](https://github.com/vinanrra/Docker-7DaysToDie) ，docker-compose.yml 文件使用项目提供的默认文件即可。

```yaml
version: '2'
services:
  7dtdserver:
    image: vinanrra/7dtd-server
    container_name: 7dtdserver
    environment:
      - START_MODE=1 #Change between START MODES
      - VERSION=stable # Change between 7 days to die versions
      - PUID=1000 # Remember to use same as your user
      - PGID=1000 # Remember to use same as your user
      - TimeZone=Europe/Madrid # Optional - Change Timezone
      - TEST_ALERT=NO # Optional - Send a test alert
      - UPDATE_MODS=NO # Optional - This will allow mods to be update on start, each mod also need to have XXXX_UPDATE=YES to update on start
      - ALLOC_FIXES=NO # Optional - Install ALLOC FIXES
      - ALLOC_FIXES_UPDATE # Optional - Update Allocs Fixes before server start
      - UNDEAD_LEGACY=NO # Optional - Install Undead Legacy mod, if DARKNESS_FALLS it's enable will not install anything
      - UNDEAD_LEGACY_VERSION=stable # Optional - Undead Legacy version
      - UNDEAD_LEGACY_UPDATE=NO # Optional - Update Undead Legacy mod before server start
      - DARKNESS_FALLS=NO # Optional - Install Darkness Falls mod, if UNDEAD_LEGACY it's enable will not install anything
      - DARKNESS_FALLS_UPDATE=NO  # Optional - Update Darkness Falls mod before server start
      - DARKNESS_FALLS_URL=False # Optional - Install the provided Darkness Falls url
      - ENZOMBIES=NO # Optional - Install EnZombies mod
      - ENZOMBIES_ADDON_SNUFKIN=NO # Optional - Install EnZombies addon mod
      - ENZOMBIES_ADDON_ROBELOTO=NO # Optional - Install EnZombies addon mod
      - ENZOMBIES_ADDON_NONUDES=NO # Optional - Install EnZombies addon mod
      - ENZOMBIES_UPDATE=NO # Optional - Update EnZombies mod and addons before server start
      - CPM=NO # Optional - CSMM Patron's Mod (CPM)
      - CPM_UPDATE=NO # Optional - Update CPM before server start
      - BEPINEX=NO # Optional - BepInEx
      - BEPINEX_UPDATE=NO # Optional - Update BepInEx before server start
      - BACKUP=NO # Optional - Backup server at 5 AM
      - MONITOR=NO # Optional - Keeps server up if crash
    volumes:
      - /path/to/folder/7DaysToDie:/home/sdtdserver/.local/share/7DaysToDie/
      - /path/to/folder/LGSM-Config:/home/sdtdserver/lgsm/config-lgsm/sdtdserver
      - /path/to/folder/ServerFiles:/home/sdtdserver/serverfiles/ # Optional - serverfiles folder
      - /path/to/folder/log:/home/sdtdserver/log/ # Optional - Logs folder
      - /path/to/folder/backups:/home/sdtdserver/lgsm/backup/ # Optional - If BACKUP=NO, backups folder
    ports:
      - 26900:26900/tcp # Default game ports
      - 26900:26900/udp # Default game ports
      - 26901:26901/udp # Default game ports
      - 26902:26902/udp # Default game ports
      - 8080:8080/tcp # OPTIONAL - WEBADMIN
      - 8081:8081/tcp # OPTIONAL - TELNET
      - 8082:8082/tcp # OPTIONAL - WEBSERVER https://7dtd.illy.bz/wiki/Server%20fixes
    restart: unless-stopped # INFO - NEVER USE WITH START_MODE=4 or START_MODE=0
```

#### 参数配置

配置文件会在首次启动时自动生成，位于 `/path/to/folder/ServerFiles/sdtdserver.xml` ，详细配置文件参考 [配置文件](#配置文件) 。

#### 启动服务器

在 `docker-compose.yml` 目录下执行 `docker compose up -d` ，执行 `docker compose logs -f` 查看启动日志。

需要注意的是运行过程需要下载脚本文件，请保证你的网络环境可以访问该脚本。

![image-20230726142707910](https://images.orkva.com/images/2023/07/26/image-20230726142707910.png)

当看到 `GameServer.LogOn successful` 信息即为服务器启动成功

```shell
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Loading dymesh settings
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Dynamic Mesh Settings
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Use Imposter Values: False
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Only Player Areas: True
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Player Area Buffer: 3
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Max View Distance: 1000
7dtdserver  | 2023-07-26T07:24:45 47.744 INF Regen all on new world: False
7dtdserver  | 2023-07-26T07:24:45 47.748 INF Dymesh: Prepping dynamic mesh. Resend Default: True
7dtdserver  | 2023-07-26T07:24:45 47.748 INF Dymesh: Mesh location: /home/sdtdserver/.local/share/7DaysToDie/Saves/Navezgane/My Game/DynamicMeshes/
7dtdserver  | 2023-07-26T07:24:45 47.750 INF [Web] Webserver not started, WebDashboardEnabled set to false
7dtdserver  | 2023-07-26T07:24:45 47.750 INF StartGame done
7dtdserver  | 2023-07-26T07:24:45 47.756 INF [Steamworks.NET] Registering auth callbacks
7dtdserver  | 2023-07-26T07:24:45 47.759 INF [Steamworks.NET] GameServer.Init successful
7dtdserver  | 2023-07-26T07:24:45 47.762 INF [Steamworks.NET] Making server public
7dtdserver  | 2023-07-26T07:24:46 48.607 INF Dymesh: Warming dynamic mesh
7dtdserver  | 2023-07-26T07:24:46 48.607 INF Dymesh: Creating dynamic mesh manager
7dtdserver  | 2023-07-26T07:24:46 48.621 INF Dymesh: Awake
7dtdserver  | 2023-07-26T07:24:46 48.622 INF Dymesh: Mesh location: /home/sdtdserver/.local/share/7DaysToDie/Saves/Navezgane/My Game/DynamicMeshes/
7dtdserver  | 2023-07-26T07:24:46 48.627 INF Dymesh: Loading Items: /home/sdtdserver/.local/share/7DaysToDie/Saves/Navezgane/My Game/DynamicMeshes/
7dtdserver  | 2023-07-26T07:24:46 48.633 INF Dymesh: Loaded Items: 0
7dtdserver  | 2023-07-26T07:24:46 48.639 INF Dymesh: Loading all items took: 0.012797 seconds.
7dtdserver  | 2023-07-26T07:24:46 48.661 INF Clearing queues.
7dtdserver  | 2023-07-26T07:24:46 48.662 INF Cleared queues.
7dtdserver  | 2023-07-26T07:24:46 48.664 INF Dynamic thread starting
7dtdserver  | 2023-07-26T07:24:46 48.667 INF Dymesh door replacement: imposterBlock
7dtdserver  | 2023-07-26T07:24:48 50.340 INF [EOS] Server registered, session: 3762f18aae26443b85649c04af296cff
7dtdserver  | 2023-07-26T07:24:48 50.357 INF [EOS] Session address: 1.83.161.8
7dtdserver  | 2023-07-26T07:24:54 56.234 INF [Steamworks.NET] GameServer.LogOn successful, SteamID=*, public IP=*.8*.16*.*
7dtdserver  | Warning: failed to init SDL thread priority manager: SDL not found
```

可以看到我们的服务器出现在七日杀服务器搜索列表中。

![image-20230726153844640](https://images.orkva.com/images/2023/07/26/image-20230726153844640.png)

#### 服务器命令

由于服务器脚本拒绝使用 `root` 用户进行操作，所以容器内部用户默认 uid 为 1000 , 你可以使用 `docker compose exec -u 1000 7dtdserver bash` 连接到容器内部对游戏服务器进行操作。

```shell
LinuxGSM - 7 Days To Die - Version v23.3.6
https://linuxgsm.com/sdtdserver

Commands
start         st   | Start the server.
stop          sp   | Stop the server.
restart       r    | Restart the server.
monitor       m    | Check server status and restart if crashed.
test-alert    ta   | Send a test alert.
details       dt   | Display server information.
postdetails   pd   | Post details to termbin.com (removing passwords).
skeleton      sk   | Create a skeleton directory.
update-lgsm   ul   | Check and apply any LinuxGSM updates.
update        u    | Check and apply any server updates.
check-update  cu   | Check if a gameserver update is available
force-update  fu   | Apply server updates bypassing check.
validate      v    | Validate server files with SteamCMD.
backup        b    | Create backup archives of the server.
console       c    | Access server console.
debug         d    | Start server directly in your terminal.
mods-install  mi   | View and install available mods/addons.
mods-remove   mr   | View and remove an installed mod/addon.
mods-update   mu   | Update installed mods/addons.
install       i    | Install the server.
auto-install  ai   | Install the server without prompts.
developer     dev  | Enable developer Mode.
sponsor       s    | Donation options.
```

> 详细的服务器命令可以参考 https://linuxgsm.com/servers/sdtdserver/ 。

#### 防火墙配置

如果你使用的是云服务器，则需要在对应的云服务器防火墙允许服务器端口入站，参考 [服务器端口](#服务器端口) 。

## 配置文件

详细配置文件可以参考 https://developer.valvesoftware.com/wiki/7_Days_to_Die_Dedicated_Server ，这里只列举几条比较重要的配置。

```xml
<?xml version="1.0"?>
<ServerSettings>
        <!-- GENERAL SERVER SETTINGS -->

        <!-- Server representation -->
        <property name="ServerName"                                             value="My Game Host"/>          <!-- 服务器名称 -->
        <property name="ServerDescription"                              value="A 7 Days to Die server"/>        <!-- 服务器描述 -->
        <property name="ServerWebsiteURL"                               value=""/>                                      <!-- 服务器网站 -->
        <property name="ServerPassword"                                 value=""/>                                      <!-- 服务器密码 -->
        <property name="ServerLoginConfirmationText"    value="" />                                     <!-- 用户进入服务器欢迎文字 -->
        <property name="Region"                                                 value="NorthAmericaEast" />     <!-- 服务器位置，建议：Asia，可选值: NorthAmericaEast, NorthAmericaWest, CentralAmerica, SouthAmerica, Europe, Russia, Asia, MiddleEast, Africa, Oceania -->
        <property name="Language"                                               value="English" />                      <!-- 服务器语言，建议：Chinese -->

        <!-- Slots -->
        <property name="ServerMaxPlayerCount"                   value="8"/>                                     <!-- 最大玩家数量 -->

        <!-- Other technical settings -->
        <property name="EACEnabled"                                             value="true"/>                          <!-- 是否启用作弊检查，如果用 Mod 建议关闭 -->
        
        <!-- GAMEPLAY -->

        <!-- World -->
        <property name="GameWorld"                                              value="Navezgane"/>                     <!-- 世界类型，默认 Navezgane，可选值 PREGEN6k, PREGEN8k, PREGEN10k -->
        <property name="GameName"                                               value="My Game"/>                       <!-- 世界名称，每一个世界对应一个单独的存档，如果想开新档更改此值后重启游戏即可 -->
        <property name="GameMode"                                               value="GameModeSurvival"/>      <!-- GameModeSurvival -->

        <!-- Difficulty -->
        <property name="GameDifficulty"                                 value="1"/>                                     <!-- 0 - 5, 游戏难度, 0 最低, 5 最高 -->
</ServerSettings>
```

## 服务器端口

* TCP / 26900 （必须，游戏默认端口）
* UDP / 26900 - 26902  （必须，游戏默认端口）
* TCP / 8080 （可选，Web 管理端口）
* TCP / 8081 （可选，Telnet 管理端口）
* TCP / 8082 （可选，WebServer端口）

## 最后

玩游戏的最高配置是和你一起玩游戏的人，希望这篇文章对你有所帮助。
