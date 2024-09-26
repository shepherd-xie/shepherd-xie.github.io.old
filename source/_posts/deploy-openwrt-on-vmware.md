---
title: 在 VMWare 中安装 OpenWrt
date: 2024-09-26 18:20:24
tags: 
  - vmware
  - openwrt
categories:
  - openwrt
top:
---

在安装之前需要准备以下相关文件或软件：

- *VMWare Workstation*
- [openwrt 固件](https://github.com/DHDAXCW/OpenWRT_x86_x64/releases) (本文使用 [DHDAXCW](https://github.com/DHDAXCW) 大佬固件)
- [StarWind V2V Converter](https://www.starwindsoftware.com/tmplink/starwindconverter.exe) 或其他 P2V 磁盘转换工具

此次安装的目的为使 *VMWare* 中的虚拟机不用再手动配置魔法，使用 *openwrt* 网关作到 **免配置科学上网** 。

<!-- more -->

### 1. 使用 StarWind V2V Converter 将 openwrt 固件转换为 vmdk 文件

选择 *Local file*

![image-20240926183631667](/images/image-20240926183631667.png)

选择 *openwrt* 解压后的 *img* 文件

![image-20240926184015581](/images/image-20240926184015581.png)

选择 *Local file*

![image-20240926184052299](/images/image-20240926184052299.png)

选择 *VMDK*

![image-20240926184131246](/images/image-20240926184131246.png)

![image-20240926184224063](/images/image-20240926184224063.png)

选择文件生成的位置

![image-20240926184248393](/images/image-20240926184248393.png)

![image-20240926184331239](/images/image-20240926184331239.png)

生成的文件如下

![image-20240926194610102](/images/image-20240926194610102.png)

### 2. 新建 openwrt 虚拟机

![image-20240926184429064](/images/image-20240926184429064.png)

![image-20240926184457606](/images/image-20240926184457606.png)

![image-20240926191144953](/images/image-20240926191144953.png)

处理器与内存分配看个人喜好与需求，这里我就一路默认了

![image-20240926191206812](/images/image-20240926191206812.png)

![image-20240926191253173](/images/image-20240926191253173.png)

网络这里先选择仅主机模式，我们稍后还要对网络进行设置

![image-20240926191352656](/images/image-20240926191352656.png)

一路默认直到选择磁盘，选择 **使用现有虚拟磁盘** 。

![image-20240926184555125](/images/image-20240926184555125.png)

选择刚才生成的 vmdk 文件

![image-20240926184631922](/images/image-20240926184631922.png)

磁盘格式转不转换都可以，这里我选择转换。

![image-20240926184708550](/images/image-20240926184708550.png)

点击完成，完成后不要着急开启虚拟机，我们还有配置需要修改

![image-20240926184821207](/images/image-20240926184821207.png)

选择 *编辑虚拟机设置*

![image-20240926191519032](/images/image-20240926191519032.png)

选择添加网络适配器

![image-20240926185612197](/images/image-20240926185612197.png)

将 *网络适配器2* 指定为 *NAT 模式*

![image-20240926191717546](/images/image-20240926191717546.png)

将 *网络适配器* 指定为 *VMent2 (仅主机模式)*

![image-20240926191641313](/images/image-20240926191641313.png)

这里 **网络适配器** 和 **网络适配器1** 的顺序非常重要，默认会将 **网络适配器 (eth0)** 分配给 **lan** 口， **网络适配器1 (eth1)** 分配给 **wan** 口。

至此，虚拟机配置完成

**p.s.** 

这里的 *VMnet2* 是之前已经创建的，打开 **VMWare - 编辑 - 虚拟网络编辑器** ，选择 **添加网络** ，将网络设置为 **仅主机模式** 并且 **取消勾选** **使用本地 DHCP 服务将 IP 地址分配给虚拟机**。

下面的网段可以选择自己喜欢的网段，不过尽量 *避免* 与主机正在使用的网段重合。

![image-20240926190134787](/images/image-20240926190134787.png)

### 3. 配置 openwrt 虚拟机

开启 *openwrt* 虚拟机，使用 `ping baidu.com` 检查网络，在以上操作完全正确的情况下，网络是可以正常访问的。如果不能正常访问，可以尝试重启网络 `/etc/init.d/network restart` 

![image-20240926193459195](/images/image-20240926193459195.png)

> 如果在重启网络之后依旧不能正常访问网络，请删除虚拟机 **(右键虚拟机 - 管理 - 从磁盘中删除)** ，并 **手动删除 vmdk 文件** ，重新执行前两步的操作。

修改网络配置文件 `vim /etc/config/network` 

```conf
config interface 'lan'
				option type 'bridge'
				option ifname 'eth0'
				option proto 'static'
				option ipaddr '192.168.11.1' # 将此 ip 修改为 VMent2 网段下，如 192.168.221.253
				option netmask '255.255.255.0'
				option ip6assign '60'
```

修改完成并保存后，重启网络 `/etc/init.d/network restart` 

![image-20240926190851262](/images/image-20240926190851262.png)

之后就可以使用浏览器访问 *openwrt* 地址为网络配置文件中修改的地址，默认密码为 **root/password**

![image-20240926194000612](/images/image-20240926194000612.png)

至此，在 *VMWare* 中安装 *openwrt* 就已经完成了。

### 4. 配置虚拟机科学上网

在 *openwrt* 中配置相应软件并开启后，所有虚拟机网络接口为 **VMnet2** 的虚拟机便可自行科学上网，确实是很方便。
