---
title: IDEA错误
date: 2018-11-06 10:38:00
tags: 
categories: IDEA
top:
---

<!-- more -->

## 1. Cannot start compilation: the output path is not specified for module "XXX". Specify the output path in Configure Project.

未定义编译路径，进入 `Project Structure(Ctrl + Shift + Alt + S)` -> `Project Settrings` -> `Modules` -> 左侧选中选中 Module ，点击 Paths 在 Compiler output 中设置 Use module compile output path

![](assets/idea_error_1.png)