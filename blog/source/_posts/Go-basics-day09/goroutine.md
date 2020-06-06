---
title: goroutine
date: 2020-06-05 10:35:02
tags: go, goroutine, channel
categories: Go语言基础
keywords: goroutine
description: 关于go 语言并发的学习和理解
top_img: https://cdn.jsdelivr.net/gh/ThreerEyed/CDN/img/2015_05_07_01.jpg
comments: true
cover: https://cdn.jsdelivr.net/gh/ThreerEyed/CDN/img/2015_05_07_01.jpg
toc:
toc_number: true
copyright: true
mathjax:
katex:
---



{% note warning%}
这是一个创建于 2020-06-05 10:35:02 的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}



---



## 并发、并行、同步、异步



几乎只要一提到高并发，这几个词几乎都会一起出现，面试也会经常问关于它们的区别，虽然自己都能描述一些，但是不够详尽，这里花时间对这几个概念进行充分理解。



**同步**：任务依次执行，下一个任务必须等待上一个任务出了结果之后再继续执行，容易阻塞。

**异步**：任务的执行没有顺序，在较短时间内，启动运行多个任务，不关系任务结果，任务启动之后，不用等待该任务的结果返回，可以继续执行另外一个任务，然后通过其他方式得知任务执行的结果

**并发**：在某个时间段里，有多个任务请求，然后我们交替处理这些任务，并发往往就是对资源的一个合理调度。古老的计算器处理器是单核的，当有多个任务请求时，处理器会处理这些任务，但是不是同时处理的，每个任务有自己的优先处理管理，当该任务需要优先处理，那么处理器就腾出资源来处理这个任务，然后等到下一个任务优先处理的时候就把上一个任务挂起，处理下一个优先的任务，如此往复。

**并行**：在一个时间点，有多个任务在运行，就说这些任务在并行执行，往往是说有有多个资源在同时执行相同或者不相同的任务，例如cpu的多核，8个核心处理器，在某一个时间点， 都在执行不同的任务。



并发在微观上（具体的某个时刻）不是同步执行的，只是cpu根据任务的优先情况交替处理这些任务，但是在宏观上（某个时间段）看起来是同时执行的。

并行无论在微观还是宏观都是同时执行的。

总结下同步、异步、并发、并行的特点：

|      | 特点                                                         |
| ---- | ------------------------------------------------------------ |
| 同步 | 顺序执行，等待执行结果                                       |
| 异步 | 没有固定的执行顺序，不用一直等待某个任务的执行结果，通过其他方式得到结果 |
| 并发 | 并发性是对有限物理资源强制行使多用户共享以提高效率（单核交替处理任务，假并行） |
| 并行 | 同时执行不相干的任务，真正意义的同时执行                     |

  

---



## goroutine

Go语言的并发通过`goroutine`实现。`goroutine`类似于线程，属于用户态的线程，我们可以根据需要创建成千上万个`goroutine`并发工作。`goroutine`是由Go语言的运行时（runtime）调度完成，而线程是由操作系统调度完成。

Go语言还提供`channel`在多个`goroutine`间进行通信。`goroutine`和`channel`是 Go 语言秉承的 CSP（Communicating Sequential Process）并发模式的重要实现基础。



### 内核态线程和用户态线程

所谓用户态线程就是把内核态的线程在用户态实现了一遍而已，目的是更轻量化（更少的内存占用、更少的隔离、更快的调度）和更高的可控性（可以自己控制调度器）。用户态所有东西内核态都「看得见」，只是对于内核而言「用户态线程」只是一堆内存数据而已。



### go语言的运行时

尽管 Go 编译器产生的是本地可执行代码，这些代码仍旧运行在 Go 的 runtime（这部分的代码可以在 runtime 包中找到）当中。这个 runtime 类似 Java 和 .NET 语言所用到的虚拟机，它负责管理包括内存分配、垃圾回收（第 10.8 节）、栈处理、goroutine、channel、切片（slice）、map 和反射（reflection）等等。

runtime 主要由 C 语言编写（Go 1.5 开始自举），并且是每个 Go 包的最顶级包。你可以在目录 [`$GOROOT/src/runtime`](https://github.com/golang/go/tree/master/src/runtime) 中找到相关内容。

**垃圾回收器** Go 拥有简单却高效的标记-清除回收器。它的主要思想来源于 IBM 的可复用垃圾回收器，旨在打造一个高效、低延迟的并发回收器。目前 gccgo 还没有回收器，同时适用 gc 和 gccgo 的新回收器正在研发中。使用一门具有垃圾回收功能的编程语言不代表你可以避免内存分配所带来的问题，分配和回收内容都是消耗 CPU 资源的一种行为。

Go 的可执行文件都比相对应的源代码文件要大很多，这恰恰说明了 Go 的 runtime 嵌入到了每一个可执行文件当中。当然，在部署到数量巨大的集群时，较大的文件体积也是比较头疼的问题。但总的来说，Go 的部署工作还是要比 Java 和 Python 轻松得多。因为 Go 不需要依赖任何其它文件，它只需要一个单独的静态文件，这样你也不会像使用其它语言一样在各种不同版本的依赖文件之间混淆。



关于go语言的运行时 转自于 https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/02.7.md



---



## 关于goroutine

Go语言中的`goroutine`就是这样一种机制，`goroutine`的概念类似于线程，但 `goroutine`是由Go的运行时（runtime）调度和管理的。Go程序会智能地将 goroutine 中的任务合理地分配给每个CPU。Go语言之所以被称为现代化的编程语言，就是因为它在语言层面已经内置了调度和上下文切换的机制。

在Go语言编程中你不需要去自己写进程、线程、协程，你的技能包里只有一个技能–`goroutine`，当你需要让某个任务并发执行的时候，你只需要把这个任务包装成一个函数，开启一个`goroutine`去执行这个函数就可以了，就是这么简单粗暴。



### 使用goroutine



在go程序中，一般有一个主goroutine，就是我们的程序入口main 函数所在的，其他的goroutine 是由主goroutine 分化出来的。

以下实例为启动一个goroutine 

```go
package main

import "fmt"

func hello() {
    fmt.Printfn("你好啊。")
}

func main(){
    go hello()
    fmt.Printfn("main goroutine down")
}
```

以上程序返回的结果:

> main goroutine down

go语言并发使用goroutine 来完成非常简单，我们只需要在函数前加一个  `go` 关键字，就可以启动一个goroutine ，但是这里我们发现一个问题，我们只有一行打印，这个打印是main函数中的打印，hello 函数中的打印则没有，这是因为main函数也是一个goroutine 而且是主要的g，当main函数执行完返回后，主gouroutine 也就结束了

创建goroutine 也是需要时间的



`main`函数所在的`goroutine`就像是权利的游戏中的夜王，其他的`goroutine`都是异鬼，夜王一死它转化的那些异鬼也就全部GG了。



这里要想实现我们的并发操作，那么则需要让主g 等待hello 函数这一个g，最简单的实现方式就是使用`time.sleep()`



```go
package main

import (
	"fmt"
	"time"
)

func hello() {
	fmt.Println("你好啊。")
}

func main() {
	go hello()
	time.Sleep(1)  // 等待hello 函数所在的goroutine 执行完
	fmt.Println("main goroutine down")

}
```

### goroutine 结束时间

一个goroutine 对应一个函数，当函数返回的时候，这个函数所在的goroutine 也就结束了







{% note info %}

在很多时候，使用sleep 都不是一个很好的操作，等待时间过于僵化，不够灵活和精明

{% endnote %}



### 使用优雅的方式等待goroutine 结束（sync.WaitGroup）

用法：

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup // 使用sync 下的包waitgroup 并声明一个变量

func hello() {
	defer wg.Done()   // 在函数执行将要返回之时，执行。类似于计数器减一的操作
	fmt.Println("你好啊。")
}

func main() {

	wg.Add(1)  // 启动一个goroutine 计数器加一
	go hello()
	wg.Wait()  // 等待goroutine 完成

	fmt.Println("main goroutine down")

}

```



### 启动多个goroutine

在Go语言中实现并发就是这样简单，我们还可以启动多个`goroutine`。让我们再来一个例子： （这里使用了`sync.WaitGroup`来实现goroutine的同步）

```go
var wg sync.WaitGroup

func hello(i int) {
	defer wg.Done() // goroutine结束就登记-1
	fmt.Println("Hello Goroutine!", i)
}
func main() {

	for i := 0; i < 10; i++ {
		wg.Add(1) // 启动一个goroutine就登记+1
		go hello(i)
	}
	wg.Wait() // 等待所有登记的goroutine都结束
}
```

多次执行上面的代码，会发现每次打印的数字的顺序都不一致。这是因为10个`goroutine`是并发执行的，而`goroutine`的调度是随机的。



### goroutine 与线程

#### 可增长的栈

OS线程（操作系统线程）一般都有固定的栈内存（通常为2MB）,一个`goroutine`的栈在其生命周期开始时只有很小的栈（典型情况下2KB），`goroutine`的栈不是固定的，他可以按需增大和缩小，`goroutine`的栈大小限制可以达到1GB，虽然极少会用到这么大。所以在Go语言中一次创建十万左右的`goroutine`也是可以的。



### goroutine 的调度

**gotoutine  是通过GPM（Goroutine、Processor、Machine） 这一套调度系统来调度的**

#### GPM：

G、P、M 是 Go 调度器的三个核心组件，各司其职。在它们精密地配合下，Go 调度器得以高效运转，这也是 Go 天然支持高并发的内在动力。

先看 G，取 goroutine 的首字母，主要保存 goroutine 的一些状态信息以及 CPU 的一些寄存器的值，例如 IP 寄存器，以便在轮到本 goroutine 执行时，CPU 知道要从哪一条指令处开始执行。

当 goroutine 被调离 CPU 时，调度器负责把 CPU 寄存器的值保存在 g 对象的成员变量之中。

当 goroutine 被调度起来运行时，调度器又负责把 g 对象的成员变量所保存的寄存器值恢复到 CPU 的寄存器。

上面这段描述来自公众号“go语言核心编程技术”的调度器系列文章。



`GPM`是Go语言运行时（runtime）层面的实现，是go语言自己实现的一套调度系统。区别于操作系统调度OS线程。

- `G`很好理解，就是个goroutine的，里面除了存放本goroutine信息外 还有与所在P的绑定等信息。
- `P`管理着一组goroutine队列，P里面会存储当前goroutine运行的上下文环境（函数指针，堆栈地址及地址边界），P会对自己管理的goroutine队列做一些调度（比如把占用CPU时间较长的goroutine暂停、运行后续的goroutine等等）当自己的队列消费完了就去全局队列里取，如果全局队列里也消费完了会去其他P的队列里抢任务。
- `M（machine）`是Go运行时（runtime）对操作系统内核线程的虚拟， M与内核线程一般是一一映射的关系， 一个groutine最终是要放到M上执行的；

P与M一般也是一一对应的。他们关系是： P管理着一组G挂载在M上运行。当一个G长久阻塞在一个M上时，runtime会新建一个M，阻塞G所在的P会把其他的G 挂载在新建的M上。当旧的G阻塞完成或者认为其已经死掉时 回收旧的M。

P的个数是通过`runtime.GOMAXPROCS`设定（最大256），Go1.5版本之后默认为物理线程数。 在并发量大的时候会增加一些P和M，但不会太多，切换太频繁的话得不偿失。

单从线程调度讲，Go语言相比起其他语言的优势在于OS线程是由OS内核来调度的，`goroutine`则是由Go运行时（runtime）自己的调度器调度的，这个调度器使用一个称为m:n调度的技术（复用/调度m个goroutine到n个OS线程）。 其一大特点是goroutine的调度是在用户态下完成的， 不涉及内核态与用户态之间的频繁切换，包括内存的分配与释放，都是在用户态维护着一块大的内存池， 不直接调用系统的malloc函数（除非内存池需要改变），成本比调度OS线程低很多。 另一方面充分利用了多核的硬件资源，近似的把若干goroutine均分在物理线程上， 再加上本身goroutine的超轻量，以上种种保证了go调度方面的性能。

[点我了解更多](https://www.cnblogs.com/sunsky303/p/9705727.html)



### GOMAXPROCS

Go运行时的调度器使用`GOMAXPROCS`参数来确定需要使用多少个OS线程来同时执行Go代码。默认值是机器上的CPU核心数。例如在一个8核心的机器上，调度器会把Go代码同时调度到8个OS线程上（GOMAXPROCS是m:n调度中的n）。

Go语言中可以通过`runtime.GOMAXPROCS()`函数设置当前程序并发时占用的CPU逻辑核心数。

Go1.5版本之前，默认使用的是单核心执行。Go1.5版本之后，默认使用全部的CPU逻辑核心数。

我们可以通过将任务分配到不同的CPU逻辑核心上实现并行的效果，这里举个例子：

```go
func a() {
	for i := 1; i < 10; i++ {
		fmt.Println("A:", i)
	}
}

func b() {
	for i := 1; i < 10; i++ {
		fmt.Println("B:", i)
	}
}

func main() {
	runtime.GOMAXPROCS(1)
	go a()
	go b()
	time.Sleep(time.Second)
}
```

两个任务只有一个逻辑核心，此时是做完一个任务再做另一个任务。 将逻辑核心数设为2，此时两个任务并行执行，代码如下。

```go
func a() {
	for i := 1; i < 10; i++ {
		fmt.Println("A:", i)
	}
}

func b() {
	for i := 1; i < 10; i++ {
		fmt.Println("B:", i)
	}
}

func main() {
	runtime.GOMAXPROCS(2)
	go a()
	go b()
	time.Sleep(time.Second)
}
```

Go语言中的操作系统线程和goroutine的关系：

1. 一个操作系统线程对应用户态多个goroutine。
2. go程序可以同时使用多个操作系统线程。
3. goroutine和OS线程是多对多的关系，即m:n。



### channel

单纯地将函数并发执行是没有意义的。函数与函数间需要交换数据才能体现并发执行函数的意义。

虽然可以使用共享内存进行数据交换，但是共享内存在不同的`goroutine`中容易发生竞态问题。为了保证数据交换的正确性，必须使用互斥量对内存进行加锁，这种做法势必造成性能问题。

Go语言的并发模型是`CSP（Communicating Sequential Processes）`，提倡**通过通信共享内存**而不是**通过共享内存而实现通信**。

如果说`goroutine`是Go程序并发的执行体，`channel`就是它们之间的连接。`channel`是可以让一个`goroutine`发送特定值到另一个`goroutine`的通信机制。

Go 语言中的通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循先入先出（First In First Out）的规则，保证收发数据的顺序。每一个通道都是一个具体类型的导管，也就是声明channel的时候需要为其指定元素类型。



channel 是一种类型，是一种引用类型，我们可以使用make 来创建一个channel 类型

通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

```go
// 声明格式如下
var 变量 chan 元素类型

// 声明一个channel 类型(用来传递int类型)
var channel1 chan int
// 声明一个channel 类型(用来传递float类型)
var channel2 chan float
// 声明一个channel 类型(用来传递int切片类型)
var channel3 chan []int
```

使用make 函数对声明的通道进行初始化

```go
make(chan 元素类型, [缓冲大小])
```



### channel操作

通道有发送（send）、接收(receive）和关闭（close）三种操作。

发送和接收都使用`<-`符号。

现在我们先使用以下语句定义一个通道：

```go
ch := make(chan int)
```

#### 发送

将一个值发送到通道中。

```go
ch <- 10 // 把10发送到ch中
```

#### 接收

从一个通道中接收值。

```go
x := <- ch // 从ch中接收值并赋值给变量x
<-ch       // 从ch中接收值，忽略结果
```

#### 关闭

我们通过调用内置的`close`函数来关闭通道。

```go
close(ch)
```

关于关闭通道需要注意的事情是，只有在通知接收方goroutine所有的数据都发送完毕的时候才需要关闭通道。通道是可以被垃圾回收机制回收的，它和关闭文件是不一样的，在结束操作之后关闭文件是必须要做的，但关闭通道不是必须的。

关闭后的通道有以下特点：

1. 对一个关闭的通道再发送值就会导致panic。
2. 对一个关闭的通道进行接收会一直获取值直到通道为空。
3. 对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值。
4. 关闭一个已经关闭的通道会导致panic。



### 无缓冲的通道

无缓冲的通道又称为阻塞的通道。我们来看一下下面的代码：

```go
func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}
```

上面这段代码能够通过编译，但是执行的时候会出现以下错误：

```bash
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.main()
        .../src/github.com/Q1mi/studygo/day06/channel02/main.go:8 +0x54
```

为什么会出现`deadlock`错误呢？

因为我们使用`ch := make(chan int)`创建的是无缓冲的通道，无缓冲的通道只有在有人接收值的时候才能发送值。就像你住的小区没有快递柜和代收点，快递员给你打电话必须要把这个物品送到你的手中，简单来说就是无缓冲的通道必须有接收才能发送。

上面的代码会阻塞在`ch <- 10`这一行代码形成死锁，那如何解决这个问题呢？

一种方法是启用一个`goroutine`去接收值，例如：

```go
func recv(c chan int) {
	ret := <-c
	fmt.Println("接收成功", ret)
}
func main() {
	ch := make(chan int)
	go recv(ch) // 启用goroutine从通道接收值
	ch <- 10
	fmt.Println("发送成功")
}
```

无缓冲通道上的发送操作会阻塞，直到另一个`goroutine`在该通道上执行接收操作，这时值才能发送成功，两个`goroutine`将继续执行。相反，如果接收操作先执行，接收方的goroutine将阻塞，直到另一个`goroutine`在该通道上发送一个值。

使用无缓冲通道进行通信将导致发送和接收的`goroutine`同步化。因此，无缓冲通道也被称为`同步通道`。

### 有缓冲的通道

解决上面问题的方法还有一种就是使用有缓冲区的通道。我们可以在使用make函数初始化通道的时候为其指定通道的容量，例如：

```go
func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}
```

只要通道的容量大于零，那么该通道就是有缓冲的通道，通道的容量表示通道中能存放元素的数量。就像你小区的快递柜只有那么个多格子，格子满了就装不下了，就阻塞了，等到别人取走一个快递员就能往里面放一个。

我们可以使用内置的`len`函数获取通道内元素的数量，使用`cap`函数获取通道的容量，虽然我们很少会这么做。



### 单向通道

有的时候我们会将通道作为参数在多个任务函数间传递，很多时候我们在不同的任务函数中使用通道都会对其进行限制，比如限制通道在函数中只能发送或只能接收。

Go语言中提供了**单向通道**来处理这种情况。例如，我们把上面的例子改造如下：

```go
func counter(out chan<- int) {
	for i := 0; i < 100; i++ {
		out <- i
	}
	close(out)
}

func squarer(out chan<- int, in <-chan int) {
	for i := range in {
		out <- i * i
	}
	close(out)
}
func printer(in <-chan int) {
	for i := range in {
		fmt.Println(i)
	}
}

func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)
	go counter(ch1)
	go squarer(ch2, ch1)
	printer(ch2)
}
```

其中，

- `chan<- int`是一个只写单向通道（只能对其写入int类型值），可以对其执行发送操作但是不能执行接收操作；
- `<-chan int`是一个只读单向通道（只能从其读取int类型值），可以对其执行接收操作但是不能执行发送操作。

在函数传参及任何赋值操作中可以将双向通道转换为单向通道，但反过来是不可以的。