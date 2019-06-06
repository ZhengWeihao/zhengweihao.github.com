---
layout:     post
title:      "工厂模式"
subtitle:   ""
date:       2018-02-22 20:11:27
author:     "ZhengWeihao"
header-img: "imgs/post-bg-default.jpg"
tags:
    - Java
    - DesignPattern
---

工厂模式
---

* 目的：
  * 将一系列相关的类（继承关系），统一管理起来，以便于维护和整体把控
* 简单工厂模式：
  * ![simple-factory](/imgs/designPattern/factory/simple-factory.png)
  * 优点：
  * 缺点：
    * 不易扩展
* 工厂模式：
  - ![simple-factory](/imgs/designPattern/factory/function-factory.png)
  - 优点：
  - 缺点：
    - 需要传参
* 抽象工厂模式：
  - ![simple-factory](/imgs/designPattern/factory/abstract-factory.png)
  - 优点：
    - 不需要传参
    - 易于扩展
  - 缺点：
* 应用：
  * Spring：DefaultListableBeanFactory
  * MyBatis：SessionFactory
  * Executors


* 单例模式：
  * 饿汉式
  * 懒汉式
  * 注册登记式

* 原型模式：




