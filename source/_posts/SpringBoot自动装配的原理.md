---
title: SpringBoot自动装配的原理
date: 2023-02-18 09:52:11
tags:
- Spring
categories:
- java面试
---
Spring boot 项目中的引导类上有个一注解@SpringBootApplication，这个注解是对三个注解进行了封装
<!--more-->
分别是
- @SpringBootConfiguration
- @EnableAutoConfiguration
- @ComponentScan

其中 **@EnableAutoConfiguration**是实现自动装配的核心注解。
该注解通过`@import`注解导入对应的配置选择器。其内部就是读取了该项目和该项目所引用jar包下的classpath路径下的**META-INF/spring.factories**文件中所配置类的全类名。
在这些配置类中所定义的bean会根据条件注解所指定的条件来决定，是否要将其导入到Spring容器中。

一般条件判断会有像`@ConditionalOnClass`这样的注解，判断是否有对应的Class文件，如果有则加载该类，把这个配置类的所有bean放到Spring容器中使用。

