---
layout: post
title: Spring用到的设计模式
category: Java
tags: [Java]
---

设计模式表示面向对象开发中最好的编程实践。Spring框架中广泛使用了很多的设计模式，以达到框架的灵活扩展，高度抽象。

## 本文通过如下论述

1. 控制反转（IOC）和依赖注入（DI）
2. 工厂设计模式
3. 单例模式
4. 代理模式
5. 模板方法
6. 观察者模式
7. 适配器模式
8. 装饰者模式

## 控制反转（IOC）和依赖注入（DI）

IoC（Inverson of Control）是一种解耦的思想，是一种原则，不是一个模式

DI是实现控制反转的一种设计模式，依赖注入是将一个实例变量传入到一个对象中去

## 工厂设计模式

Spring使用工厂模式可以通过 BeanFactory 或 ApplicationContext 创建bean对象

## 单例模式

省去创建对象所需要的时间，不频繁new对象对系统内存使用频率也会降低

Spring使用ConcurrentHashMap实现单例注册表的特殊方式实现单例模式

## 代理模式

AOP，面向切面编程，能够将与业务无关，却为业务模块所共同调用的逻辑（事务处理、日志管理、权限控制）封装起来，便于减少系统重复代码，降低模块耦合度，增强可扩展性和可维护性

Spring AOP基于动态代理，如果要代理的对象实现了摸个接口，那么Spring APO会使用JDK Proxy去创建代理对象，而对于没有实现接口的对象，使用Cglib生成一个被代理对象的子类来作为代理

## 模板方法

一般情况下，我们都使用继承的方式来实现模板方法，但Spring使用Callback模式与模板方法配合

jdbcTemplate，hibernateTemplate

## 观察者模式

Spring事件驱动模型就是基于观察者模式的实现

事件角色：ApplicationEvent
ContextStartedEvent、ContextStoppedEvent、ContextRefreshedEvent、ContextCloseEvent
事件监听者：ApplicationListener
事件发布者：ApplicationEventPublisher

## 适配器模式

适配器模式将一个接口转换成希望的另一个接口，别名包装器（Wrapper）；Spring MVC中DispatcherServlet根据HandlerMapping，解析到对应的Handler

## 装饰者模式

装饰者模式可以动态地给对象添加一些额外的属性和行为。


















