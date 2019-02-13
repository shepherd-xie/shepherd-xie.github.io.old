---
title: Shiro
date: 2018-10-09 15:02:00
tags: 
categories: 
top:
---

## *参考*[Apache Shiro Reference Documentation](https://shiro.apache.org/reference.html#apache-shiro-reference-documentation)

## 1. QuickStart

Apache Shiro是一个具有许多功能的综合应用程序安全框架。

![img](https://shiro.apache.org/assets/images/ShiroFeatures.png)

Shiro针对Shiro开发团队所称的“应用程序安全的四大基石” - 身份验证，授权，会话管理和加密：

* **Authentication：**有时也称为“登录”，这是证明用户是他们所说的人的行为。
* **Authorization：**访问控制的过程，即确定“谁”可以访问“什么”。
* **Session Management：**即使在非Web或EJB应用程序中，也可以管理特定于用户的会话。
* **Cryptography：**使用加密算法保持数据安全，同时仍然易于使用。

在不同的应用程序环境中还有其他功能可以支持和强化这些问题，尤其是：

* Web支持：Shiro的Web支持API可帮助轻松保护Web应用程序。
* 缓存：缓存是Apache Shiro API中的第一层公民，可确保安全操作保持快速高效。
* 并发：Apache Shiro支持具有并发功能的多线程应用程序。
* 测试：存在测试支持以帮助您编写单元和集成测试，并确保您的代码按预期受到保护。
* “运行方式”：允许用户假定其他用户的身份（如果允许）的功能，有时在管理方案中很有用。
* “记住我”：记住用户在会话中的身份，这样他们只需要在强制要求时登录。

## 2.