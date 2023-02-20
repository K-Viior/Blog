---
title: Golang基础语法
date: 2023-02-20 20:34:32
categories：
- Golang
---
# Go的语言特性
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220203722.png)
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220203731.png)
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220203740.png)
## Go适合做什么
### 云计算基础设施领域
代表项目：docker、kubernetes、etcd、consul、cloudflare CDN、七牛云存储等。
### 基础后端软件
代表项目：tidb、influxdb、cockroachdb等。
### 微服务
代表项目：go-kit、micro、monzo bank的typhon、bilibili等。
### 互联网基础设施
代表项目：以太坊、hyperledger等。
## Go的不足
- 包管理，大部分包都在github上
- 所有Excepiton都用Error来处理(比较有争议)。
- 对C的降级处理，并非无缝，没有C降级到asm那么完美(序列化问题)
# Golang的基础语法
## 定义变量

**使用==var==关键字定义变量**

Go语言的变量若没有给定初始值，则会自带一个初始值
```Golang
func variableZeroValue() {
	var a int  //go语言的变量在定义后会有一个初始值，int为0
	var s string   //string为空字符串
	fmt.println(a , s)
}
```
Go语言的变量给定初始值，可以使用`:=`在定义时直接赋值
同时Go语言可以自动判断声明值的类型，所以可以不加类型
并且Go语言可以同时声明多个不同类型的变量
```Golang
func variableInitialValue() {
var a int = 1
var s int = "abc"
//也可以使用:=直接声明赋值，并且会自动判断类型
b := 1
c := "abc"
//go语言可以同时声明多个不同类型的变量,也可以直接用:=赋值
var d, e, f, g = 1, true, "DEF", a
// d, e, f, g := 1, true, "DEF", a
d = 5
}
```
**注意：在函数内定义变量可以使用`:=`，但是在函数外定义变量需要使用`var 变量名 变量类型`的方式，同时在函数外定义的变量作用域在包内，没有全局变量的概念**
```Golang
package main
//可以一行一行定义，也可以放到括号内一起定义，简化书写
var (
aa = 3
bb = "abc"
cc = false
)
```

使用下划线`_`可以将变量废弃
```Golang
func main() {

_, value := 7, 5 //实际上7的值被废弃了，变量_不具备读特性
//fmt.println(_)  变量_是读不出来的
 fmt.println(value)  //结果为5
}
```
