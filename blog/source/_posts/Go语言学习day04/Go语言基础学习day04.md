---
title: Go语言基础学习day04（数组、切片）
date: 2020-05-27 22:31:21
tags: go
categories: Go语言基础
keywords: 
  - 数组
  - 切片
description:
top_img: https://i.loli.net/2020/05/27/HS3ukmqGgh7x2K4.png
comments: true
cover: https://i.loli.net/2020/05/27/HS3ukmqGgh7x2K4.png
toc:
toc_number: true
copyright: true
mathjax:
katex:

---



{% note info %}
本节内容承接上节内容[Go语言基础学习day03](https://threereyed.top/2020/05/26/Go语言学习day03/Go语言基础学习day03/)，关于Go语言的基础学习
{% endnote %}



{% note warning%}
这是一个创建于 2020-05-27 22:31:21 的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}





## 数组

Go 语言提供了数组类型的数据结构。

数组是具有相同唯一类型的一组已编号且长度固定的数据项序列，这种类型可以是任意的原始类型例如整形、字符串或者自定义类型。

数组元素可以通过索引（位置）来读取（或者修改），索引从 0 开始，第一个元素索引为 0，第二个索引为 1，以此类推。



### 数组的声明

Go 语言数组声明需要指定元素类型及元素个数，语法格式如下：

```go
var variable_name [SIZE] variable_type
```

例如：

```go
var array [3]int
```



### 数组的初始化

```go
var array = [3]int{1, 2, 3}
```

初始化数组中 {} 中的元素个数不能大于 [] 中的数字。

如果忽略 [] 中的数字不设置数组大小，Go 语言会根据元素的个数来设置数组的大小

```go
var array = [...]int{1, 2, 3}
```

初始化数组的初始化有多种形式。

```go
[5] int {1,2,3,4,5}
```

长度为5的数组，其元素值依次为：1，2，3，4，5。

```go
[5] int {1,2}
```

长度为 5 的数组，其元素值依次为：1，2，0，0，0 。

在初始化时没有指定初值的元素将会赋值为其元素类型 int 的默认值0，string 的默认值是 ""。

```go
[...] int {1,2,3,4,5}
```

长度为 5 的数组，其长度是根据初始化时指定的元素个数决定的。

```go
[5] int { 2:1,3:2,4:3}
```

长度为 5 的数组，key:value，其元素值依次为：0，0，1，2，3。在初始化时指定了 2，3，4 索引中对应的值：1，2，3

```go
[...] int {2:1,4:3}
```

长度为5的数组，起元素值依次为：0，0，1，0，3。由于指定了最大索引 4 对应的值 3，根据初始化的元素个 数确定其长度为5赋值与使用。



### 访问数组元素

数组元素可以通过索引（位置）来读取。格式为数组名后加中括号，中括号中为索引的值。例如

```go
fmt.println(array[1])
```





## 切片

切片是由数组引出的，因为数组的长度是固定不变的，有的时候我们需要一个可变的元素集合，这个时候就有了动态数组，也就是切片。

内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

切片由三部分组成：指向底层数组的指针、len、cap。



### 切片的定义

切片有两种定义方式

#### 声明一个没有大小的数组来定义切片

不需要说明长度[] 内没有数值

```go
var identifier []type
var a []int
```



#### 使用`make()` 函数来定义

```go
var slice1 []type = make([]type, len)
// 或者
slice1 :=  make([]type, len)
```

```go
package main

import "fmt"

func main() {
	var makeSlice = make([]int, 5, 5)
    var makeSlice = make([]int, 5)
	fmt.Println(makeSlice)
}
```

返回：

> [0 0 0 0 0]



### 切片初始化

```go
s := []int{1,2,3} 
```

直接初始化切片，[]表示是切片类型，{1,2,3}初始化值依次是1,2,3.其cap=len=3

```go
s := arr[:] 
```

初始化切片s, 是数组arr的引用

```go
s := arr[startIndex:endIndex] 
```

将arr中从下标startIndex到endIndex-1 下的元素创建为一个新的切片

```go
s := arr[startIndex:] 
```

默认 endIndex 时将表示一直到arr的最后一个元素

```go
s := arr[:endIndex] 
```

默认 startIndex 时将表示从arr的第一个元素开始

```go
s1 := s[startIndex:endIndex] 
```

通过切片s初始化切片s1

```go
s :=make([]int,len,cap) 
```



注意： 

切片有两种方式，一种是make创建或者声明[]int，另一种是通过数组切片获得，切片是对数组的引用，采用的是引用传递，当给切片赋值后，底层的数组也会发生改变

```go
package main

import "fmt"

func main() {
	var makeSlice = make([]int, 5, 5)
	fmt.Println(makeSlice)

	a := [...]int{1, 2, 3, 4, 5}
	fmt.Printf("a 的类型是%T\n", a)

	b := a[:]
	fmt.Printf("b 的类型是%T, 值是%v\n", b, b)

	b[0] = 10
	fmt.Printf("b 的类型是%T, 值是%v\n", b, b)
	fmt.Printf("a 的类型是%T, 值是%v\n", a, a)

}

```

返回：

> [0 0 0 0 0]
> a 的类型是[5]int
> b 的类型是[]int, 值是[1 2 3 4 5]
> b 的类型是[]int, 值是[10 2 3 4 5]
> a 的类型是[5]int, 值是[10 2 3 4 5]



### len() 和 cap() 函数

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

```go
lenth := len(b)
capB := cap(b)
fmt.Printf("len(b)=%v, cap(b)=%v\n", lenth, capB)

```

返回：

> len(b)=5, cap(b)=5



### 空的切片

```go
var c []int
fmt.Printf("c 的类型是%T，值是%v", c, c)
```

返回：

> c 的类型是[]int，值是[]



### 切片截取

切片的底层是数组，基于数组构建

```go
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

   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */
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

返回：

> len=9 cap=9 slice=[0 1 2 3 4 5 6 7 8]
> numbers == [0 1 2 3 4 5 6 7 8]
> numbers[1:4] == [1 2 3]
> numbers[:3] == [0 1 2]
> numbers[4:] == [4 5 6 7 8]
> len=0 cap=5 slice=[]
> len=2 cap=9 slice=[0 1]
> len=3 cap=7 slice=[2 3 4]



注意：

在以上代码中我们发现最后一个number3 的输出和cap 发现cap发生改变，这是go的切片特性，在从索引0以后进行切片时，容量会发生改变，改变值取决于索引从哪开始 



### append() 和 copy() 函数

如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。

```go
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

返回：

> len=0 cap=0 slice=[]
> len=1 cap=1 slice=[0]
> len=2 cap=2 slice=[0 1]
> len=5 cap=6 slice=[0 1 2 3 4]
> len=5 cap=12 slice=[0 1 2 3 4]



对于同时添加多个元素之后，我们发现初始切片的容量发生了改变，而且这个改变似乎有迹可循，这里对于append 多个元素这里有些规则

#### 添加较少的元素，一次性只添加一个元素

```go
a := []int{}
for i := 0; i < 16; i++ {
    a = append(a, i)
    fmt.Print(cap(a), " ")
}
// 1 2 4 4 8 8 8 8 16 16 16 16 16 16 16 16
```



#### 一次性添加多个元素

```go
a := []int{}
for i := 0; i < 16; i++ {
    a = append(a, 1, 2, 3, 4, 5)
    fmt.Print(cap(a), " ")
}
// 6 12 24 24 48 48 48 48 48 96 96 96 96 96 96 96

c := []int{1, 3}
d := []int{1, 2, 3, 4, 5, 6, 7}
fmt.Printf("len(c)=%v, cap(c)=%v, %v\n", len(c), cap(c), c)
c = append(c, d...)
fmt.Printf("len(c)=%v, cap(c)=%v, %v\n", len(c), cap(c), c)
// len(c)=9, cap(c)=10, [1 3 1 2 3 4 5 6 7]
```

如上代码所示，当我们初始化一个空的切片，然后不断使用append 向其中添加元素，然后查看切片a的容量改变，可以发现，在容量未满之前，容量不变



但是不同的元素类型会有不同的扩容情况

```go
// int8
a := []int8{}
for i := 0; i < 16; i++ {
    a = append(a, 1, 2, 3, 4, 5, 6)
    fmt.Print(cap(a), " ")
}
// 8 16 32 32 32 64 64 64 64 64 128 128 128 128 128 128

// int16
fmt.Println()
b := []int16{}
for i := 0; i < 16; i++ {
    b = append(b, 1, 2, 3, 4, 5)
    fmt.Print(cap(b), " ")
}
// 8 16 32 32 32 64 64 64 64 64 128 128 128 128 128 128

// bool
fmt.Println()
c := []bool{}
for i := 0; i < 16; i++ {
    c = append(c, true, false, true, false, false)
    fmt.Print(cap(c), " ")
}
// 8 16 32 32 32 64 64 64 64 64 128 128 128 128 128 128

// float32
fmt.Println()
d := []float32{}
for i := 0; i < 16; i++ {
    d = append(d, 1.1, 2.2, 3.3, 4.4, 5.5)
    fmt.Print(cap(d), " ")
}
// 8 16 16 32 32 32 64 64 64 64 64 64 128 128 128 128 

// float64
fmt.Println()
e := []float64{}
for i := 0; i < 16; i++ {
    e = append(e, 1.1, 2.2, 3.3, 4.4, 5.5)
    fmt.Print(cap(e), " ")
}
// 6 12 24 24 48 48 48 48 48 96 96 96 96 96 96 96 

// string
fmt.Println()
f := []string{}
for i := 0; i < 16; i++ {
    f = append(f, "1.1", "2.2", "3.3", "4.4", "5.5")
    fmt.Print(cap(f), " ")
}
// 5 10 20 20 40 40 40 40 80 80 80 80 80 80 80 80 

// []int
fmt.Println()
g := [][]int{}
g1 := []int{1, 2, 3, 4, 5}
for i := 0; i < 16; i++ {
    g = append(g, g1, g1, g1, g1, g1)
    fmt.Print(cap(g), " ")
}
// 5 10 20 20 42 42 42 42 85 85 85 85 85 85 85 85
```



以上关于append 的代码来源于[吴子寒的博客](https://www.jianshu.com/p/54be5b08a21c)

关于append 的扩容策略，还请参照源码，网上诸多答案，再加上我自行测试之后，发现都不是很正确，总有不对的地方，所以只能通过源码来看底层策略是如何做的，初学者应该把更多的时间花在其他的方面



#### copy() 函数

copy 函数可以将一个sliece 复制给另一个slice

**数组切片内容复制**

用于将内容从一个数组切片复制到另一个数组切片。如果加入的两个数组切片不一样大，就会按其中较小的那个数组切片的元素个数进行复制。

```
slice1 := []int{1, 2, 3, 4, 5} 
slice2 := []int{5, 4, 3} 

copy(slice2, slice1) // 只会复制slice1的前3个元素到slice2中 
copy(slice1, slice2) // 只会复制slice2的3个元素到slice1的前3个位置
```



copy 函数会创建一个新的数组



{% note warning%}
本节部分内容来源于[菜鸟教程](https://www.runoob.com/go/go-arrays.html)
{% endnote %}