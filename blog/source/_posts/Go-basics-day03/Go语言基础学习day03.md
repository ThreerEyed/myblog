---


title: Go语言基础学习day03（流程控制、运算符、函数）
date: 2020-05-26 18:03:31
tags: go
categories: Go语言基础
keywords: Go语言基础
description: 关于Go语言的基础学习
top_img: https://i.loli.net/2020/05/26/SDAJuBVNXrpexls.jpg
comments: true
cover: https://i.loli.net/2020/05/26/SDAJuBVNXrpexls.jpg
toc:
toc_number: true
copyright: true
mathjax:
katex:
---



{% note warning%}
这是一个创建于 2020-05-26 18:03:31 的文章，其中的信息可能已经有所发展或是发生改变。
{% endnote %}





## 类型转换

golang是一种强类型语言，虽然在代码中经常看到这种写法，`i:=10`这其实这是编译器自动做了类型推断在编译期间。编译器会对数据进行类型检查。不同类型的数据不能赋值,不能在函数中传参。强类型语言有一些优势，很多的错误会在编译期间被检查出来，不想php和python等弱类型语言，很多错误只有运行到才能被发现。同样，强类型也有一些缺点，写代码的时候要考虑数据类型了，失去了一些灵活性。



```go
package main

import "fmt"

func main() {
    var a float32 = 5.6
    var b int = 10
    fmt.Println (a * b)
}
```

这样的代码会报错，因为类型不匹配
这时候需要强制类型转换

```go
package main

import "fmt"

func main() {
    var a float32 = 5.6
    var b int = 10
    fmt.Println (a * float32(b))
}
```

这样就不会报错了
普通变量类型**int,float,string** 都可以使用 `type (a)`这种形式来进行强制类型转换,比如

```go
var a int32  = 10
var b int64 = int64(a)
var c float32 = 12.3
var d float64 =float64(c)
```





## 流程控制



### go语言条件语句

条件语句需要开发者通过指定一个或多个条件，并通过测试条件是否为 true 来决定是否执行指定语句，并在条件为 false 的情况在执行另外的语句

| 语句                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [if 语句](https://www.runoob.com/go/go-if-statement.html)    | **if 语句** 由一个布尔表达式后紧跟一个或多个语句组成。       |
| [if...else 语句](https://www.runoob.com/go/go-if-else-statement.html) | **if 语句** 后可以使用可选的 **else 语句**, else 语句中的表达式在布尔表达式为 false 时执行。 |
| [if 嵌套语句](https://www.runoob.com/go/go-nested-if-statements.html) | 你可以在 **if** 或 **else if** 语句中嵌入一个或多个 **if** 或 **else if** 语句。 |
| [switch 语句](https://www.runoob.com/go/go-switch-statement.html) | **switch** 语句用于基于不同条件执行不同动作。                |
| [select 语句](https://www.runoob.com/go/go-select-statement.html) | **select** 语句类似于 **switch** 语句，但是select会随机执行一个可运行的case。如果没有case可运行，它将阻塞，直到有case可运行。 |

> 注意：Go 没有三目运算符，所以不支持 **?:** 形式的条件判断。

**f 语句使用 tips**：

**（1）** 不需使用括号将条件包含起来

**（2）** 大括号{}必须存在，即使只有一行语句

**（3）** 左括号必须在if或else的同一行

**（4）** 在if之后，条件语句之前，可以添加变量初始化语句，使用；进行分隔，并且该变量只作用于该if 条件语句中（作用域）

**（5）** 在有返回值的函数中，最终的return不能在条件语句中



### go 语言循环语句

Go 语言提供了以下几种类型循环处理语句：

| 循环类型                                                   | 描述                                 |
| :--------------------------------------------------------- | :----------------------------------- |
| [for 循环](https://www.runoob.com/go/go-for-loop.html)     | 重复执行语句块                       |
| [循环嵌套](https://www.runoob.com/go/go-nested-loops.html) | 在 for 循环中嵌套一个或多个 for 循环 |



使用for 循环打印九九乘法表

```go
package main

import "fmt"

func main() {
	// 打印九九乘法表
	for i := 1; i < 10; i++ {
		for j := 1; j < i+1; j++ {
			fmt.Printf("%dx%d=%d ", j, i, i*j)
		}
		fmt.Println(" ")
	}

}
```

输出：

> 1x1=1
> 1x2=2 2x2=4
> 1x3=3 2x3=6 3x3=9
> 1x4=4 2x4=8 3x4=12 4x4=16
> 1x5=5 2x5=10 3x5=15 4x5=20 5x5=25
> 1x6=6 2x6=12 3x6=18 4x6=24 5x6=30 6x6=36
> 1x7=7 2x7=14 3x7=21 4x7=28 5x7=35 6x7=42 7x7=49
> 1x8=8 2x8=16 3x8=24 4x8=32 5x8=40 6x8=48 7x8=56 8x8=64
> 1x9=9 2x9=18 3x9=27 4x9=36 5x9=45 6x9=54 7x9=63 8x9=72 9x9=81

程序理解：

程序自伤而下运行，当执行到外层for 循环的时候，设置变量i 初始值为1，如果 i < 10 则在本次循环结束之后递增加1，i++ 是在本次循环执行语句完成之后再执行，给定 j 初始值为1，此时1 < 1 + 1成立，执行语句输出，1 x 1 = 1，然后内层for循环的j 再加 1，此时 2 < 1 + 1 不成立，跳出内层for循环，打印换行，再执行外层循环，此时的i == 2，然后依次执行.....






## 循环控制语句

循环控制语句可以控制循环体内语句的执行过程。

GO 语言支持以下几种循环控制语句：

| 控制语句                                                     | 描述                                             |
| :----------------------------------------------------------- | :----------------------------------------------- |
| [break 语句](https://www.runoob.com/go/go-break-statement.html) | 经常用于中断当前 for 循环或跳出 switch 语句      |
| [continue 语句](https://www.runoob.com/go/go-continue-statement.html) | 跳过当前循环的剩余语句，然后继续进行下一轮循环。 |
| [goto 语句](https://www.runoob.com/go/go-goto-statement.html) | 将控制转移到被标记的语句。                       |



break：跳出本层循环，继续往下执行循环之外的代码

continue：跳出本次循环，执行本层循环的下一次循环

goto：跳出多层循环，跳到指定label（标签）





## 运算符

运算符用于在程序运行时执行数学或逻辑运算。

Go 语言内置的运算符有：

- 算术运算符
- 关系运算符
- 逻辑运算符
- 位运算符
- 赋值运算符
- 其他运算符



### 算术运算符

| 运算符 | 描述 | 实例               |
| :----- | :--- | :----------------- |
| +      | 相加 | A + B 输出结果 30  |
| -      | 相减 | A - B 输出结果 -10 |
| *      | 相乘 | A * B 输出结果 200 |
| /      | 相除 | B / A 输出结果 2   |
| %      | 求余 | B % A 输出结果 0   |
| ++     | 自增 | A++ 输出结果 11    |
| --     | 自减 | A-- 输出结果 9     |



### 关系运算符

| 运算符 | 描述                                                         | 实例              |
| :----- | :----------------------------------------------------------- | :---------------- |
| ==     | 检查两个值是否相等，如果相等返回 True 否则返回 False。       | (A == B) 为 False |
| !=     | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。   | (A != B) 为 True  |
| >      | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。   | (A > B) 为 False  |
| <      | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。   | (A < B) 为 True   |
| >=     | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 | (A >= B) 为 False |
| <=     | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 | (A <= B) 为 True  |



### 逻辑运算符

| 运算符 | 描述                                                         | 实例               |
| :----- | :----------------------------------------------------------- | :----------------- |
| &&     | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。 | (A && B) 为 False  |
| \|\|   | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。 | (A \|\| B) 为 True |
| !      | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。 | !(A && B) 为 True  |



### 位运算符

位运算符对整数在内存中的二进制位进行操作。

下表列出了位运算符 &, |, 和 ^ 的计算：

| p    | q    | p & q | p \| q | p ^ q |
| :--- | :--- | :---- | :----- | :---- |
| 0    | 0    | 0     | 0      | 0     |
| 0    | 1    | 0     | 1      | 1     |
| 1    | 1    | 1     | 1      | 0     |
| 1    | 0    | 0     | 1      | 1     |



Go 语言支持的位运算符如下表所示。假定 A 为60，B 为13：

| 运算符 | 描述                                                         | 实例                                   |
| :----- | :----------------------------------------------------------- | :------------------------------------- |
| &      | 按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。 | (A & B) 结果为 12, 二进制为 0000 1100  |
| \|     | 按位或运算符"\|"是双目运算符。 其功能是参与运算的两数各对应的二进位相或 | (A \| B) 结果为 61, 二进制为 0011 1101 |
| ^      | 按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。 | (A ^ B) 结果为 49, 二进制为 0011 0001  |
| <<     | 左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。 其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。 | A << 2 结果为 240 ，二进制为 1111 0000 |
| >>     | 右移运算符">>"是双目运算符。右移n位就是除以2的n次方。 其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数。 | A >> 2 结果为 15 ，二进制为 0000 1111  |



### 赋值运算符

下表列出了所有Go语言的赋值运算符。

| 运算符 | 描述                                           | 实例                                  |
| :----- | :--------------------------------------------- | :------------------------------------ |
| =      | 简单的赋值运算符，将一个表达式的值赋给一个左值 | C = A + B 将 A + B 表达式结果赋值给 C |
| +=     | 相加后再赋值                                   | C += A 等于 C = C + A                 |
| -=     | 相减后再赋值                                   | C -= A 等于 C = C - A                 |
| *=     | 相乘后再赋值                                   | C *= A 等于 C = C * A                 |
| /=     | 相除后再赋值                                   | C /= A 等于 C = C / A                 |
| %=     | 求余后再赋值                                   | C %= A 等于 C = C % A                 |
| <<=    | 左移后赋值                                     | C <<= 2 等于 C = C << 2               |
| >>=    | 右移后赋值                                     | C >>= 2 等于 C = C >> 2               |
| &=     | 按位与后赋值                                   | C &= 2 等于 C = C & 2                 |
| ^=     | 按位异或后赋值                                 | C ^= 2 等于 C = C ^ 2                 |
| \|=    | 按位或后赋值                                   | C \|= 2 等于 C = C \| 2               |



### 其他运算符

下表列出了Go语言的其他运算符。

| 运算符 | 描述             | 实例                       |
| :----- | :--------------- | :------------------------- |
| &      | 返回变量存储地址 | &a; 将给出变量的实际地址。 |
| *      | 指针变量。       | *a; 是一个指针变量         |

实例

```go
package main

import "fmt"

func main() {
  var a int = 4
  var b int32
  var c float32
  var ptr *int

  */\* 运算符实例 \*/*
  fmt.Printf("第 1 行 - a 变量类型为 = %T\n", a );
  fmt.Printf("第 2 行 - b 变量类型为 = %T\n", b );
  fmt.Printf("第 3 行 - c 变量类型为 = %T\n", c );

  */\*  & 和 \* 运算符实例 \*/*
  ptr = &a   */\* 'ptr' 包含了 'a' 变量的地址 \*/*
  fmt.Printf("a 的值为  %d\n", a);
  fmt.Printf("*ptr 为 %d\n", *ptr);
}
```



以上实例运行结果：

> 第 1 行 - a 变量类型为 = int
> 第 2 行 - b 变量类型为 = int32
> 第 3 行 - c 变量类型为 = float32
> a 的值为  4
> *ptr 为 4

指针变量 ***** 和地址值 **&** 的区别：指针变量保存的是一个地址值，会分配独立的内存来存储一个整型数字。当变量前面有 ***** 标识时，才等同于 **&** 的用法，否则会直接输出一个整型数字。

```go
func main() {
   var a int = 4
   var ptr *int
   ptr = &a
   println("a的值为", a);    // 4
   println("*ptr为", *ptr);  // 4
   println("ptr为", ptr);    // 824633794744
}
```



### 运算符优先级

有些运算符拥有较高的优先级，二元运算符的运算方向均是从左至右。下表列出了所有运算符以及它们的优先级，由上至下代表优先级由高到低：

| 优先级 | 运算符           |
| :----- | :--------------- |
| 5      | * / % << >> & &^ |
| 4      | + - \| ^         |
| 3      | == != < <= > >=  |
| 2      | &&               |
| 1      | \|\|             |

当然，你可以通过使用括号来临时提升某个表达式的整体运算优先级。





## 函数

函数是基本的代码块，用于执行一个任务。

Go 语言最少有个 main() 函数。

你可以通过函数来划分不同功能，逻辑上每个函数执行的是指定的任务。

函数声明告诉了编译器函数的名称，返回类型，和参数。

Go 语言标准库提供了多种可动用的内置的函数。例如，len() 函数可以接受不同类型参数并返回该类型的长度。如果我们传入的是字符串则返回字符串的长度，如果传入的是数组，则返回数组中包含的元素个数。



### 函数的定义

Go 语言函数定义格式如下：

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

函数定义解析：

- func：函数由 func 开始声明
- function_name：函数名称，函数名和参数列表一起构成了函数签名。
- parameter list：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
- return_types：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
- 函数体：函数定义的代码集合。

代码示例：

```go
func addNum(a, b int) int {
	var result int
	result = a + b
	return result
}
```



### 函数调用

```go
package main

import "fmt"

func addNum(a, b int) int {
	var result int
	result = a + b
	return result
}

func main() {
	var a int = 100
	var b int = 200
	var result int
	result = addNum(a, b)
	fmt.Println(result)
}

```

返回：

> 300



### 函数返回多个值

Go 函数可以返回多个值，例如：

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
  return y, x
}

func main() {
  a, b := swap("Google", "Runoob")
  fmt.Println(a, b)
}
```



### 函数参数

函数如果使用参数，该变量可称为函数的形参。

形参就像定义在函数体内的局部变量。

调用函数，可以通过两种方式来传递参数：

| 传递类型                                                     | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [值传递](https://www.runoob.com/go/go-function-call-by-value.html) | 值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。 |
| [引用传递](https://www.runoob.com/go/go-function-call-by-reference.html) | 引用传递是指在调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。 |

默认情况下，Go 语言使用的是值传递，即在调用过程中不会影响到实际参数。

------

### 函数用法

| 函数用法                                                     | 描述                                     |
| :----------------------------------------------------------- | :--------------------------------------- |
| [函数作为另外一个函数的实参](https://www.runoob.com/go/go-function-as-values.html) | 函数定义后可作为另外一个函数的实参数传入 |
| [闭包](https://www.runoob.com/go/go-function-closures.html)  | 闭包是匿名函数，可在动态编程中使用       |
| [方法](https://www.runoob.com/go/go-method.html)             | 方法就是一个包含了接受者的函数           |



{% note warning %}
本节内容引自[菜鸟教程](https://www.runoob.com/go/go-decision-making.html) 
{% endnote %}