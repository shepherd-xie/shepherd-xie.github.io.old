---
title: Typora和图床
date: 2022-12-20 17:30:22
tags: 
  - Typora
  - 图床
categories:
top: 
---

在使用 Markdown 编写博客插入图片时，经常会先将图片上传至图床，然后再将图床链接插入 Markdown 中，但是这种操作过于麻烦，Typora 的自动上传功能让我们可以在插入图片的同时自动将图片上传至我们设定的图床并且更换 Markdown 中的路径。

<!-- more -->

Typora 不用过多介绍，一个非常漂亮的 Markdown 编辑器，支持多种主题，总有一款适合你，使用这个编辑器的时候会觉得写 Markdown 会是一种享受。但是只要是工具就会有这样或那样的痛点，比如说自 1.0 版本之后的 Typora 终于进入了收费模式，这让很多白嫖党十分痛心（包括我），而且低于 1.0 版本的 Typora 会强制检测当前是否为付费版本，非付费版本会禁止用户使用。当然本文说的问题并不是这个，但是这并不妨碍我们吐槽一番。

## Typora 图片上传服务

Typora 十分贴心的提供了图片上传服务，并提供了默认客户端：

- MacOS：
  - iPic：免费
  - **uPic：开源**（推荐）
  - Picsee：免费
  - PicGo.app：开源，中文
  - PicGo-Core（命令行）：开源
- Windows
  - PicGo.app：开源，中文
  - **PicGo-Core（命令行）：开源**（本文介绍方式）

除了介绍的这些客户端之外，Typora 还支持自定义命令，你可以自己编写上传工具/脚本。

## PicGo-Core

[PicGo-Core](https://picgo.github.io/PicGo-Core-Doc/)是 PicGo 的核心组件，简单的说，你可以把它看做一个文件上传命令行工具，Typora 原生支持 PicGo-Core，可以在 `Typora - 偏好设置 - 图像 - 上传服务设置` 中下载它，然后使用 `打开配置文件` 选项配置图床参数，配置文件参数可以参考 [PicGo-Core配置文件](https://picgo.github.io/PicGo-Core-Doc/zh/guide/config.html)。

## Chevereto

[Chevereto](https://chevereto.com/) 是一个图床服务，可以部署在自己的服务器上，此次选择这个服务作为我们的在线图床。

Chevereto 从 V4 版本之后进入付费使用阶段，所以我们此次使用的是还未收费的 [Chevereto-Free](https://github.com/rodber/chevereto-free) 虽然相对于最新版有许多功能不能使用，但是对于个人图床工具来说还是够用了。

## PicGo-Core 上传 Chevereto 配置文件

PicGo-Core 并不原生支持 Chevereto，但是 PicGo-Core 拥有众多的插件，我们可以使用 [picgo-plugin-web-uploader](https://github.com/yuki-xin/picgo-plugin-web-uploader) 将图片上传至 Chevereto。

配置文件实例：

```json
{
  "picgoPlugins": {
    "picgo-plugin-web-uploader": true
  },
  "picBed": {
    "current": "web-uploader",
    "web-uploader": {
      "customBody": "{\"key\":\"[Chevereto Key：从 Chevereto 管理界面获取]\"}",
      "customHeader": null,
      "jsonPath": "image.url",
      "paramName": "source",
      "url": "http://[Chevereto 地址]/api/1/upload"
    }
  }
}
```

配置好后可以点击 `验证图片上传选项` 来进行验证

```
[PicGo INFO]: Before transform
[PicGo INFO]: Transforming...
[PicGo INFO]: Before upload
[PicGo INFO]: Uploading...
[PicGo SUCCESS]:
https://chevereto/images/2022/12/20/typora-icon2.png
https://chevereto/images/2022/12/20/typora-icon.png
```

如上输出则上传成功
