---
title: Spring 中的 BeanFactory 和 FactoryBean
categories: Spring
date: 2019-05-14 21:11:23
---

### BeanFactory
BeanFactory 是 Spring IOC 中最核心的接口之一，它负责对 Bean 的创建以及管理。

### FactoryBean
实现 FactoryBean 接口的对象是受容器管理的一个特殊的 Bean, 它可以自己再生成一个由我们自己负责其构建流程的 Bean。

### BeanFactory 和 ApplicationContext
BeanFactory接口只是定义了容器创建以及管理 Bean 等基础功能，ApplicationContext 继承了 BeanFactory接口, 并增加了许多额外功能，例如 AOP, 事件等功能。



