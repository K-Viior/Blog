---
title: Golang基础语法——函数和指针
date: 2023-02-21 16:06:35
tags:
---
# Golang基础语法——函数和指针

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221201908.png)

## 函数返回多个值
```Go
package main 

import "fmt"

func swap(x, y string) (string, string) {

	return y, x

}
  
func main() {

	a, b := swap("Mahesh", "Kumar")

	fmt.Println(a, b)

}
```
以上的打印结果为**Kumar Mahesh**
```Go
package main

import "fmt"

//返回多个返回值，返回值只有参数类型

func foo2(a,b string) (int ,int) {

    fmt.Println(a)
    fmt.Println(b)
    return 666,999

}

//返回多个返回值，返回值有参数名称

func foo1(a, b string) (r1, r2 int) {

    fmt.Println(a)
    fmt.Println(b)
    r1 = 1000
    r2 = 2000
    return

}

func main() {
    fmt.Println(foo1("abc","efg"))
    fmt.Println(foo2("ABC","EFG"))
}
```
##  init函数与import
init 函数可在package main中，可在其他package中，可在同一个package中出现多次。

代码结构：
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221205007.png)


```Go
package lib1
import "fmt"
func Test () {
    fmt.Println("lib1...Test")
}
func init() {
    fmt.Println("lib1 ... init")
}

```

```Go
package lib2
import "fmt"

func Test () {
    fmt.Println("lib2...Test")
}  

func init() {
   fmt.Println("lib2...init")
}
```

```Go
package main

import (
    "go_First_practice/InitTest/lib1"
    "go_First_practice/InitTest/lib2"
)

func main() {
    lib1.Test()
    lib2.Test()
}
```

注意，被引用的函数，函数名首字母要大写才能够够被引用
## main函数
main 函数只能在package main中。且一个包内只有一个main函数能够作为程序的入口

## 执行顺序
golang里面有两个保留的函数：init函数（能够应用于所有的package）和main函数（只能应用于package main）。这两个函数在定义时不能有任何的参数和返回值。  
  
虽然一个package里面可以写任意多个init函数，但这无论是对于可读性还是以后的可维护性来说，我们都强烈建议用户在一个package中每个文件只写一个init函数。  
  
go程序会自动调用init()和main()，所以你不需要在任何地方调用这两个函数。每个package中的init函数都是可选的，但package main就必须包含一个main函数。  
  
程序的初始化和执行都起始于main包。  
  
如果main包还导入了其它的包，那么就会在编译时将它们依次导入。有时一个包会被多个包同时导入，那么它只会被导入一次（例如很多包可能都会用到fmt包，但它只会被导入一次，因为没有必要导入多次）。  
  
当一个包被导入时，如果该包还导入了其它的包，那么会先将其它包导入进来，然后再对这些包中的包级常量和变量进行初始化，接着执行init函数（如果有的话），依次类推。  
  
等所有被导入的包都加载完毕了，就会开始对main包中的包级常量和变量进行初始化，然后执行main包中的init函数（如果存在的话），最后执行main函数。下图详细地解释了整个执行过程：
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221204938.png)

# 函数的参数
函数如果使用参数，该变量可称为函数的形参。  
  
形参就像定义在函数体内的局部变量。  
  
调用函数，可以通过两种方式来传递参数：值传递和引用传递
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221210310.png)

最终打印结果为3,4
因为pass_by_value是自己复制了一份a，对自己的a 进行修改，并没有改变原本的a

而pass_by_ref是对a所存储的值进行修改，所以改变了a

---
Go语言可以用函数作为函数的参数

```Go
func apply(op func(a int, b int) int, a, b int) int {

    fmt.Println("Calling %s with %d, %d\n",

        runtime.FuncForPC(reflect.ValueOf(op).Pointer()).Name(), a, b)

    return op(a, b)

}
```

```Go
func main () {
fmt.Println(apply(func(a int, b int) int { //这里是使用类似java内部类的方式创建了一个函数
        return int(math.Pow(float64(a), float64(b)))
    }, 3, 4))
}
//打印结果为5
```


## 值传递
值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。
但是如果参数是自定义类型，或者参数非常大，就不会将整个参数进行拷贝，而是拷贝参数指向内容的指针

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221211525.png)


默认情况下，Go 语言使用的是值传递，即在调用过程中不会影响到实际参数。

```GO
/* 定义相互交换值的函数 */

func swap(x, y int) int {

	var temp int
	
	temp = x /* 保存 x 的值 */
	x = y /* 将 y 值赋给 x */
	y = temp /* 将 temp 值赋给 y*/

	return temp;

}
```

```Go
package main

import "fmt"

func main() {
/* 定义局部变量 */
var a int = 100
var b int = 200

fmt.Printf("交换前 a 的值为 : %d\n", a )
fmt.Printf("交换前 b 的值为 : %d\n", b )

/* 通过调用函数来交换值 */
swap(a, b)

fmt.Printf("交换后 a 的值 : %d\n", a )
fmt.Printf("交换后 b 的值 : %d\n", b )

}
```
以下代码执行结果为：  
  
交换前 a 的值为 : 100   
交换前 b 的值为 : 200  
  
交换后 a 的值 : 100  
交换后 b 的值 : 200

## 指针传递

Go语言实际只有值传递，引用传递实际是指针传递
我们都知道，变量是一种使用方便的占位符，用于引用计算机内存地址。
Go 语言的取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址。
```Go
func main () {
    a := 10

    fmt.Println("变量的地址为:", &a)
}

//变量的地址为:  0xc00000e098
```

指针：
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221210017.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221211217.png)

Go语言使用`*变量类型`定义指针,Go语言的指针不能运算


```Go
package main

import "fmt"  

func main() {
/* 定义局部变量 */
var a int = 100
var b int= 200

fmt.Printf("交换前，a 的值 : %d\n", a )
fmt.Printf("交换前，b 的值 : %d\n", b )


/* 调用 swap() 函数
* &a 指向 a 指针，a 变量的地址
* &b 指向 b 指针，b 变量的地址
*/
swap(&a, &b)

fmt.Printf("交换后，a 的值 : %d\n", a )
fmt.Printf("交换后，b 的值 : %d\n", b )
}

 

func swap(x *int, y *int) {
var temp int
temp = *x /* 保存 x 地址上的值 */
*x = *y /* 将 y 值赋给 x */
*y = temp /* 将 temp 值赋给 y */
}
```
以上代码执行结果为：  
  
交换前，a 的值 : 100    
交换前，b 的值 : 200  
  
交换后，a 的值 : 200  
交换后，b 的值 : 100