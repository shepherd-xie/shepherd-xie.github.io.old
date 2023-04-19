---
title: certbot 获取免费 SSL 证书
date: 2023-04-19 14:16:40
tags: 
  - HTTP
  - SSL
categories:
  - 网络
top:
---

自建网站或者在访问别人的网站时，有些人习惯性的关注浏览器地址栏左侧的小绿锁，总觉得没有它这个网站就不安全，这个问题也许是对的，毕竟 12306 和 MIT 的网站至今也没加上锁。对于 HTTPS 的讨论不是今天所关注的终点范畴，我们今天研究的对象是——如何给我们的网站加上这一把小绿锁。

<!-- more -->

所谓的小绿锁，通常情况下指的是 SSL 证书。有关于 SSL 证书更详细的部分，那就看我什么时候有时间去研究一下了，或许会再出一篇博客吧。

SSL 证书包含：

- 针对其颁发证书的域名
- 证书颁发给哪一个人、组织或设备
- 证书由哪一证书颁发机构颁发
- 证书颁发机构的数字签名
- 关联的子域
- 证书的颁发日期
- 证书的到期日期
- 公钥（私钥为保密状态）

# 如何获得 SSL 证书

绝大多数的域名服务商都会提供 SSL 证书服务，当然，大部分都是付费的。

其次，你也可以使用 _**自签名证书**_ ，但是基于安全考虑，自签名证书仅可以在信任证书的环境下使用，例如私人局域网。

最后，还有一些机构免费提供 SSL 证书，包括 Cloudflare 和 **Let's Encrypt** 。

# 使用 Certbot 获取 Let's Encrypt 证书

终于到了今天的正题，certbot 是一个开源的工具软件，可以在自管理的网站上使用 Let's Encrypt 的证书。

certbot 可以通过多种方式获取证书，具体的方式列表可以参考[Certbot 官方文档](https://eff-certbot.readthedocs.io/en/stable/using.html)。

## 使用域名服务商插件获取证书

对于绝大多数域名服务商，certbot 都提供了插件，可以更便捷的获取证书，这里以 Cloudflare 为例。

1. 确认你在 Cloudflare 购买了域名，或域名 DNS 已被 Cloudflare 托管

2. 在 My Profile -> API Tokens 创建 Tokens
   ![image-20230419144314550](https://images.orkva.com/images/2023/04/19/image-20230419144314550.png)

3. 将 Token 保存在 your_token_file.ini 文件中

   ```ini
   dns_cloudflare_api_token=your_token
   ```

4. 使用 certbot 获取证书

   ```shell
   certbot certonly \
     --dns-cloudflare \
     --dns-cloudflare-credentials your_token_file.ini \
     -d example.com
   ```

5. 证书会默认生成在 `/etc/letsencrypt/live` 下

6. 在你的 HTTP 服务器中添加 SSL 证书配置，以 Nginx 为例

    ```conf
    server {
        listen              443 ssl;
        server_name         www.example.com;
        ssl_certificate     www.example.com.crt;
        ssl_certificate_key www.example.com.key;
        ssl_protocols       TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        ...
    }
    ```

## 使用根目录模式

如果你可以随意更改 HTTP 服务器的内容，那么你可以使用根目录模式。

1. 登录至 HTTP 服务器所在的主机

2. 使用 certbot 获取证书

   ```shell
   certbot certonly --webroot \
   			-w ${your_web_local} \
           --email="admin@example.com" \
           -d "example.com"
   ```

3. 同域名服务商模式 5、6
