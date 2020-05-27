---
title: Go语言基础学习day02
date: 2020-05-25 09:57:50
tags: 
  - go
categories: 
  - Go语言基础
keywords: 
  - 变量
  - 常量
  - 关键字
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



{% note info %}
本节内容承接上节内容[Go语言基础学习day01](https://threereyed.top/2020/05/20/Go语言学习day01/Go语言基础学习day01/)，关于Go语言的基础学习
{% endnote %}



{% note warning%}
这是一个创建于 2020-05-25 的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}





## Go语言关键字



### go的25个关键字

break default func interface select
case defer go map struct
chan else goto package switch
const fallthrough if range type
continue for import return var



### 关键词解释

1.break：break 用于跳出循环
2.default：用于选择结构的默认选项（switch、select）
3.func：函数定义
4.interface：定义接口
5.select：go语言特有的channel选择结构
6.case：选择结构标签
7.chan：定义channel
8.const：常量
9.continue：跳过本次循环
10.defer：延迟执行内容（收尾工作）有点类似C++的析构，但是它是再函数结尾的时候去执行（也就是栈即将被释放的时候）
11.go：并发执行
12.map：map类型
13.struct：定义结构体
14.else：选择结构
15.goto：跳转语句
16.package：包
17.switch：选择结构
18.fallthrough：如果case带有fallthrough，程序会继续执行下一条case,不会再判断下一条case的值
19.if：选择结构
20.range：从slice、map等结构中取元素
21.type：定义类型
22.for：循环
23.import：导入包
24.return：返回
25.var：定义变量



### go的保留字

```go
Constants:    true  false  iota  nil

Types:    int  int8  int16  int32  int64  
uint  uint8  uint16  uint32  uint64  uintptr
float32  float64  complex128  complex64
bool  byte  rune  string  error

Functions:   make  len  cap  new  append  copy  close  delete
complex  real  imag
panic  recover
```





## 数据类型

数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候需要用大数据的时候才需要申请大内存，就可以充分利用内存。

Go 语言按类别有以下几种数据类型：

| 序号 | 类型和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **布尔型** 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。 |
| 2    | **数字类型** 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。 |
| 3    | **字符串类型:** 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。 |
| 4    | **派生类型:** 包括：(a) 指针类型（Pointer）(b) 数组类型(c) 结构化类型(struct)(d) Channel 类型(e) 函数类型(f) 切片类型(g) 接口类型（interface）(h) Map 类型 |



### 数字类型

Go 也有基于架构的类型，例如：int、uint 和 uintptr。

| 序号 | 类型和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **uint8** 无符号 8 位整型 (0 到 255)                         |
| 2    | **uint16** 无符号 16 位整型 (0 到 65535)                     |
| 3    | **uint32** 无符号 32 位整型 (0 到 4294967295)                |
| 4    | **uint64** 无符号 64 位整型 (0 到 18446744073709551615)      |
| 5    | **int8** 有符号 8 位整型 (-128 到 127)                       |
| 6    | **int16** 有符号 16 位整型 (-32768 到 32767)                 |
| 7    | **int32** 有符号 32 位整型 (-2147483648 到 2147483647)       |
| 8    | **int64** 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |



### 浮点型

| 序号 | 类型和描述                        |
| :--- | :-------------------------------- |
| 1    | **float32** IEEE-754 32位浮点型数 |
| 2    | **float64** IEEE-754 64位浮点型数 |
| 3    | **complex64** 32 位实数和虚数     |
| 4    | **complex128** 64 位实数和虚数    |



### 布尔型

同python 语言不太一样，go语言中的布尔型true 和 false不等于0 和 1，在python 中，True  == 1 False == 0



### 字符串

go语言中的字符串是由双引号包含的，单引号包含的是int32 类型，不是字符串

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var a = 'b'
	fmt.Println(reflect.TypeOf(a).Kind())

	b := "threereyed"
	fmt.Println(reflect.TypeOf(b).Kind())
}
```

以上代码输出为

>int32
>string



### 字符串常用函数

**（1）func HasPrefix(s, prefix string) bool**

判断字符串s是否有前缀字符串prefix

```go
a := "threereyed"
// 判断字符串中是否以指定字符串开头
fmt.Println(strings.HasPrefix(a, "th"))
fmt.Println(strings.HasPrefix(a, "ht"))
```

返回：

>true
>
>false

**（2）func HasSuffix(s, suffix string) bool**

判断字符串s是否有后缀字符串suffix

```go
// 判断字符串中是否以指定字符串结尾
fmt.Println(strings.HasSuffix(a, "ed"))
fmt.Println(strings.HasSuffix(a, "sa"))
```

返回：

>ture
>
>false

**（3）func Contains(s, substr string) bool**

判断字符串s是否包含子串substr

```go
// 判断字符串中是否包含某个字符串
fmt.Println(strings.Contains(a, "ree"))
```

返回：

>true

**（4）func Count(s, sep string) int**

返回字符串s有几个不重复sep子串

```go
// 返回字符串中有几个指定的字符串
fmt.Println(strings.Count(a, "e"))
```

返回：

>4

**（5）func Index(s, sep string) int**

返回字符串s中子串sep第一次出现的位置；不存在返回-1

```go
// 返回字符串中指定字符串第一次出现的位置
fmt.Println(strings.Index(a, "e"))
```

返回：

>3

**（6）func ToLower(s string) string**

返回字符串s转小写的拷贝；

```go
b := "ThreeREyeD"
// 返回转小写后的字符串
fmt.Println(strings.ToLower(b))
```

返回：

>threereyed

**（7）func ToUpper(s string) string**

返回字符串s转大写的拷贝

```go
// 返回转大写后的字符串
fmt.Println(strings.ToUpper(b))
```

返回：

>THREEREYED

**（8）func Repeat(s string, count int) string**

返回count个字符串s串联的字符串

```go
// 将输入的字符串以指定的重复次数拼接后返回
fmt.Println(strings.Repeat(b, 3))
```

返回：

> ThreeREyeDThreeREyeDThreeREyeD

**（9）func Replace(s, old, new string, n int) string**

返回字符串s前n个不重复old子串替换为new子串的新字符串；n<0替换所有old子串；

```go
// 替换字符串中的子字符串，当替换次数小于0的时候替换所有的子字符串
fmt.Println(strings.Replace(b, "e", "T", 1))
fmt.Println(strings.Replace(b, "e", "T", -1))
```

返回：

> ThrTeREyeD
> ThrTTREyTD

**（10）func Trim(s string, cutset string) string**

返回去掉字符串s前后端所有cutset子串的字符串

```go
c := "eeeTRsaeAAAeweee"
// 去掉字符串两边的指定子字符串
fmt.Println(strings.Trim(c, "eee"))
```

返回：

> TRsaeAAAew

**（11）func TrimSpace(s string) string**

返回去掉字符串s前后端空白字符（unicode.IsSpace指定）的字符串；

```go
d := "   eeeTRsaeAAAeweee   "
// 去掉字符串两边的空白符
fmt.Println(strings.TrimSpace(d))
```

返回：

> eeeTRsaeAAAeweee

**（12）func TrimLeft(s string, cutset string) string**

返回去掉字符串s前端所有cutset子串的字符串；

```go
e := "aaayxseryewerrrr"
// 去掉字符串左边的指定子字符串
fmt.Println(strings.TrimLeft(e, "a"))
```

返回：

> yxseryewerrrr

**（13）func TrimRight(s string, cutset string) string**

返回去掉字符串s后端所有cutset子串的字符串；

```go
// 去掉字符串右边的指定子字符串
fmt.Println(strings.TrimRight(e, "r"))
```

返回：

> aaayxseryewe

**（14）func TrimPrefix(s, prefix string) string**

返回去掉字符串s的前缀prefix子串的字符串；

```go
// 去掉字符串开头的指定子字符串
fmt.Println(strings.TrimPrefix(e, "aaayx"))
```

返回：

> seryewerrrr

**（15）func TrimSuffix(s, suffix string) string**

返回去掉字符串s的后缀suffix子串的字符串；

```go
// 去掉字符串结尾的指定子字符串
fmt.Println(strings.TrimSuffix(e, "werrrr"))
```

返回：

> aaayxserye

**（16）func Fields(s string) []string**

返回将字符串s按一个或多个空白（unicode.IsSpace）字符分割的多个字符串切片；空白字符串或空字符串返回空切片；

```go
f := "Hi, Tim, how are you!"
// 将字符串进行切割再返回
fmt.Println(strings.Fields(f))
```

返回：

> [Hi, Tim, how are you!]

**（17）func Split(s, sep string) []string**

返回将字符串s按一个sep子串分割的字符串切片；sep为空字符串时，将s分割为每一个unicode码值的字符串切片；

```go
g := "just relax and take it easy"
// 将字符串以指定字符串进行切割再返回
result := strings.Split(g, " ")
fmt.Printf("result' type: %T\n", result)
fmt.Println(len(result))
```

返回：

> result' type: []string   // 返回的是一个字符串数组
> 6   // 该数组有6个元素

**（18）func Join(a []string, sep string) string**

返回将字符串切片a以子串sep连接的字符串；

```go
// 拼接字符串
h := "I"
i := "love"
j := "you"
fmt.Println(strings.Join([]string{h, i, j}, " "))
```

返回：

> I love you



以上代码省略部分内容，完整代码如下

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	a := "threereyed"
	// 判断字符串中是否以指定字符串开头
	fmt.Println(strings.HasPrefix(a, "th"))
	fmt.Println(strings.HasPrefix(a, "ht"))
	// 判断字符串中是否以指定字符串结尾
	fmt.Println(strings.HasSuffix(a, "ed"))
	fmt.Println(strings.HasSuffix(a, "sa"))
	// 判断字符串中是否包含某个字符串
	fmt.Println(strings.Contains(a, "ree"))
	// 返回字符串中有几个指定的字符串
	fmt.Println(strings.Count(a, "e"))
	// 返回字符串中指定字符串第一次出现的位置
	fmt.Println(strings.Index(a, "e"))

	b := "ThreeREyeD"
	// 返回转小写后的字符串
	fmt.Println(strings.ToLower(b))
	// 返回转大写后的字符串
	fmt.Println(strings.ToUpper(b))
	// 将输入的字符串以指定的重复次数拼接后返回
	fmt.Println(strings.Repeat(b, 3))
	// 替换字符串中的子字符串，当替换次数小于0的时候替换所有的子字符串
	fmt.Println(strings.Replace(b, "e", "T", 1))
	fmt.Println(strings.Replace(b, "e", "T", -1))

	c := "eeeTRsaeAAAeweee"
	// 去掉字符串两边的指定子字符串
	fmt.Println(strings.Trim(c, "eee"))

	d := "   eeeTRsaeAAAeweee   "
	// 去掉字符串两边的空白符
	fmt.Println(strings.TrimSpace(d))

	e := "aaayxseryewerrrr"
	// 去掉字符串左边的指定子字符串
	fmt.Println(strings.TrimLeft(e, "a"))
	// 去掉字符串右边的指定子字符串
	fmt.Println(strings.TrimRight(e, "r"))
	// 去掉字符串开头的指定子字符串
	fmt.Println(strings.TrimPrefix(e, "aaayx"))
	// 去掉字符串结尾的指定子字符串
	fmt.Println(strings.TrimSuffix(e, "werrrr"))

	f := "Hi, Tim, how are you!"
	// 将字符串进行切割再返回
	fmt.Println(strings.Fields(f))

	g := "just relax and take it easy"
	// 将字符串以指定字符串进行切割再返回
	result := strings.Split(g, " ")
	fmt.Printf("result' type: %T\n", result)
	fmt.Println(len(result))

	// 拼接字符串
	h := "I"
	i := "love"
	j := "you"
	fmt.Println(strings.Join([]string{h, i, j}, " "))
}

```

返回：

> true
> false     
> true      
> false     
> true      
> 4
> 3
> threereyed
> THREEREYED
> ThreeREyeDThreeREyeDThreeREyeD
> ThrTeREyeD
> ThrTTREyTD
> TRsaeAAAew
> eeeTRsaeAAAeweee
> yxseryewerrrr
> aaayxseryewe
> seryewerrrr
> aaayxserye
> [Hi, Tim, how are you!]
> result' type: []string
> 6
> I love you



## 变量



### 变量来源

变量来源于数学，是计算机语言中能储存计算结果或能表示值抽象概念。

变量可以通过变量名访问。



### 变量类型

一般常用的有，数值型、布尔型、字符串类型、派生类型

派生类型包括：

- (a) 指针类型（Pointer）
- (b) 数组类型
- (c) 结构化类型(struct)
- (d) Channel 类型
- (e) 函数类型
- (f) 切片类型
- (g) 接口类型（interface）
- (h) Map 类型



### 变量声明



#### 第一种方式：指定变量类型，如果没有初始化，则变量默认为零值

```go
package main

import "fmt"

// 变量的声明
func main() {
	var a int = 1 // 声明一个变量名为a的变量，其类型为int，值为1
	fmt.Println(a)

	var b string = "threereyed" // 声明一个变量名为b的变量，其类型为string，值为threereyed
	fmt.Println(b)

	var c bool = true
	fmt.Println(c)

	// 如果不给变量设置值则默认为零值
	var d int
	fmt.Println(d)
	var e string
	fmt.Println(e)
	var f bool
	fmt.Println(f)
}

```

以上代码的输出为：

> D:\Go\src\github.com\ThreeREyed\studygo\day02>go run main.go
> 1
> threereyed
> true
> 0
>
> false



#### 第二种，根据值自行判定变量类型

```go
// 根据值来判断类型
var g = 20
var h = "threereyed"
fmt.Println(g, h)
```

以上代码的输出为：

> 20 threereyed



#### 第三种，省略 var，:= ， 只在函数中体现（短变量声明）

**注意 \**:=\** 左侧如果没有声明新的变量，就产生编译错误**

```go
// 根据值来判断类型
var g = 20
var h = "threereyed"
fmt.Println(g, h)

// 省略var 使用:= 的方式来声明，:= 左侧必须是有新的变量产生
i := 30
g := 40 // 此时产生编译错误no new variables on left side of :=go
g, j := 40, 50 // 因为左侧有新的变量声明，所以不会有编译错误
```



#### 多变量声明

```go
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误


// 这种因式分解关键字的写法一般用于声明全局变量
var (
    a int
    b string
)
```



#### 注意事项

如果在相同的代码块中，我们不可以再次对于相同名称的变量使用初始化声明，例如：a := 20 就是不被允许的，编译器会提示错误 no new variables on left side of :=，但是 a = 20 是可以的，因为这是给相同的变量赋予一个新的值。

如果你在定义变量 a 之前使用它，则会得到编译错误 undefined: a。

如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误

但是全局变量是**允许声明但不使用**。

同一类型的多个变量可以声明在同一行





## 变量名



### 命名方式

Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字。

**驼峰命名**

大驼峰（eg: ThreeEyed），小驼峰(threeEyed)

**下划线连接**

eg: three_eyed

匿名变量 `-`

空白标识符 _ 也被用于抛弃值，如值 5 在：_, b = 5, 7 中被抛弃。

_ 实际上是一个只写变量，你不能得到它的值。这样做是因为 Go 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值。 





## 常量

常量是一个简单值的标识符，在程序运行过程中，不会被修改的量。

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

**关键字`const`**



### 常量声明方式

**声明单个常量**

```go
const identifier [type] = value
```

你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。

- 显式类型定义： `const b string = "abc"`
- 隐式类型定义： `const b = "abc"`

常量可以用len(), cap(), unsafe.Sizeof()函数计算表达式的值。常量表达式中，函数必须是内置函数，否则编译不过



**单行声明常量**

多个相同类型的声明可以简写为：

```go
const c_name1, c_name2 = value1, value2
```



**批量声明常量**

```go
conset {
	name1 string
	name2 bool
	name3 int
}
```

```go
package main

import "fmt"

const (
	name1 = 1
	name2
	name3
)

func main() {
	fmt.Println(name1, name2, name3)
}
```

const 批量声明常量时，如果没有被赋值，那么上一行的值会赋给下一行没有赋值的常量，以上代码输出内容为

>1 1 1



### iota

#### 常量计数器

iota，特殊常量，可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

```go
package main

import "fmt"

const (
	a = iota
	b
	c
)

const d = iota
const e = iota

func main() {
	fmt.Println(a, b, c, d, e)
}
```

以上代码输出为：

>0 1 2 0 0

批量声明中的iota 随着常量增加而增加，但是const 再出现时，iota 被重新设置为0



```go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```

以上代码输出为：

>0  1 2 ha ha 100 100 7 8

充分说明iota 是一个常量计数器

```go
package main

import "fmt"
const (
    i=1<<iota
    j=3<<iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}
```

以上实例运行结果为：

>i= 1
>j= 6
>k= 12
>l= 24

iota 表示从 0 开始自动加 1，所以 **i=1<<0**, **j=3<<1**（**<<** 表示左移的意思），即：i=1, j=6，这没问题，关键在 k 和 l，从输出结果看 **k=3<<2**，**l=3<<3**。

简单表述:

- i=1

- ：左移 0 位,不变仍为 1;

- **j=3**：左移 1 位,变为二进制 110, 即 6;
- **k=3**：左移 2 位,变为二进制 1100, 即 12;
- **l=3**：左移 3 位,变为二进制 11000,即 24。



#### 用来定义数量级

```go
const (
		_  = iota
		KB = 1 << (10 * iota)
		MB = 1 << (10 * iota)
		GB = 1 << (10 * iota)
		TB = 1 << (10 * iota)
		PB = 1 << (10 * iota)
	)
```



注意：

`<<n == *(2^n)`

左移运算符 **<<** 是双目运算符。左移 n 位就是乘以 2 的 n 次方。 其功能把 **<<** 左边的运算数的各二进位全部左移若干位，由 **<<** 右边的数指定移动的位数，高位丢弃，低位补 0。

右移运算符 **>>** 是双目运算符。右移 n 位就是除以 2 的 n 次方。 其功能是把 **>>** 左边的运算数的各二进位全部右移若干位， **>>** 右边的数指定移动的位数。



{% note warning %}
本节部分内容引自[菜鸟教程](https://www.runoob.com/go/go-variables.html) [李文周的博客](https://www.liwenzhou.com/posts/Go/01_var_and_const/)
{% endnote %}

