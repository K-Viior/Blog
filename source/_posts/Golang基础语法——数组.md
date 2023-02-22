---
title: Golang基础语法——数组
date: 2023-02-22 13:23:56
categories:
- Golang
---
# Golang基础语法——数组
## 数组
Go语言定义数组
`var arr1 [5]int` 可以定义一个定长的数组，同样和其他语言相反，先[]后int

`arr2 := [3]int{1, 2, 3}` 也可以使用:=直接定义，但是需要给数组内的元素赋值，不能定义空数组

`arr3 := [...]int{1, 2, 3, 4}`和其他语言不一样，定义一个由计算机自己判断长度的数组不是使用`[]int`，[]内要使用`[...]`

打印三个数组的结果为：[0 0 0 0 0]  [1 2 3]  [1 2 3 4]
## 二维数组

使用`var arr4 [3][4]int`可以定义二维数组
可以理解为 **3个长度为4的int数组**
打印结果为：`[ [ 0 0 0 0 ] [ 0 0 0 0 ] [ 0 0 0 0 ] ]`

## 遍历数组

普通的方法
```Go
    for i := 0;i < len(arr3); i++ {

        fmt.Println(arr3[i])

    }
```

使用`range`关键字获取每个数组元素的下标
```Go
    for i := range arr3 {

        fmt.Println(arr3[i])

    }
```

同时也可以获取键和值
```Go
for i, v := range arr3 {

        fmt.Println(i,v)

    }
```
打印结果为：
0 1
1 2
2 3
3 4
分别为数组下标和数组的值

也可以只要数组的值，不要下标
```Go
for _, v := range arr3 {

        fmt.Println(v)

    }
```

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222134140.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222134226.png)

## Go语言的数组是值类型

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230222134715.png)

Go语言的数组是值类型，在调用时会在方法内部拷贝一份，操作自己内部的数组，所以类型是值引用
而其他大部分语言引用数组都是引用类型，在方法内更改数组的内容，方法外也生效

可以使用指针类引用Go语言的数组

直接修改
```Go
func printArray (arr [3]int) {

    arr[0] = 100

    for _,v := range arr{

        fmt.Println(v)

    }

}

func main () {
	arr2 := [3]int{1,2,3}
	printArray(arr2)
	fmt.Println("主函数中的arr2",arr2)
}
```
结果为：
100
2
3
主函数中的arr2 [1 2 3]

使用指针
```Go
func printArray (arr *[3]int) {

    arr[0] = 100

    for _,v := range arr{

        fmt.Println(v)

    }

}

func main () {
	arr2 := [3]int{1,2,3}
	printArray(arr2)
	fmt.Println("主函数中的arr2",&arr2)
}
```
结果为：
100
2
3
主函数中的arr2 [100 2 3]