---
title: SpringMVC的主要组件和工作流程
date: 2023-02-14 16:32:51
tags:
- Spring
categories:
- java面试
---
#### 前端控制器（DispatcherServlet）
接受请求，相应结果，相当于转发器，有了DispatcherServlet 就减少了其它组件之间的耦合度。
#### 视图（View）
View 是一个接口， 它的实现类支持不同的视图类型，如 jsp，freemarker，pdf 等等
#### 视图解析器（ViewResolver）
进行视图的解析，根据视图逻辑名将 ModelAndView 解析成真正的视图（view）
#### 处理器（Handler）
处理业务逻辑的 Java 类
#### 处理器映射器（HandlerMapping）
根据请求的 URL 来查找 Handler
#### 处理器适配器（HandlerAdapter）
负责执行 Handler

#### 工作流程
浏览器的请求先经过前端控制器，他负责将请求分发到对应的controller。
controller处理完业务逻辑后，返回ModelAndView。
然后前端控制器找到一个或多个视图解析器，找到ModelAndView指定的视图，并把数据显示到客户端。
