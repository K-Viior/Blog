---
title: Golang基础语法——常量和枚举
date: 2023-02-21 15:12:04
categories:
- Golang
---
# Golang基础语法——常量和枚举

Go语言的常量使用`const`关键字来定义
常量是一个简单值的标识符，在程序运行时，不会被修改的量。
常量的定义格式
```Go
const identifier [type] = value
```
你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。

```Go
const filename string = "123.txt"
const a,b int = 1,2
//常量的类型也可以不规定
const file = "abc.txt"
const c,d = 3,4
//这样常量的类型就是不确定的，在进行运算时会自己判断使用可行的类型
```
常量还可以用作枚举：
```Go
const (
	UnKnow = 0
	Female = 1
	Male = 2
)
```

常量可以用**len(), cap(), unsafe.Sizeof()** 常量计算表达式的值。常量表达式中，函数必须是内置函数，否则编译不过：
```Go
package main
  
import "unsafe"
const (

    a = "abc"

    b = len(a)

    c = unsafe.Sizeof(a)

)
  

func main(){

	println(a, b, c)

}
```
输出结果为：abc, 3, 16

**unsafe.Sizeof(a)输出的结果是16 。
字符串类型在 go 里是个结构, 包含指向底层数组的指针和长度,这两部分每部分都是 8 个字节，所以字符串类型大小为 16 个字节。**

# 自增长
在 golang 中，一个方便的习惯就是使用`iota` 标示符，它简化了常量用于增长数字的定义，给以上相同的值以准确的分类。
```Go
const(
	CategoryBooks = 1
	CategoryHealth = 2
	CategoryClothing = 3
)
//这样定义值的方式十分繁琐不变，我们可以直接使用iota标识第一个常量，使之后的常量进行递增
const(
	CategoryBooks = iota //  0
	CategoryHealth       //  1
	CategoryClothing     //  2
)
//也可以使用 _ 跳过中间的值
func enums() {
	const(
		cpp = iota
		_
		python
		golang
		java
		)
	fmt.Println(cpp,java,python,golang)
}
//最终运行结果为0,4,2,3

```

# iota和表达式

iota可以做更多事情，而不仅仅是 increment。更精确地说，iota总是用于 increment，但是它可以用于表达式，在常量中的存储结果值。

```Go
// b, kb, mb, gb, tb, pb
const (
	b = 1 << (10 * iota)
	kb
	mb
	gb
	tb
	pb
)
```
结果如下
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221153457.png)
这个工作是因为当你在一个const组中仅仅有一个标示符在一行的时候，它将使用增长的iota取得前面的表达式并且再运用它，。在 Go 语言的[spec](https://legacy.gitbook.com/book/aceld/how-do-go/edit#)中， 这就是所谓的隐性重复最后一个非空的表达式列表.

当你在把两个常量定义在一行的时候会发生什么？
```GO
const(
	Apple, Banana = iota + 1, iota + 2
	Cherimoya, Durian
	Elderberry, Fig
)
```
在下一行增长，而不是立即取得它的引用。
```Go
// Apple: 1

// Banana: 2

// Cherimoya: 2

// Durian: 3

// Elderberry: 3

// Fig: 4
```


![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221153642.png)
