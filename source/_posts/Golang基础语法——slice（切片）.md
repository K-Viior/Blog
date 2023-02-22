---
title: Golang基础语法——slice（切片）
date: 2023-02-22 13:55:39
categories:
- Golang
---
# Golang基础语法——slice（切片）
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222140213.png)
上图中的s即为一个切片

```Go
package main

import "fmt"

func main() {

    arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7}

    fmt.Println("arr[2:6] = ", arr[2:6])
    fmt.Println("arr[2:] = ", arr[2:])
    fmt.Println("arr[:6] = ", arr[:6])
    fmt.Println("arr[:] = ", arr[:])

}
```
结果为：
arr[2:6] =  [2 3 4 5]
arr[2:] =  [2 3 4 5 6 7]
arr[:6] =  [0 1 2 3 4 5]
arr[:] =  [0 1 2 3 4 5 6 7]

Go 语言切片是对数组的抽象。
Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

你可以声明一个未指定大小的数组来定义切片：
```Go
var identifier []type
```
切片不需要说明长度。

或使用make()函数来创建切片:
```Go
var slice1 []type = make([]type, len)

也可以简写为

slice1 := make([]type, len)
```
也可以指定容量，其中capacity为可选参数。
```GO
make([]T, length, capacity)
```
这里 len 是数组的长度并且也是切片的初始长度。

初始化切片s,是数组arr的引用
```Go
s := arr[:]
```

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222141927.png)



![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222141523.png)

Go语言可以定义切片的切片

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222142029.png)
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222142034.png)

他们使用的数据都是同一个arr，所以他们所有切片的修改都可以体现在arr上

# slice的扩展
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222141845.png)

```GO
    arr := [...]int{0, 1, 2, 3, 4, 5, 6, 7}

    fmt.Printf("arr: %v\n", arr)

    s1 := arr[2:6]
    s2 := s1[3:5]

    fmt.Printf("s1: %v,len(s1): %d, cap(s1): %d\n", s1,len(s1),cap(s1))
    fmt.Printf("s2: %v,len(s2): %d, cap(s2): %d\n", s2,len(s2),cap(s2))
```

打印结果为：
arr: [0 1 2 3 4 5 6 7]
s1: [2 3 4 5],len(s1): 4, cap(s1): 6
s2: [5 6],len(s2): 2, cap(s2): 3

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222142627.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222142753.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222142823.png)

# slice的添加

```Go
    s3 := append(s2, 10)

    s4 := append(s3, 11)

    s5 := append(s4, 12)

    fmt.Println("s3,s4,s5 =",s3,s4,s5)
    fmt.Printf("arr: %v\n", arr)
```
打印结果为： s3,s4,s5 = [5 6 10]  [5 6 10 11]  [5 6 10 11 12]

而arr的结果为arr: [0 1 2 3 4 5 6 10]

这表明s3，和s4 所view的不再是之前的arr，而是go语言在内部分配了一个长度更长的arr
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222144757.png)

# 空（nil）切片
一个切片在未初始化之前默认为 nil，长度为 0，实例如下：
```Go
package main

import "fmt"

func main() {
	var numbers []int

	printSlice(numbers)

	if(numbers == nil){
	fmt.Printf("切片是空的")
}
}


func printSlice(x []int){

	fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)

}
```
以上实例运行输出结果为:
```Go
len=0 cap=0 slice=[]

切片是空的
```

# 切片截取
可以通过设置下限及上限来设置截取切片[lower-bound:upper-bound]，实例如下：
```Go
package main

import "fmt"

func main() {
/* 创建切片 */

	numbers := []int{0,1,2,3,4,5,6,7,8}

	printSlice(numbers)
	/* 打印原始切片 */
	fmt.Println("numbers ==", numbers)
	/* 打印子切片从索引1(包含) 到索引4(不包含)*/
	fmt.Println("numbers[1:4] ==", numbers[1:4])
	/* 默认下限为 0*/
	fmt.Println("numbers[:3] ==", numbers[:3])
	/* 默认上限为 len(s)*/
	fmt.Println("numbers[4:] ==", numbers[4:])
	numbers1 := make([]int,0,5)

	printSlice(numbers1)

	/* 打印子切片从索引 0(包含) 到索引 2(不包含) */

	number2 := numbers[:2]
	printSlice(number2)

	/* 打印子切片从索引 2(包含) 到索引 5(不包含) */
	number3 := numbers[2:5]
	printSlice(number3)
}
  
func printSlice(x []int){

	fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)

}
```
执行以上代码输出结果为：
len=9 cap=9 slice=[0 1 2 3 4 5 6 7 8]
numbers == [0 1 2 3 4 5 6 7 8]
numbers[1:4] == [1 2 3]
numbers[:3] == [0 1 2]
numbers[4:] == [4 5 6 7 8]
len=0 cap=5 slice=[]
len=2 cap=9 slice=[0 1]
len=3 cap=7 slice=[2 3 4]

# append() 和 copy() 函数
如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。
```Go
package main

import "fmt"

func main() {
	var numbers []int
	printSlice(numbers)
	
	/* 允许追加空切片 */
	numbers = append(numbers, 0)
	printSlice(numbers)
	
	/* 向切片添加一个元素 */
	numbers = append(numbers, 1)
	printSlice(numbers)
	
	
	/* 同时添加多个元素 */
	numbers = append(numbers, 2,3,4)
	printSlice(numbers)
	
	/* 创建切片 numbers1 是之前切片的两倍容量*/
	numbers1 := make([]int, len(numbers), (cap(numbers))*2)
	
	/* 拷贝 numbers 的内容到 numbers1 */
	copy(numbers1,numbers)
	printSlice(numbers1)
}

func printSlice(x []int){
fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
以上代码执行输出结果为：
len=0 cap=0 slice=[]
len=1 cap=1 slice=[0]
len=2 cap=2 slice=[0 1]
len=5 cap=6 slice=[0 1 2 3 4]
len=5 cap=12 slice=[0 1 2 3 4]