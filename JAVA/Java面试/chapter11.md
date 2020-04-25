# Spring Boot/Spring Cloud
## 104.什么是 spring boot？
---
springboot就是Spring开源框架下的子项目，是Spring的一站式解决方案，主要是简化了spring的使用难度，降低了对配置文件的要求，使得开发人员能够更容易得上手

## 105.为什么要用 spring boot？
---
- 简化了Spring配置文件，
- 没有代码和XML文件的生成
- 内置TomCat
- 能够独立运行
- 简化监控

## 106.spring boot 核心配置文件是什么？
---
1. Spring Boot 有两种类型的配置文件，application 和 bootstrap 文件
2. Spring Boot会自动加载classpath目前下的这两个文件，文件格式为 properties 或 yml 格式
- *.properties 文件是 key=value 的形式
- *.yml 是 key: value 的形式
- *.yml 加载的属性是有顺序的，但不支持 @PropertySource 注解来导入配置，一般推荐用yml文件，看下来更加形象
3. bootstrap 配置文件是系统级别的，用来加载外部配置，如配置中心的配置信息，也可以用来定义系统不会变化的属性.bootstatp 文件的加载先于application文件
4. application 配置文件是应用级别的，是当前应用的配置文件

## 107.spring boot 配置文件有哪几种类型？它们有什么区别？
---
1. SpringBoot的核心配置文件有哪些？
    SpringBoot的核心配置文件有application和bootstarp配置文件。

2. 他们的区别是什么？
application文件主要用于Springboot自动化配置文件。

bootstarp文件主要有以下几种用途：
- 使用Spring Cloud Config注册中心时 需要在bootStarp配置文件中添加链接到配置中心的配置属性来加载外部配置中心的配置信息。
- 一些固定的不能被覆盖的属性
- 一些加密/解密的场景
3. 都有什么格式？
    - .properties 和 .yml
    - .yml采取的是缩进的格式 不支持@PeopertySource注解导入配置
## 108.spring boot 有哪些方式可以实现热部署？
---
- Spring Loaded
- spring-boot-devtools
- JRebel插件 

## 109.jpa 和 hibernate 有什么区别？
---
- Hibernate是JPA规范的一个具体实现

- hibernate有JPA没有的特性 

- hibernate 的效率更快

- JPA 有更好的移植性，通用性

## 110.什么是 spring cloud？
---
- Spring Cloud是一个微服务框架，相比Dubbo等RPC框架, Spring Cloud提供的全套的分布式系统解决方案。 

- Spring Cloud对微服务基础框架Netflix的多个开源组件进行了封装，同时又实现了和云端平台以及和Spring Boot开发框架的集成。 

- Spring Cloud为微服务架构开发涉及的配置管理，服务治理，熔断机制，智能路由，微代理，控制总线，一次性token，全局一致性锁，leader选举，分布式session，集群状态管理等操作提供了一种简单的开发方式。

- Spring Cloud 为开发者提供了快速构建分布式系统的工具，开发者可以快速的启动服务或构建应用、同时能够快速和云平台资源进行对接。 

## 111.spring cloud 断路器的作用是什么？
---
- 当一个服务调用另一个服务由于网络原因或自身原因出现问题，调用者就会等待被调用者的响应 当更多的服务请求到这些资源导致更多的请求等待，发生连锁效应（雪崩效应）

- 断路器有完全打开状态:一段时间内 达到一定的次数无法调用 并且多次监测没有恢复的迹象 断路器完全打开 那么下次请求就不会请求到该服务

- 半开:短时间内 有恢复迹象 断路器会将部分请求发给该服务，正常调用时 断路器关闭

- 关闭：当服务一直处于正常状态 能正常调用


## 112.spring cloud 的核心组件有哪些？
---
- Eureka：服务注册于发现。

- Feign：基于动态代理机制，根据注解和选择的机器，拼接请求 url 地址，发起请求。

- Ribbon：实现负载均衡，从一个服务的多台机器中选择一台。

- Hystrix：提供线程池，不同的服务走不同的线程池，实现了不同服务调用的隔离，避免了服务雪崩的问题。

- Zuul：网关管理，由 Zuul 网关转发请求给对应的服务。