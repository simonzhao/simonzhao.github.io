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


## 主要层次


### 资源注册与发现

基于 Spring Cloud Eureka 搭建，对外提供查询、注册、心跳共3个接口，它是整个系统的核心，除了负载均衡层，其他应用都需要向它进行注册。
任何调用前服务的查询也都会经过它。多个实例应该组成集群，统一提供服务。如果它崩溃了，整个系统将面临灾难。
客户端负载均衡会对查询到的服务信息进行本地缓存，对短时间内对崩溃提供了容错。

### 负载均衡层

负责将内部的微服务向公网提供服务，允许用户通过App，浏览器等使用RESTFul调用服务。
负载均衡层常见的解决方案有：

* Nginx 反向代理
* LVS
* ECMP

本项目是项目初期采用Nginx反向代理方案，同时提供HTTPS，保障用户信息的安全性。

### API 网关层

基于 Spring Cloud Zuul 实现，负载均衡层将流量转发到 Zuul，
根据URL中的信息，Zuul向Eureka查询服务地址，按照配置的负载均衡算法，将请求转发到具体的服务实例中。


### 微服务实例层

基于Spring Boot搭建具体的微服务实例，主要分为两大类，实体对象服务和中间件服务。
实体服务解决业务中涉及到的实体对象，提供对象的增删改查，向业务服务屏蔽数据的存储细节。
中间件服务，提供与业务无关的底层服务，比如“全局唯一ID服务”，“敏感词屏蔽”这类底层功能。


## 业务故事与数据流转
