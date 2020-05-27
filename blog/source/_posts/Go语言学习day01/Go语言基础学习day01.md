---
title: Go语言基础学习day01
date: 2020-05-20 22:17:49
tags: 
  - go
categories: 
  - Go语言基础
keywords: 
description: Go语言基础学习day01
top_img: https://i.loli.net/2020/05/23/H9yRgArpw2NM4ne.jpg
comments: true
cover: https://i.loli.net/2020/05/23/H9yRgArpw2NM4ne.jpg
toc: 
toc_number: true
copyright: 
---

**每天记录一点点之go语言基础学习第一天**





## Golang的环境搭建

1. go下载
2. 配置环境变量

可参考大佬博客 https://www.liwenzhou.com/posts/Go/install_go_dev/





## ide 下载 

go语言支持utf-8编码，只要能支持utf-8编码的都可以写go程序，

一般使用Goland、Vs code、Sublime，Goland 大型项目使用，但是容易卡顿，JB的软件大都这样，Vs code需要自己下载插件来用





## Go 工程目录

![](https://i.loli.net/2020/05/23/fXzDQRLnVgy7AO6.png)





## 第一个go程序

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello World")
}

```

### package

 go的所有文件都需要指定包，包有两种类型，一种是 **main** 包，使用 `package main` 在代码的最前面声明。另外一种就是 **非main** 包，使用 `package`  + `包名` 。main 包的可以有唯一的一个 main 函数，这个函数也是程序的入口。也只有 main 包可以编译成可执行的文件。



### import

`import "fmt"`：最常用的一种形式

`import "./test"`：导入同一目录下test包中的内容

`import f "fmt"`：导入fmt，并给他启别名ｆ

`import . "fmt"`：将fmt启用别名"."，这样就可以直接使用其内容，而不用再添加ｆｍｔ，如fmt.Println可以直接写成Println

`import _ "fmt"` ：表示不使用该包，而是只是使用该包的init函数，并不显示的使用该包的其他内容。注意：这种形式的import，当import时就执行了fmt包中的init函数，而不能够使用该包的其他函数。

`import ("fmt")` 也可以这么写



### main函数

程序的入口



## 编译

- `go build` ：程序写完之后使用go build 进行编译，编译完成后生成可执行文件（windows下为后缀为.exe 的文件）

- `go install`:  go install 执行了两个步骤，先执行go build 再将生成的可执行文件拷贝到go path 的bin目录下

- `go build -o 文件名` :  -o 可以重新指定文件名 

- `go run`:  执行脚本文件

- 交叉编译：

  在windows下可以编译在linux平台执行的文件

  只需要指定目标操作系统的平台和处理器架构即可：

  >```bash
  >SET CGO_ENABLED=0  // 禁用CGO
  >SET GOOS=linux  // 目标平台是linux
  >SET GOARCH=amd64  // 目标处理器架构是amd64
  >```
  >
  >*使用了cgo的代码是不支持跨平台编译的*
  >
  >然后再执行`go build`命令，得到的就是能够在Linux平台运行的可执行文件了。





#### 运行程序

编译之后点击可执行文件既可以运行程序