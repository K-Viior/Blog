---
title: Golang基础语法——map
date: 2023-02-22 19:08:13
categories:
- Golang
---
# Golang基础语法——map
首先map的key在map中是无序，本质是一个hashmap
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222193156.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222193327.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222193428.png)

## map的定义
```Go
m := map[string]string{   //直接声明
        "one":   "java",
        "two":   "golang",
        "three": "python",
    }
	//使用make声明一个空的map
    m2 := make(map[string]string) //m2 == empty map
    
	//在使用map前，需要先make，make的作用就是给map分配数据空间
	test1 = make(map[string]string, 10)
	test1["one"] = "php"
	test1["two"] = "golang"
	test1["three"] = "java"
	fmt.Println(test1) //map[two:golang three:java one:php]

    var m3 map[string]string // m3 == nil

	//map的value可以是map类型
	var m4 map[string]map[string]int
    fmt.Printf("m4: %v\n", m4) //m4:map[]

```

## map的操作
```Go
    //遍历map
    for k, v := range m {
        fmt.Println(k, v)
    }

    //也可以只取key
    for k := range m {
        fmt.Println(k)
    }

    //value当然也可以
    for _, v := range m {
        fmt.Println(v)
    }
```

## 判断map的key是否存在

```Go
    one,ok := m["one"]
    fmt.Println(one,ok)
    four := m["four"]
    fmt.Printf("four: %v\n", four) // 打印的是zero Value

    // 可以使用双返回值获取key是否存在，来进行判断
    if four,ok := m["four"]; ok {
        fmt.Printf("four: %v\n", four)
    }else {
        fmt.Println("key doesn't exist")
    }
```

## 对map的value进行修改
```Go
language := make(map[string]map[string]string)
language["php"] = make(map[string]string, 2)
language["php"]["id"] = "1"
language["php"]["desc"] = "php是世界上最美的语言"
language["golang"] = make(map[string]string, 2)
language["golang"]["id"] = "2"
language["golang"]["desc"] = "golang抗并发非常good"

  
language["php"]["id"] = "3" //修改了php子元素的id值
language["php"]["nickname"] = "啪啪啪" //增加php元素里的nickname值
delete(language, "php") //删除了php子元素
fmt.Println(language)

```

## 算法题：寻找最长不含有重复字符的子串
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222200014.png)

# rune

```Go
package main

import (
   "fmt"
)

func main() {
    s := "123木头人"

    for i,char := range []rune(s) {
        fmt.Printf("(%d,%c)", i,char)
    }

}
```
输出结果：(0,1)(1,2)(2,3)(3,木)(4,头)(5,人)


![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222201001.png)

Go语言对字符串进行操作在strings包下
部分api如下
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222201226.png)
