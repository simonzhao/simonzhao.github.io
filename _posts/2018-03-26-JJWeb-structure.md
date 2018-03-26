---
layout:       post
title:        "微服务Web系统架构"
subtitle:     "基于 Spring Cloud 搭建"
date:         2018-03-26 15:29:00
author:       "赵影楠"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Spring Boot
    - Spring Cloud
    - Web架构
    - Java
---

> 2017年开始，随着使用的微服务越来越多，服务治理问题提上了日程。
> 基于 Spring Cloud 搭建Web架构简单可扩展。
> 但是，由于运行于 RESTFul 协议之上，在网络IO上性能相比基于自定义的二进制协议较低。


## 系统架构示意图

![](/img/in-post/f2d133/hong_guan_jia_gou_tu.jpg)


## 使用组件

* LVS
* Spring Boot
* Spring Cloud Zuul
* Spring Cloud Eureka
* Spring Cloud Hystrix
* Spring Cloud Ribbon
* [Spring Cloud Feign](https://github.com/OpenFeign/feign) 
Feign是一种声明式、模板化的HTTP客户端。
