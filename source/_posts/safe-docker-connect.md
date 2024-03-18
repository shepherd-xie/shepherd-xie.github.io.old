---
title: TLS 下的 docker 远程连接
date: 2024-03-18 10:53:51
tags: 
  - docker
  - safe
categories: docker
top:
---

由于需要使用 docker 远程调试一些程序，所以我开启了云服务器上的远程连接及其端口。因为是处于临时使用，所以我并没有对其进行任何安全加固，于是我在调试完成之后理所当然的忘记了关闭这个端口。我是在几天之后收到云服务商将服务器挖矿的邮件时才恍然大悟，哦，我好像没有关掉 docker 的远程连接，所以在这一段时间，服务器上的 docker 近似处在一种“裸奔”的状态。算起来这是我第二次被攻击，第一次被攻击的是没有密码的 MySQL 。

---

亡羊补牢为时未晚，如何安全的使用 docker 远程连接。官方为我们提供了几种解决方案：

- 使用安全的网络环境，如内网、VPN
- 使用 SSH 连接 Docker 守护进程
- **使用 TLS(HTTPS) 连接 Docker 守护进程**

## 使用 OpenSSL 创建 CA、服务器和客户端密钥

```shell
# 首先，在 Docker 守护进程的主机上，生成 CA 私钥和公钥
export HOST="your_host"

openssl genrsa -aes256 -out ca-key.pem 4096
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
# 确保 Common Name 与主机名相同
# Common Name (e.g. server FQDN or YOUR name) []:$HOST

# 创建服务器密钥和证书签名请求 (CSR)
openssl genrsa -out server-key.pem 4096
openssl req -subj "/CN=$HOST" -sha256 -new -key server-key.pem -out server.csr

# 使用 CA 签署公钥
# 由于 TLS 连接可以通过 IP 地址和 DNS 名称进行，因此在创建证书时需要指定 IP 地址。
echo subjectAltName = DNS:$HOST,IP:$HOST,IP:127.0.0.1 >> extfile.cnf
echo extendedKeyUsage = serverAuth >> extfile.cnf
# 生成签名证书
openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem \
  -CAcreateserial -out server-cert.pem -extfile extfile.cnf
  
# 创建客户端密钥和证书签名请求
openssl genrsa -out key.pem 4096
openssl req -subj '/CN=client' -new -key key.pem -out client.csr

echo extendedKeyUsage = clientAuth > extfile-client.cnf
# 生成签名证书
openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem \
  -CAcreateserial -out cert.pem -extfile extfile-client.cnf
  
# 生成 cert.pem, server-cert.pem 后，您可以安全地删除两个证书签名请求和扩展配置文件
rm -v client.csr server.csr extfile.cnf extfile-client.cnf

# 开启受保护的 dockerd 守护进程
dockerd \
    --tlsverify \
    --tlscacert=ca.pem \
    --tlscert=server-cert.pem \
    --tlskey=server-key.pem \
    -H=0.0.0.0:2376
```

## 默认开启 TLS

如果您想默认保护 Docker 客户端连接，您可以将文件移动到`.docker`主目录中的目录 --- 并设置 `DOCKER_HOST`和`DOCKER_TLS_VERIFY`变量（而不是在每次调用时传递 `-H=tcp://$HOST:2376`和`--tlsverify`）。

```shell
mkdir -pv ~/.docker
cp -v {ca,cert,key}.pem ~/.docker

export DOCKER_HOST=tcp://$HOST:2376 DOCKER_TLS_VERIFY=1
```

## 使用 CURL 验证

```shell
curl https://$HOST:2376/images/json --cert cert.pem --key key.pem --cacert ca.pem
```
