---
title: Golang基础语法——条件语句和循环
date: 2023-02-21 15:41:40
categories:
- Golang
---
# Golang基础语法——条件语句和循环
## 条件语句
### if
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221154222.png)


if 的条件里不需要括号，且变量的定义也是名称在前类型在后

```Go
func main() {
	const filename = "123.txt"
	if consts,err := ioutil.ReadFile(filename); err != nil {
		fmt.Println(err)
	}else {
		fmt.Println("%s\n",consts)
	}

//if的条件中可以定义变量，再对变量进行判断，中间使用分号 ; 隔开
}
```

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221154804.png)

### switch
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221154821.png)

switch会自动break，除非使用fallthrough

```Go
func grade(score int) string {

    g := ""

    switch {

    case score < 0 || score > 100:

        panic(fmt.Sprintf("wrong score: %d", score))

    case score < 60:

        g = "D"

    case score < 70:

        g = "C"

    case score < 85:

        g = "B"

    case score <= 100:

        g = "A"

    }

    return g

}
```

Switch本身可以不带表达式，只依靠case的条件进行判断

`panic`可以抛出异常，并且中断程序运行

## 循环
### for
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221155341.png)



```Go
//整数转二进制
func covertToBin(n int) string {
	result := ""
	for ; n > 0 ; n /= 2 {
		lsb := n % 2
		result = strconv.Itoa(lsb) + result
	}
	return result
}

func main () {
	fmt.Println(
		covertToBin(5), // 101
		covertToBin(13), // 1101
	)

}

```


Go语言可以省略for 语句中的起始条件，递增条件，就变成了一个while
```Go
func printFile(filename string) {

	file,err := os.Open(filename)
	
	if err != nil {
		painc(err)
	}
	
	scanner := bufio.NewScanner(file)
	
	for scanner.Scan() {
		fmt.println(scanner.Text())
	}
}
```

for的三个条件都去掉就称为了死循环

```Go
func forever () {
	for {
		fmt.Println("abc")
	}
}
```


![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221160507.png)
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221160517.png)


![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221160550.png)


![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230221160531.png)
