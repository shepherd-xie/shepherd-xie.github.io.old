---
title: 常见的Content-Type
date: 2018-11-13 10:43:00
tags: 
categories: 
top:
---

# 常见的Content-Type属性

<!-- more -->

## x-www-form-urlencoded

最常见的 `POST` 上传数据方式，浏览器原生表单如果不设置 `enctype` 就会以此种方式提交数据，需要上传的数据会以 `key=value` 的格式进行编码，随后进行 `url` 转码。

```http
POST http://www.example.com HTTP/1.1
Content-Type: application/x-www-form-urlencoded;charset=utf-8

title=test&sub%5B%5D=1&sub%5B%5D=2&sub%5B%5D=3
```

在使用 `Ajax` 提交数据是，也是使用这种方式。

## multipart/form-data

进行文件上传时，必须将表单 `enctype` 设为此值。

```http
POST http://www.example.com HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryrGKCBY7qhFd3TrwA

------WebKitFormBoundaryrGKCBY7qhFd3TrwA
Content-Disposition: form-data; name="text"

title
------WebKitFormBoundaryrGKCBY7qhFd3TrwA
Content-Disposition: form-data; name="file"; filename="chrome.png"
Content-Type: image/png

PNG ... content of chrome.png ...
------WebKitFormBoundaryrGKCBY7qhFd3TrwA--

```

## application/json

用于传递 `JSON` 数据，表名传递的数据是序列化后的 `JSON` 字符串

```http
POST http://www.example.com HTTP/1.1
Content-Type: application/json;charset=utf-8

{"title":"test","sub":[1,2,3]}
```

## text/xml

作为 `XML-RPC` 传输的协议，用于 `XML` 远程过程调用

```http
POST http://www.example.com HTTP/1.1
Content-Type: text/xml

<?xml version="1.0"?>
<methodCall>
    <methodName>examples.getStateName</methodName>
    <params>
        <param>
            <value><i4>41</i4></value>
        </param>
    </params>
</methodCall>
```

