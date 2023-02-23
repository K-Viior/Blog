---
title: Golang基础语法——面向对象
date: 2023-02-23 15:51:49
categories:
- Golang
---
# Golang基础语法——面向对象

假设有两个方法，一个方法的接收者是指针类型，一个方法的接收者是值类型，那么：  
  
●对于值类型的变量和指针类型的变量，这两个方法有什么区别？  
●如果这两个方法是为了实现一个接口，那么这两个方法都可以调用吗？  
●如果方法是嵌入到其他结构体中的，那么上面两种情况又是怎样的？
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223170738.png)

```Go
package main

import "fmt"

//定义一个结构体
type T struct {
name string
}
  

func (t T) method1() {
t.name = "new name1"
}

func (t *T) method2() {
t.name = "new name2"
}


func main() {

t := T{"old name"}

fmt.Println("method1 调用前 ", t.name)
t.method1()
fmt.Println("method1 调用后 ", t.name)


fmt.Println("method2 调用前 ", t.name)
t.method2()
fmt.Println("method2 调用后 ", t.name)

}
```
method1 调用前 old name
method1 调用后 old name
method2 调用前 old name
method2 调用后 new name2
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223170712.png)

## 接收者

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223170948.png)


Go语言创建结构体的方法的方式，大体和普通创建方法的过程一样
但是在方法名前要添加接收者，代表是哪个结构体的方法
```Go
type mode struct{
    value int
}

func (mode *mode) setModValue1(value int) {
    mode.value = value
}

func (mode mode) setModValue2(value int) {
    mode.value = value
}

func main () {

    mode1 := mode{100}
    fmt.Printf("set1修改前: %v\n", mode1)
    mode1.setModValue2(200)
    fmt.Printf("set1修改后: %v\n", mode1)

    fmt.Printf("set2修改前: %v\n", mode1)
    mode1.setModValue1(200)
    fmt.Printf("set2修改后: %v\n", mode1)
}
```
打印结果为：
set1修改前: {100}
set1修改后: {100}
set2修改前: {100}
set2修改后: {200}

当调用t.method1()时相当于method1(t)，实参和行参都是类型 `T`，可以接受。此时在method1()中的t只是参数t的值拷贝，所以method1()的修改影响不到main中的t变量。  
  
当调用t.method2()=>method2(t)，这是将 T 类型传给了 `*T` 类型，go可能会取 t 的地址传进去：method2(&t)。所以 method1() 的修改可以影响 t。

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223170958.png)


```Go
package main

import "fmt" 

//结构体定义
type treeNode struct {
    value int
    left,right *treeNode
}

func createNode (value int) *treeNode{
    return &treeNode{value: value
}

//结构体的方法,其中的(node *treeNode)为接收者
func (node *treeNode) setValue (value int) {
    node.value = value
}

func (node treeNode) print () {
    fmt.Print(node.value," ")
}

//遍历node,使用中序遍历
func (node *treeNode) traverse() {
    if node == nil {
        return
    }

    node.left.traverse()
    node.print()
    node.right.traverse()

}

  
  

func main () {

    // var root treeNode
    root := treeNode{value: 3}

    root.left = &treeNode{}
    root.right = &treeNode{5,nil,nil}
    root.left.right = new(treeNode)
    root.right.left = createNode(2)

    root.traverse()
    fmt.Println()
    root.left.right.setValue(4)
    root.traverse()

}
```


![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223165153.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223171020.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230223171031.png)

