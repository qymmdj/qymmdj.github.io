---
layout: post
title:  "揭开spring cloud的神秘面纱"
date:   2018-12-21 17:31:00 +0800
categories: 微服务
tag: springcloud
---

* content
{:toc}

#### 一：前言 	{#start}

这篇帖子中《揭开spring cloud的神秘面纱系列之一：对spring cloud的》中提到了今年非常热门的微服务开发框架SpringCloud，其中所涉及的技术springboot、springCloud，NetFlix OSS 微服务工具，由于是新技术，很多人对它们有很多误解，没有搞清楚他们的关系，而我们部分的微服务项目马上就要进入开发阶段，如果对springBoot没有一些基本认识和了解，就使用springCloud进行微服务开发，项目肯定会存在风险。这篇文章进一步对SpringBoot和SpringCloud进行更深一层分析，识别出问题和风险，然后设计一套易上手的开发框架，来提升编码效率，测试运维效率、版本发布和升级效率，来降低项目风险，缩短项目开发周期。

####  二、Spring、SpringBoot、SpringCloud、Netflix OSS             	{#spring}

研究他们之前，先得看下这些顶级开源项目背后支撑团队到底都是些什么样的组织。根据 Rod Johnson《Expert one on one J2EE design and development》书的思想 ， Rod Johnson开发了sprigFrameWork 并成立 interface21公司，08年改名 springSource，并且收购了 RabbitMQ，redis，主要提供咨询和培训服务。09年 Vmware 收购springSource ,13年Vmware和Emc合资成立 GoPivotal公司，简称Pivotal。打开spring 官网http://spring.io/，能看到下面图标

![image](https://spring.io/img/spring-logo-3b6f842fa77c3bea3bac17dbce36a101.png)

可以说spring 官网上能看到产品都是Privotal 公司提供的，包括spring、springBoot、SpringCloud等等。这些开源项目源码贡献者大部分都是Privotal 公司的员工，还有云原生、DevOps理论的提出者都在这个公司。

Netflix OSS(Netflix Open Source Software Center)，即Netflix公司开源的产品（http://netflix.github.io/），这个网站里内容都是Netflix 开源的产品，其中就有开发微服务所需要的一套组件（服务发现注册、配置中心、消息总线、负载均衡、断路器、API 网关)，这些组件都是可以独立运行的，而且不是基于spring开发的。

  那么Spring、SpringBoot、SpringCloud都解决了什么问题，之间的关系是什么？ 先从一个实际项目开发步骤来分析下，假设我们要开发一个mvc web应用,经常会使用SSM框架(Spring+SpringMVC+MyBatis)，那么整合这个框架，可能需要经过以下步骤：




1：需要一个spring主配置文件applicationContext.xml：配置事务，数据源、bean、拦截器
2：一个springMvc配置文件，并配置SpringMvc的视图解析
3：mybatis 自身配置文件，以及和spring 结合的spring配置文件，配置插件、mapper、数据获取方式等。
4：如果不是通过maven管理，还需要下载spring和mybatis整合所需的jar包，还要关心版本。
5：配置 web.xml
6：一个web容器
7：部署
从步骤可以到要构建一个产品级的应用，用spring来整合springMvc、mybatis框架，还是需要较多配置文件，配置的内容也会比较多，整合过程中会出现各种问题，需要花时间解决，相对比较麻烦、费时。spring设计的初衷就是不要重复造轮子，着眼于轻便、灵巧，易于开发、测试和部署的轻量级开发框架，然而随着spring的发展以及各种框架的层出不穷，spring整合这些框架会越来越复杂，逐渐背离初衷。
这个时候Pivotal团队就设计了全新的框架springBoot，目的是解决spring集成（整合）各种框架越来越复杂的问题，通过少量的代码就能创建一个独立的、产品级别的Spring应用。

 
springBoot 并非重复造轮子，并不是说spring有缺点，就重新开发了一个框架来替代spring，spring框架本身还是非常优秀的：比喻它的开放式设计架构，才会使得基于spring框架扩展的框架非常多，任何框架都可以通过spring的开放式设计进行扩展。springBoot也正是利用spring的开放式设计，基于spring开发了springBoot，springBoot只是一个脚手架，用来快速集成第三方框架，做到开箱即用，无需或少量配置。
如果是用springBoot整合SSM框架，那么以上7步一个都不需要，只需要配置一个连接池，然后在maven pom添加依赖，通过springBoot这个脚手架将各个组件黏合在一起，使其可以条不紊地运行，web容器也不需要了，因为已经内嵌了。通过以下main方法来启动应用，包括web容器一起启动：

@SpringBootApplication(scanBasePackages = {"com.sitech"})
public class App 
{
        public static void main(String[] args) {

            SpringApplication.run(App.class, args);
        }
}

springCloud是什么？ 说白了就是利用springBoot集成第三方框架的便利性，使用springBoot开发框架将Netflix OSS 中微服务组件进行了集成，简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，但是Netflix OSS套件并没有业务功能服务开发框架，只是提供基础设施组件（和业务无关）。那么业务服务开发框架由谁提供，SpringBoot吗？当然不是，SpringBoot只是集成第三方框架，自己本身不重复造轮子，基本上基于springCloud的业务服务开发还是使用传统SpringMvc框架，但在SpringBoot 2.0集成了Spring5.0提供的WebFlux新的开发框架，业务服务开发框架算是有了第二选择，但是只能选其一。