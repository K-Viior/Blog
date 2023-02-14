---
title: SpringMVC中同过转发将数据传递到前台的方式
date: 2023-02-14 16:40:54
tags:
- Spring
categories:
- java面试
---
#### 1、直接使用request域传递数据
`request.setAttribuate("name",value);`
#### 2、使用model进行传值，底层会将数据存放到request域进行数据的传递
`model.addAttribute("name",value);`
#### 3、使用modelMap进行传值，底层同样会将数据放到request域中进行传递
`modelMap.put("name",value);`
#### 4、借用modelAndView,在其中设置数据和视图
```java
mv.addObject("name",value);
mv.setView("success");
return mv;
```