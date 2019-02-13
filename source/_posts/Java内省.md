---
title: Java内省机制
date: 2018-10-25 16:02:00
tags: 
categories: 
top:
---

## Java内省机制

内省( Introspector ) 是 Java 语言对 JavaBean 类属性、事件的一种缺省处理方法。

JavaBean是一种特殊的类，主要用于传递数据信息，这种类中的方法主要用于访问私有的字段，且方法名符合某种命名规则。如果在两个模块之间传递信息，可以将信息封装进 JavaBean 中，这种对象称为“值对象”( Value Object )，或“ VO ”。方法比较少。这些信息储存在类的私有变量中，通过 `setter/getter` 获得。

Java JDK 中提供了一套 API 用来访问某个属性的 `getter/setter` 方法，这就是内省。

## JDK内省类库

### `PropertyDescriptor`类

`PropertyDescriptor`类表示 JavaBean 类通过存储器导出一个属性。主要方法：

1. `getPropertyType()`，获得属性的 Class 对象;
2. `getReadMethod()`，获得用于读取属性值的方法；
3. `getWriteMethod()`，获得用于写入属性值的方法;
4. `hashCode()`，获取对象的哈希值;
5. `setReadMethod(Method readMethod)`，设置用于读取属性值的方法;
6. `setWriteMethod(Method writeMethod)`，设置用于写入属性值的方法。

### `Introspector`类

将 JavaBean 中的属性封装起来进行操作。在程序把一个类当做 JavaBean 来看，就是调用 `Introspector.getBeanInfo()` 方法，得到的 BeanInfo 对象封装了把这个类当做 JavaBean 看的结果信息，即属性的信息。

### `BeanUtils`工具包

Apache开发了一套简单、易用的API来操作Bean的属性——BeanUtils工具包。

[BeanUtils工具包](http://commons.apache.org/beanutils/)

注意：应用的时候还需要一个[logging包](http://commons.apache.org/logging/)