---
title: Golang基础语法——内建变量
date: 2023-02-21 14:55:03
categories:
- Golang
---
# Go语言的内建变量类型
- bool，string
- (u)int, (u)int8 ,(u)int16 ,(u)int32 ,(u)int64 ,uintptr(指针)
- byte （8位）, rune（char）
- float32，float64，complex64，complex128
**rune是Go语言的字符型，用来代替java中的char类型，为32位，来替代char对多语言的字节不兼容**
**complex是复数类型，为实部+虚部，complex64的意思为实部和虚部为32位，complex128为实部和虚部为64位**
# 复数回顾
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221150219.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221150240.png)

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221150253.png)

```Go
func euler() {
	fmt.Println(
//	cmplx.Pow(math.E, 1i * math.Pi) + 1)
	cmplx.Exp(1i * math.Pi) + 1 )
}
```
由于浮点数在计算机中的标准，所以尤拉公式在计算机中得到的数接近0，但得不到0

# 类型转换
Go语言没有隐式类型转换，只有强制类型转换
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221151018.png)
