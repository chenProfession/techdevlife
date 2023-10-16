---
title: Go语言如何赋值
date: 2023-10-16 23:45:15
categories:
  - [技术教程与指南]
tags:
  - Ethereum
  - development
  - learning path
---
# 3-Go语言如何赋值，比如go语言的多重赋值
笔者这里在学习go语言开发以太坊的时候，遇到过下面的语句：
```go
account := common.HexToAddress("0x71c7656ec7ab88b098defb751b7401b5f6d8976f")
balance, err := client.BalanceAt(context.Background(), account, nil)
if err != nil {
  log.Fatal(err)
}

fmt.Println(balance) 
```
这里笔者有一个疑问，以下语句的赋值是什么意思？
```go
balance, err := client.BalanceAt(context.Background(), account, nil)
```
为什么语句中，左边的变量会有两个而且用逗号隔开？
带着这个疑问，笔者又重新细致的阅读了go语言的赋值语句。
## Go 语言变量
Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字。
声明变量的一般形式是使用 var 关键字：
```go
var identifier type
```
```go
var 变量名称 type
```
可以一次声明多个变量：
```go
var identifier1, identifier2 type
```
案例如下：
```go
var a, c int = 1, 2
```
## 变量声明
### 1. 指定变量类型，如果没有初始化，则变量默认为零值。
```go
// 字符串为 ""（空字符串）
var a string
fmt.Println(a)

// 没有初始化就为零值
var b int
fmt.Println(b)

// bool 零值为 false
var c bool
```
总结一下，如果没有初始化变量，各类型变量的初始值：
* 数值类型（包括complex64/128）为 0
* 布尔类型为 false
* 字符串为 ""（空字符串）
* 以下几种类型为 nil：
  ```go
  var a *int
  var a []int
  var a map[string] int
  var a chan int
  var a func(string) int
  var a error // error 是接口
  ```
### 2. 根据值自行判定变量类型
```go
package main
import "fmt"
func main() {
    var a = true
    fmt.Println(a)
}
```
输出的结果为：
`true`
### 3. 如果变量已经使用 var 声明过了，再使用 := 声明变量，就产生编译错误；直接使用下面的语句即可：
```go
v_name := value
```
### 4. 多变量声明
Go语言支持多重赋值，这意味着你可以同时给多个变量赋值。
**多重赋值：**
```go
package main

import "fmt"

func main() {
    var x, y, z int

    x = 10
    y = 20
    z = 30

    fmt.Println("x:", x)
    fmt.Println("y:", y)
    fmt.Println("z:", z)
}
```

-------
**同时赋值多个变量：**
```go
package main

import "fmt"

func main() {
    x := 10
    y := 20
    z := 30

    fmt.Println("x:", x)
    fmt.Println("y:", y)
    fmt.Println("z:", z)
}
```

-------
**多重赋值示例：**
```go
package main

import "fmt"

func main() {
    a, b := 5, 10

    fmt.Println("Before swap:")
    fmt.Println("a:", a)
    fmt.Println("b:", b)

    a, b = b, a // Swap the values using multiple assignment

    fmt.Println("After swap:")
    fmt.Println("a:", a)
    fmt.Println("b:", b)
}
```
在多重赋值中，右侧的表达式会在赋值操作之前求值，然后将其值赋给左侧的变量。这使得交换两个变量的值非常简单，如上面的示例所示。

-------
**在Go语言中，函数可以返回多个值，这是一项非常有用的特性。返回多个值的函数通常用于在单个函数调用中返回多个相关的结果，以减少函数的调用次数和代码复杂性。多个返回值在Go语言中通常被用于处理成功和错误情况、结果和错误信息等。**
以下是一个返回多个值的函数示例，展示了如何解释和使用这些值：
```go
package main

import "fmt"

// 一个返回姓名和年龄的函数
func getUserInfo() (string, int) {
    name := "Alice"
    age := 30
    return name, age
}

func main() {
    // 调用返回多个值的函数，并将结果分解为独立的变量
    userName, userAge := getUserInfo()

    fmt.Println("User Name:", userName)
    fmt.Println("User Age:", userAge)
}
```
在这个示例中，getUserInfo 函数返回两个值：姓名和年龄。在 main 函数中，我们调用了 getUserInfo 函数并将返回的值分解为 userName 和 userAge 两个变量，然后我们可以分别使用这两个变量。

**这种方式使得函数返回多个相关的值非常方便，而且代码更加清晰易读。当然，你也可以使用 _ 来忽略其中一个或多个不感兴趣的返回值，例如：**
```go
userName, _ := getUserInfo() // 忽略年龄
```
总结起来，Go语言的多返回值机制允许你在函数中返回多个相关的值，并且使用解构赋值的方式将这些值分解到不同的变量中，从而简化代码并提高可读性。
