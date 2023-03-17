---
title: Go语言的并发编程
date: 2023-03-15 19:09:58
tags:
- Golang
categories:
- Golang
---
# goroutine
## 什么是goroutine
goroutine是Go语言并行设计的核心，有人称之为go程。 Goroutine从量级上看很像协程，它比线程更小，十几个goroutine可能体现在底层就是五六个线程，Go语言内部帮你实现了这些goroutine之间的内存共享。执行goroutine只需极少的栈内存(大概是4~5KB)，当然会根据相应的数据伸缩。也正因为如此，可同时运行成千上万个并发任务。goroutine比thread更易用、更高效、更轻便。
  一般情况下，一个普通计算机跑几十个线程就有点负载过大了，但是同样的机器却可以轻松地让成百上千个goroutine进行资源竞争。

```Go
func main() {  
   //var a [10]int  
   for i := 0; i < 1000; i++ {  
      go func(i int) {  
         for {  
            fmt.Println("Hello World ", i)  
            //a[i]++  
            //runtime.Gosched()         }  
      }(i)  
   }  
   time.Sleep(time.Minute)  
   //fmt.Println(a)  
}
```
## 为什么要有goroutine

当前的大多数的并发调度，是切换cpu的线程来保证多个线程同时运行，但这样会让CPU有较高的消耗，降低了CPU的执行效率
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230316191934.png)
所以我们将线程的职能一份为二，在用户空间内对协程做切换，而由于CPU的视野限制，所以它依然认为这是一个线程
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230316192102.png)

所以我们通过协程调度器，实现单个线程对多个协程的模式，通过切换协程实现并发，从而保证cpu不切换线程，减少CPU由于切换线程造成的性能损失
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230316192249.png)


# channel
## 定义
channel是Go语言中的一个核心类型，可以把它看成管道。并发核心单元通过它就可以发送或者接收数据进行通讯，这在一定程度上又进一步降低了编程的难度。

channel是一个==数据类型==，主要用来解决go程的同步问题以及go程之间数据共享（数据传递）的问题。因为它是一个数据类型，所以可以作为函数的==参数==和==返回值==

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230316191435.png)


channel在代码中的定义如下
```Go
func defineChannel() {  
   //初始化一个channel  
   c := make(chan int)  
   go func() {  
      defer fmt.Println("goroutine end")  
      fmt.Println("goroutine running")  
      //将数据发送到channel中  
      c <- 666  
   }()  
   //从channel接收数据  
   num := <-c  
   fmt.Println("num :", num)  
   fmt.Println("main goroutine end")  
}
```
可以将channel保存在数组中
```Go
func worker(i int, c chan int) {  
   //开启循环，持续从channel接收数据  
   for {  
      fmt.Printf("Worker %d received %c \n", i, <-c)  
   }  
}  
  
func chanDemo() {  
   //创建一个长度为10的数组，数组内存放的是Channel,但是channel没有经过初始化，暂时无法使用  
   var channles [10]chan int  
   for i := 0; i < 10; i++ {  
      //将channel初始化  
      channles[i] = make(chan int)  
      //开启接收函数  
      go worker(i, channles[i])  
   }  
   //发送数据
   for i := 0; i < 10; i++ {  
      channles[i] <- 'a' + i  
   }  
   //发送数据
   for i := 0; i < 10; i++ {  
      channles[i] <- 'A' + i  
   }  
  
   //c <- 1  
   //c <- 2   time.Sleep(time.Millisecond)  
}
```

而由于channel可以作为参数和返回值,所以在正常生产中我们一般将channel定义在函数中，而是定义在主函数中
```Go
func worker(i int) chan int {  
   c := make(chan int)  
   //开启goroutine，防止接收死循环
   go func() {  
      //开启循环，持续从channel接收数据  
      for {  
         fmt.Printf("Worker %d received %c \n", i, <-c)  
      }  
   }()  
   return c  
}  
  
func chanDemo() {  
   //创建一个长度为10的数组，数组内存放的是Channel,但是channel没有经过初始化，暂时无法使用  
   var channles [10]chan int  
   for i := 0; i < 10; i++ {  
      //开启接收函数，并获取初始化的channel  
      channles[i] = worker(i)  
   }  
   for i := 0; i < 10; i++ {  
      channles[i] <- 'a' + i  
   }  
   for i := 0; i < 10; i++ {  
      channles[i] <- 'A' + i  
   }  
  
   //c <- 1  
   //c <- 2   time.Sleep(time.Millisecond)  
}
```
channel可以通过箭头方向进行修饰

`chan<- int`表示只能发送数据到channel中

`<-chan int`表示只能从channel中获取数据

## 缓冲区
通过channel发送的数据，必须有人能够接收，如果没有接收者，就需要有一个==缓冲区==
在定义channel时，可以通过设置channel的参数，给参数设定缓冲区的大小
```Go
func main() {  
   //定义一个channel  
   //defineChannel()   
   //chanDemo()   
   bufferedChannel()  
}  
  
func bufferedChannel() {  
   c := make(chan int, 3)  
   c <- 'a'  
   c <- 'b'  
   c <- 'c'  
   //c<- 'd'  
}
```
上述中创建了一个缓冲区大小为3的channel，且运行不会报错，因为前三个存入的数据都被放进了缓冲区中，如果放入第四个参数而没有人接收，就会报错

## 手动关闭channel
关闭channel永远是发送方关闭，由发送方说明自己已经发送完信息了

```Go
func channelClose() {  
   c := make(chan int, 3)  
   go work(0, c)  
   c <- 'a'  
   c <- 'b'  
   c <- 'c'  
   c <- 'd'  
   close(c)  
   time.Sleep(time.Microsecond * 5)  
}
```
由于关闭了channel，接收方如果使用while循环接收的话就会接收不到channel

所以接收方可以判断是否有数据
```Go
func work(i int, c chan int) {  
   for {  
      result, ok := <-c  
      if ok {  
         fmt.Printf("Worker %d received %c \n", i, result)  
      } else {  
         break  
      }  
   }  
}
```

也可直接使用range遍历channel
```Go
func work(i int, c chan int) {  
   for n := range c {  
      fmt.Printf("Worker %d received %c \n", i, n)  
   }  
}
```

![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230316191514.png)

## 等待channel

我们发现上述的代码都通过time.sleep等待一段时间，等channel 内的数据传输完后再结束，但是这样十分粗糙
我们可以根据上图中的“通过通信来共享内存”反过来告诉发送者，接收完毕了
```GO
func main() {  
   chanDemo()  
}  
  
func doWork(i int, c chan int, done chan bool) {  
   for n := range c {  
      fmt.Printf("Worker %d received %c \n", i, n)  
      //开启goroutine异步发送完成的消息  
      //注意这里必须开启异步，因为接收到a的信息后，返回了消息，但是下方代码没有立即接收，而是
      //又发送了A，而没有接收方，就会造成channel的死锁
	  
	  //当然也可以让下方的代码发送完后立即接收，这里就不必要异步执行了
      go func() { done <- true }()  
  
   }  
}  
  
type worker struct {  
   c    chan int  
   done chan bool  
}  
  
func createWorker(i int) worker {  
   w := worker{  
      c:    make(chan int),  
      done: make(chan bool),  
   }  
   go doWork(i, w.c, w.done)  
   return w  
}  
  
func chanDemo() {  
   //创建一个长度为10的数组，数组内存放的是Channel,但是channel没有经过初始化，暂时无法使用  
   var workers [10]worker  
   for i := 0; i < 10; i++ {  
      //开启接收函数，并获取初始化的channel  
      workers[i] = createWorker(i)  
   }  
   for i, worker := range workers {  
      worker.c <- 'a' + i  
   }  
   for i, worker := range workers {  
      worker.c <- 'A' + i  
   }  
   //wait for all of them  
   for _, worker := range workers {  
      //接收来自信息接收端的信息，由于接收后方法才会结束，也就能够等待接收方法执行完成  
      <-worker.done  
      //因为发送消息发送了两次，所以也要接收两次  
      <-worker.done  
   }  
   //time.Sleep(time.Millisecond)  
}
```

Go语言中有能够判断channel是否完成的功能
使用`sync.WaitGroup`，里面有三个方法可以分别等待、结束，添加任务
```Go
func main() {  
   chanDemo()  
}  
  
func doWork(i int, c chan int, wg *sync.WaitGroup) {  
   for n := range c {  
      fmt.Printf("Worker %d received %c \n", i, n)  
      wg.Done()  
   }  
  
}  
  
type worker struct {  
   c  chan int  
   wg *sync.WaitGroup  
}  
  
func createWorker(i int, wg *sync.WaitGroup) worker {  
   w := worker{  
      c:  make(chan int),  
      wg: wg,  
   }  
   go doWork(i, w.c, w.wg)  
   return w  
}  
  
func chanDemo() {  
   wg := sync.WaitGroup{}  
  
     
   var workers [10]worker  
   for i := 0; i < 10; i++ {  
      //开启接收函数，并获取初始化的channel  
      workers[i] = createWorker(i, &wg)  
   }  
   for i, worker := range workers {  
      worker.c <- 'a' + i  
      wg.Add(1)  
   }  
   for i, worker := range workers {  
      worker.c <- 'A' + i  
      wg.Add(1)  
   }  
   //wait for all of them  
   wg.Wait()  
   //time.Sleep(time.Millisecond)  
}
```


## Select
select 是 Go 中的一个控制结构，类似于用于通信的 switch 语句。每个 case 必须是一个通信操作，要么是发送要么是接收。  
select 随机执行一个可运行的 case。如果没有 case 可运行，它将阻塞，直到有 case 可运行。一个默认的子句应该总是可运行的。

-   每个 case 都必须是一个通信
-   所有 channel 表达式都会被求值
-   所有被发送的表达式都会被求值
-   如果任意某个通信可以进行，它就执行，其他被忽略。  
    如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行。  
    否则：  
    如果有 default 子句，则执行该语句。  
    如果没有 default 子句，select 将阻塞，直到某个通信可以运行；Go 不会重新对 channel 或值进行求值。

在Select中可以使用Nil Channel，当数据还没准备好的时候可以把Channel置为nil，这样case就不会执行

select+default可以实现类似于非阻塞式的获取，如下示例输出结果为“no value”
```Go
func main() {
	c1 := make(chan int)
	select {
	case n := <-c1:
		fmt.Println(n)
	default:
		fmt.Println("no value")
	}
}

```

```Go
package main  
  
import (  
   "fmt"  
   "math/rand"   "time")  
  
func generateWorker(i int) chan int {  
   c := make(chan int) //开启一个channel  
   go worker(i, c)     //异步开启消耗channel内的数据  
   return c            //返回channel  
}  
  
func worker(i int, c chan int) {  
   for n := range c {  
      time.Sleep(time.Second) //每秒钟消耗一个  
      fmt.Printf("recevied from %d value : %d\n", i, n)  
   }  
}  
  
func generator() chan int {  
   out := make(chan int)  
   //在goroutine中发送数据  
   go func() {  
      i := 0  
      for true {  
         time.Sleep(time.Duration(rand.Intn(1500)) * time.Millisecond)  
         out <- i  
         i++  
      }  
   }()  
   return out  
}  
  
func main() {  
   var c1, c2 = generator(), generator() //获取两个channel  
   var worker = generateWorker(0)        //获取一个channel  
   //10s后通知给after  
   after := time.After(time.Second * 10)  
   //通过数组存储获得的value  
   var values []int  
   tick := time.Tick(time.Second)  
   for true {  
      var activeWorker = make(chan int)  
      var activeValue int  
      if len(values) > 0 { //如果队列里有数据，就把worker给空channel,把队列的第一个数给worker消耗  
         activeWorker = worker  
         activeValue = values[0]  
      }  
      select {  
      //从c1,c2中获取数据后，将数据添加进队列  
      case n := <-c1:  
         values = append(values, n)  
      case n := <-c2:  
         values = append(values, n)  
      case activeWorker <- activeValue: //消耗队列的第一个数据  
         values = values[1:]  
      case <-time.After(time.Millisecond * 800):  
         fmt.Println("timeOut")  
      case <-tick: //定时获取队列的长度  
         fmt.Println("queue len = ", len(values))  
      case <-after:  
         fmt.Println("bye")  
         return  
      }  
   }  
  
}
```

# 传统的同步机制

-   WaitGroup
-   Mutex
-   Cond

传统同步机制（共享内存实现通信）较少使用，一般使用channel进行通信（通信实现共享内存）
```Go
  
type atomicInt struct {  
   value int  
   lock  sync.Mutex  
}  
  
func (a *atomicInt) increment() {  
   fmt.Println("safe increment")  
  
   func() {  
      a.lock.Lock()  
      defer a.lock.Unlock()  
      a.value++  
   }()  
}  
func (a *atomicInt) get() int {  
   a.lock.Lock()  
   defer a.lock.Unlock()  
   return int(a.value)  
}  
func main() {  
   var a atomicInt  
   a.increment()  
   go func() {  
      a.increment()  
   }()  
   time.Sleep(time.Millisecond)  
   fmt.Println(a.get())  
}
```