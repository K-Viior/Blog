---
title: Spring Bean的生命周期
date: 2023-02-14 16:27:09
tags: 
- Spring
categories:
- java面试
---
#### 1、创建前准备阶段
这段阶段的主要作用是，在Bean在开始加载之前，通过上下文和相关配置解析并查找bean的相关扩展实现
#### 2、创建实例阶段
这个阶段主要是通过反射来创建bean的实例对象，并且扫描和分析bean声明的一些属性
#### 3、依赖注入阶段
如果被实例化的bean有依赖其他bean的现象，则需要对被依赖的bean进行依赖注入。
如@Autowired、@Setter等依赖注入的配置形式
#### 4、容器缓存阶段（初始化阶段）
容器缓存阶段主要是将bean保存到容器以及Spring缓存中，到了这个阶段，bean就可以被开发者使用了
#### 5、销毁实例阶段
当Spring应用上下文关闭时，该上下文中的所有bean都会被销毁


### 在实例化和初始化前后都可以放置beanpostProcessor,对bean做各式各样的修改
