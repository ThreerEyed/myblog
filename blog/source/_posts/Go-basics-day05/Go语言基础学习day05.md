---
title: Go语言基础学习day05（指针，map，range）
date: 2020-05-29 14:14:59
tags: go
categories: Go语言基础
keywords: Go, 基础学习, Go指针
description: 
top_img: https://i.loli.net/2020/05/23/H9yRgArpw2NM4ne.jpg
comments: true
cover: https://i.loli.net/2020/05/23/H9yRgArpw2NM4ne.jpg
toc:
toc_number: true
copyright: true
mathjax:
katex:
---



{% note warning%}
这是一个创建于 2020-05-29 14:14:59的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}

<br/>





## 指针

> **原文链接：**[Understand Go pointers in less than 800 words or your money back](https://link.jianshu.com?t=https://dave.cheney.net/2017/04/26/understand-go-pointers-in-less-than-800-words-or-your-money-back)

这是一篇面向即将学习 go 语言并且对指针理念或者 go 的指针类型不是很了解的程序员的内容

<br/>

### 什么是指针？

简单解释，指针就是指向另一段地址的值，课本上是这么解释的，但是如果你是一个来自并不讨论变量地址开发语言的开发者，使用指针进行编程将感觉非常的美妙。

<br/>

我们先换一个话题

### 什么是内存？

计算机的内存 RAM 可以把它想象成一些有序的盒子，一个接一个的排成一排，每一个盒子或者单元格都被一个唯一的数字标记依次递增，这个数字就是该单元格的地址，也就是内存的地址。



![img](https://i.loli.net/2020/05/29/MFodZcxTshmjynJ.jpg)

每一个单元格存储单一的值，如果你知道一个单元格的地址，那么你就可以通过地址读取单元格中的内容。你也可以向这个单元格中放置一个值来替代之前的任何值。

这就是关于内存的所有内容。CPU 所做的一切操作也都是在读取或者存储单元格中值。

<br/>

### 什么是变量？

编写一段程序，检索出值并存储在地址为 200 的一个块内存中，将其乘以 3，并将结果存储在地址为 201 的另一块内存中，我们就可以像这样写一段伪代码：

1. 检索出内存地址为 200 的值，并将其存储在 CPU 中

2. 将存储在 CPU 中的值乘以 3

3. 将 CPU 中存储的结果，写入地址为 201 的内存块中

   ![img](https://i.loli.net/2020/05/29/drpQl1FhRqiOmxs.jpg)

这是相当早期的编程形式，程序员需要维护一列内存地址，需要清楚的知道：谁、什么时候在使用它以及内存中存储的值是什么。

很显然这是非常枯燥又很容易出错的方式，并且还意味着你要为程序构建过程中内存中的每一个可能出现的值分配地址。更糟糕的是这种设计使得你动态的为变量分配存储变得很艰难，你可以想象一下如果编写一个大型的程序而只能使用全局变量是什么感受？

为了解决这个问题，变量的概念被创造了出来，一个变量仅仅是一个代表内存地址的字母数字组合、也可以一个标签或者一个昵称 。

现在，我们与其探讨内存地址，不如说一下变量，变量是我们为内存地址赋予的一个更容易理解的名称，之前的那个程序现在可以这样的解释一下：

![img](https://i.loli.net/2020/05/29/Fd9uvejL2NOKD1P.jpg)

1. 获取变量 a 中存储的值，并将其存储在 CPU 中
2. 将其乘以 3
3. 将结果保存在变量 b 中

这是一个相同的程序，唯一的一个重要改进是我们不在直接关注内存地址了，我们也不在需要持续追踪内存地址，而是把这个苦差事交给了编译器。

现在我们可以像这样写程序了：



```go
var a = 6 
var b = a * 3
```

编译器将确保 a 与 b 的变量被分配唯一的地址，同时保证其值可以在被调用结束之前不被释放。

<br/>

### 到底什么是指针呢？

截止目前我们知道了内存就是一系列有序列号的存储单元，变量就是编译器为内存地址分配的昵称，那么指针是什么呢？

> 指针就是一个指向另一个内存地址变量的值

指针指向变量的内存地址，指针就像该变量值的内存地址一样

我们来看一个代码片段



```go
func main() {
    a := 200
    b := &a
    *b++
    fmt.Println(a)
}
```

> 在 main 函数的第一行，我们定义了一个新的变量 a ，并赋值为 200。接下来我们定义了一个变量 b ，并将变量 a 的地址赋值给 b 。我们并不知道 a 的准确存储地址，但是我们依然可以将 a 的地址存储在变量 b 中。

![img](https://i.loli.net/2020/05/29/bSPQcL6CmwfThex.jpg)

![img](https://i.loli.net/2020/05/29/GKewcBOox7UsFS8.jpg)

因为 Go 强类型的特性，第三行代码也许是最具干扰性的了，b 包含 a 变量的地址，但是我们想增加存储在 a 变量中的值。这样我们必须取消引用 b ，而是跟随指针由 b 引用 a。
 然后我们将该值加 1 后，存储回 b 中存储的内存地址上。
 最后一行打印了 a 的值，可以看到 a 的值已经增加为了 201

![img](https://i.loli.net/2020/05/29/SVNgwbyoq81RIXu.jpg)

![img](https://i.loli.net/2020/05/29/6opzHtAKVwJyXd8.jpg)

<br/>

### 结论

假如你是一个来自没有指针概念或者变量中隐藏了指针的开发语言的开发者，你需要在形成一个指针与变量关联关系的模型，总之记住这个规则：

> 指针是一个指向另一个变量内存地址的值



{% note warning %}
以上关于指针的解释来源于 https://www.jianshu.com/p/44b9429d7bef
{% endnote %}

<br/>

### & 和 *

&：用来查找变量的内存地址

*：用来查找内存地址保存的值

用法如下：

```go
package main

import "fmt"

func main() {
	var a = 20
	var b = &a
	fmt.Printf("a 在内存中的地址是%v, 值是%v", b, *b)
}

```

返回：

> a 在内存中的地址是0xc0000100b0, 值是20

<br/>

### Go 空指针

当一个指针被定义后没有分配到任何变量时，它的值为 nil。

nil 指针也称为空指针。

nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。

一个指针变量通常缩写为 ptr。

```go
package main

import "fmt"

func main() {
   var  ptr *int

   fmt.Printf("ptr 的值为 : %x\n", ptr  )
}
```

返回：

> ptr 的值为 : \<nil>

<br/>

### Go指针更多内容

接下来我们将为大家介绍Go语言中更多的指针应用：

| 内容                                                         | 描述                                         |
| :----------------------------------------------------------- | :------------------------------------------- |
| [Go 指针数组](https://www.runoob.com/go/go-array-of-pointers.html) | 你可以定义一个指针数组来存储地址             |
| [Go 指向指针的指针](https://www.runoob.com/go/go-pointer-to-pointer.html) | Go 支持指向指针的指针                        |
| [Go 向函数传递指针参数](https://www.runoob.com/go/go-passing-pointers-to-functions.html) | 通过引用或地址传参，在函数调用时可以改变其值 |

<br/>

## make 和 new

### new

new是一个内置的函数，它的函数签名如下：

```go
func new(Type) *Type
```

其中，

- Type表示类型，new函数只接受一个参数，这个参数是一个类型
- *Type表示类型指针，new函数返回一个指向该类型内存地址的指针。

new函数不太常用，使用new函数得到的是一个类型的指针，并且该指针对应的值为该类型的零值。举个例子：

```go
func main() {
	a := new(int)
	b := new(bool)
	fmt.Printf("%T\n", a) // *int
	fmt.Printf("%T\n", b) // *bool
	fmt.Println(*a)       // 0
	fmt.Println(*b)       // false
}	
```

<br/>

### make

make也是用于内存分配的，区别于new，它只用于slice、map以及chan的内存创建，而且它返回的类型就是这三个类型本身，而不是他们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。make函数的函数签名如下：

```go
func make(t Type, size ...IntegerType) Type
```

make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作。这个我们在上一章中都有说明，关于channel我们会在后续的章节详细说明。

<br/>

### new与make的区别

1. 二者都是用来做内存分配的。
2. make只用于slice、map以及channel的初始化，返回的还是这三个引用类型本身；
3. 而new用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针。



<br/>

## map(集合)

Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的

<br/>

### 定义map

上文我们学过make 函数，make是专门针对于 slice、map、chan的内存创建。

可以使用内建函数 make 也可以使用 map 关键字来定义 Map:

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 为map在程序中开辟内存空间*/
map_variable := make(map[key_data_type]value_data_type)
```

如果不初始化 map，那么就会创建一个 nil map。nil map 不能用来存放键值对

<br/>

### map 运用

```go
package main

import "fmt"

func main() {
    var countryCapitalMap map[string]string /*创建集合 */
    countryCapitalMap = make(map[string]string)

    /* map插入key - value对,各个国家对应的首都 */
    countryCapitalMap [ "France" ] = "巴黎"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India " ] = "新德里"

    /*使用键输出地图值 */
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }

    /*查看元素在集合中是否存在 */
    capital, ok := countryCapitalMap [ "American" ] /*如果确定是真实的,则存在,否则不存在 */
    /*fmt.Println(capital) */
    /*fmt.Println(ok) */
    if (ok) {
        fmt.Println("American 的首都是", capital)
    } else {
        fmt.Println("American 的首都不存在")
    }
}
```

以上实例运行结果为：

> France 首都是 巴黎
> Italy 首都是 罗马
> Japan 首都是 东京
> India  首都是 新德里
> American 的首都不存在

以上代码中`countryCapitalMap [ "American" ]` 返回一个value 和一个bool 值，go语言中约定成俗使用ok 来接受这个bool 值，判断这个ok的值，来判断是否存在这个key

<br/>

### map的遍历

可以使用range 来遍历map

```go
for k, v := range mapHanshu {
    fmt.Println(k, v)
}
```

<br/>

### delete() 

delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。

```go
package main

import "fmt"

func main() {
        /* 创建map */
        countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}

        fmt.Println("原始地图")

        /* 打印地图 */
        for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [ country ])
        }

        /*删除元素*/ delete(countryCapitalMap, "France")
        fmt.Println("法国条目被删除")

        fmt.Println("删除元素后地图")

        /*打印地图*/
        for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [ country ])
        }
}
```

返回：

> 原始地图
> India 首都是 New delhi
> France 首都是 Paris
> Italy 首都是 Rome
> Japan 首都是 Tokyo
> 法国条目被删除
> 删除元素后地图
> Italy 首都是 Rome
> Japan 首都是 Tokyo
> India 首都是 New delhi



{% note info %}

这里我们可以通过一个命令来查看一些函数的用法，命令如下：

`go doc 包` 或者查看函数 `go doc 包名.函数名`

{% endnote %}

例如：

> D:\Go\src\github.com\ThreeREyed\studygo\day05\pointer>go doc fmt.Printf
> package fmt // import "fmt"
>
> func Printf(format string, a ...interface{}) (n int, err error)
>     Printf formats according to a format specifier and writes to standard
>     output. It returns the number of bytes written and any write error
>     encountered.
>
> D:\Go\src\github.com\ThreeREyed\studygo\day05\pointer>



<br/>

## Go中的range 

Go 语言中 range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 key-value 对。

range 是一个关键字不是函数，所以在使用range的时候，直接在被range的容器前，使用range 关键字即可，一般用于for 循环中

<br/>

{% note warning%}
本节部分内容来源于[菜鸟教程](https://www.runoob.com/go/go-arrays.html)
{% endnote %}