---
title: 如何设计一个库存系统——以自营超市库存管理为例
date: 2023-08-03 15:29:36
tags:
categories:
top:
---

库存领域的业务相对来说是一个专业且复杂的领域，目前也有许多的通用库存管理系统，但是仍然存在许多企业去自建属于自己的库存系统。

相对于购买通用系统来说，自建系统虽然前期需要投入相当资源，但是对于自身库存的定制化处理以及仓储行为的处理会更加游刃有余。

本文以自营超市的库存管理为例，简单的介绍一下如何设计一个库存系统，一方面是理清自己的思路，另一方面在输出的过程中也会发现自己的不足。

<!-- more -->

# 前言

关于库存系统这个命题，一部分原因来自于上一份工作本身会涉及到这部分的业务。

众所周知，在软件学习过程中我们或多或少都会接触到电商系统的设计与开发。因为电商系统足够的亲民，我们每一个人都使用过，了解它的运行过程。同时电商系统又有足够的包容性，我们所学习的绝大多数架构系统设计都可以在电商系统中运用，或者说正是电商系统的发展，才使得这一类技术如此的生机蓬勃。

而库存系统作为电商系统的一个重要子系统，它处在一个偏向底层的位置。众所周知，当一个系统越是处于底层架构之中，那么它的稳定性与可靠性就尤其要得到保障，如果要修改这种底层系统，那么花费的代价无疑是十分巨大。

所以我们要在系统的设计之初就要对其业务、工作场景及其核心模块有较为清晰的理解。

# 运行场景

我们以一个自营超市库存管理为例。某大型超市自有仓库，需要进行线上销售，拥有自有仓库，不考虑线下销售对仓库库存影响，大致购买流程如下：

* 顾客在线上平台购买商品，在商品数量充足时产生订单；
* 订单经过仓管人员确认，或当天统一结算确认之后发货；
* 订单支持更改订单、退货、退款等操作；
* 库存定时由线下人员进行盘点，统一线上线下库存情况；
* 由门店人员向供应商发起采购；

# 业务分析

 根据以上业务逻辑，我们大致可以画出一下的业务架构图。

![未命名文件](https://images.orkva.com/images/2023/08/03/be8ae0537891eb46d0ff5c11202a2fe5.png)

需要注意的是最下方的基于批次的库存，对于采购来说，通常情况下我们将同一次入库的 sku 定义为同一批次，同一批次的 sku 它们在系统层面是等价的，它们具有相同的性质，如生产日期、保质期等。

库存数量分为逻辑数量与物理数量，具体可分为：

* 批次数量
* 剩余量
* 可用量（逻辑数量）
* 锁定量（逻辑数量）
* 出库量

它们之间的关系为

```
批次数量 = 剩余量 + 出库量
剩余量 = 可用量 + 锁定量
批次数量 = 可用量 + 锁定量 + 出库量
```

它们之间的变化关系大致如下表所示

|          | 批次数量 | 剩余量   | 可用量   | 锁定量 | 出库量 |
| -------- | -------- | -------- | -------- | ------ | ------ |
| 采购     | 新增     | 批次数量 | 批次数量 | 0      | 0      |
| 创建订单 | -        | -        | 减少     | 增加   | -      |
| 出库     | -        | 减少     | -        | 减少   | 增加   |
| 退货     | -        | 增加     | 增加     | -      | 减少   |



> 相关参考：
>
> * [产品经理：库存管理系统如何设计？](https://www.niaogebiji.com/article-105228-1.html)