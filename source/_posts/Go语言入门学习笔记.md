---
title: 入门Go语言学习笔记
author: Oliver
top: false
cover: true
toc: true
mathjax: false
date: 2020-08-25 22:54:11
img: /images/55a9aba97d1e214f849ab2e55f3dabff.jpeg
password:
summary:
tags:
  - Golang
categories:
  - 后端
  - 学习笔记
keywords: golang, go语言, go学习笔记, 学习笔记, go入门, go入门笔记
---

# 1. 编写第一个 Go 程序

## 基本程序结构

```go
// 基本程序结构

package main // 包，表明代码所在模块（包）

import "fmt" // 引入代码依赖

// 功能实现
func main() {
	fmt.Println("hellow world")
}
```

## 应用程序入口

1. 必须是 main 包：package main
2. 必须是 main 方法：func main()
3. 文件名不一定是 main.go

## 退出返回值

**与其他主要编程语言的差异**

- Go 中 main 函数不支持任何返回值
- 通过 os.Exit 来返回状态

```go
package main
import (
  "fmt"
  "os"
)
func main() {
  fmt.Println("hello world")
  os.Exit(0)
}
```

## 获取命令行参数

**与其他主要编程语言的差异**

- main 函数不支持传入参数
  func main(~~arg []string~~)
- 在程序中直接通过 os.Args 获取命令行参数

```go
package main
import (
  "fmt"
  "os"
)
func main() {
  if len(os.Args) > 1 {
    fmt.Println("hello world", os.Args[1])
  }
}
// go run hello_world.go oliver
// hello world oliver
```

# 2. 变量、常量以及与其他语言的差异

## 编写单元测试程序

1. 源码文件以 \_test 结尾：xxx_test.go
2. 测试方法名以 Test 开头：func TestXXX(t \*testing.T) {...}

## VSCode 调试 Go

1. 安装 vscode 插件：Go Autotest
2. 查看 vscode 输出

## 单元测试实现一个斐波那契数列

```go
package fib

import (
  "fmt"
  "testing"
)

func TestFibList() {
  // 赋值方式
  // var a int = 1
  // var b int = 1

  // 赋值方式
  var (
    a int = 1
    b int = 1
  )
  fmt.Print(a)
  for i := 0; i < 5; i++ {
    fmt.Print(" ", b)
    tmp := a
    a = b
    b = tmp + a
  }
  fmt.Println()
}
```

## 变量赋值

**与其他主要编程语言的差异**

- 赋值可以进行自动类型推断
- 在一个赋值语句中可以对多个变量进行同时赋值

```go
/**
  * 交换两个变量的值
  */
func TestExchange() {
  a := 1
  b := 2

  // tmp := a
  // a = b
  // b = tmp

  a, b = b, a // 等价于上面那段代码

  fmt.Print(a， b) // 2 1
}
```

## 常量定义

**与其他主要编程语言的差异**

- 快速设置连续值

```go
// 连续常量简写
const (
  Monday    = iota + 1      // 1
  Tuesday                   // 2
  Wednesday                 // 3
)

// 连续位运常量
const (
  Readable = 1 << iota
  Writable
  Executable
)

func TestContantTry() {
  fmt.Println(Monday, Tuesday) // 1 2
}

func TestContantTry1() {
  a := 7
  fmt.Println(a&Readable == Readable, a&Writable == Writable, a&Executable == Executable) // true true true
}
```

# 3. 基本数据类型

- bool
- string
- int int8 int16 int32 int64
- uint uint8 uint16 uint32 uint64 uintptr
- byte // alias for uint8
- rune // alias fro int32, represnts a Unicode code point
- float32 float64
- complex64 complex128

## 类型转化

**与其他主要编程语言的差异**

1. Go 语言不允许隐式类型转换
2. 别名和原有类型也不能进行隐式类型转换

```go
type MyIntType int64
func TestImplicit() {
    var a int32 = 1
    var b int64
    b = int64(a)
    var c MyIntType
    // c = b              // Error：不允许隐式类型转换
    c = MyIntType(b)
    fmt.Print(a, b, c)    // 1 1 1
}
```

## 类型的预定义值

1. math.MaxInt64
2. math.MaxFloat64
3. math.MaxUnit32

## 指针类型

**与其他主要编程语言的差异**

1. 不支持指针运算
2. string 是值类型，其默认的初始化值为空字符串，而不是 nil

```go
func TestPoint() {
  a := 1
  aPtr := &a
  // aPtr = aPtr + 1             // Error: 不支持指针运算
  fmt.Println(a, aPtr)
  fmt.Printf("%T %T", a, aPtr) // 1 0xc000014258
}

func TestString() {
  var s string
  fmt.Println("*" + s + "*")   // **
  fmt.Println(len(s))             // 0
  if s == "" {
    fmt.Println("空字符串！") // 空字符串！
  }
}
```

# 4. 运算符

## 算数运算符

| 运算符 | 描述 | 实例                |
| ------ | ---- | ------------------- |
| +      | 相加 | A + B 输出结果 30   |
| -      | 相减 | A - B 输出结果 - 10 |
| \*     | 相乘 | A - B 输出结果 200  |
| /      | 相除 | B / A 输出结果 2    |
| %      | 求余 | B % A 输出结果 0    |
| ++     | 自增 | A++ 输出结果 11     |
| --     | 自减 | A-- 输出结果 9      |

**Go 语言没有前置的 ++，--，~~（++a）~~**

## 比较运算符

| 运算符 | 描述                                                         | 实例             |
| ------ | ------------------------------------------------------------ | ---------------- |
| ==     | 检查两个值是否相等，如果相等返回 True 否则返回 False         | (A==B) 为 False  |
| !=     | 检查两个值是否不相等，如果不相等返回 True 否则返回 False     | (A != B) 为 Ttue |
| >      | 检查左边值是否大于右边值，如果是返回 True 否则返回 False     | (A>B) 为 False   |
| <      | 检查左边值是否小于右边值，如果是返回 True 否则返回 False     | (A<B) 为 Ttue    |
| >=     | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False | (A>=B) 为 False  |
| <=     | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False | (A<=B) 为 Ttue   |

## 用 == 比较数组

- 相同维数且含有相同个数元素的数组才可以比较
- 每个元素都相同的才相等

## 位运算符

**与其他主要编程语言的差异**

- &^ 按位置零
  | 实例 | 结果 |
  | --- | --- |
  | 1 &^ 0 | 1 |
  | 1 &^ 1 | 0 |
  | 0 &^ 1 | 0 |
  | 0 &^ 0 | 0 |

# 5. 条件和循环

## 循环

```go
// while 条件循环
// while (n<5)
n := 0
for n < 5 {
    n ++
    fmt.Println(n)
}

// 无限循环
// while (true)
for {
    // do sth
}
```

**与其他主要编程语言的差异**

- Go 语言仅支持循环关键字 for

```go
func TestWhileLoop() {
  n := 0
  for n < 5 {
    n++
    fmt.Println(n)  // 0 1 2 3 4
  }
}
```

## if 条件

```go
if condition {
    // 条件成立
} else {
    // 条件不成立
}

if condition1 {
    // 条件成立
} else if condition2 {
    // 条件成立
} else {
    // 条件不成立
}

if var declaration; conditon {
    // 条件成立
}
```

**与其他主要编程语言的差异**

1. condition 表达式结果必须为布尔值
2. 支持变量赋值

```go
func someFun()  {
    // do sth  
}

func TestIfMultiSec() {
    if v,err := someFun(); err == nil {
        fmt.Println("true")
    } else {
        fmt.Println("false")
    }
}
```

## switch 条件

**与其他主要编程语言的差异**

1. 条件表达式不限制为常量或者整数
2. 单个 case 中，可以出现多个结果选项，使用逗号分隔
3. 与 C 语言等规则相反，Go 语言不需要用 break 来明确退出一个 case
4. 可以不设定 switch 之后的条件表达式，在此种情况下，整个 switch 结构与多个 if...else... 的逻辑作用等同

```go
// 写法一
func TestSwitchMultCase()  {
    for i:= 0; i<5; i++ {
        switch i {
            case 0, 2:
                fmt.Println("Even")
            case 1, 3:
                fmt.Println("Odd")
            default:
                fmt.Println("is not 0-3")
        }
    }
}

// 写法二
func TestSwitchCaseCondition()  {
    for i:= 0; i<5; i++ {
        switch {
            case i%2 == 0:
                fmt.Println("Even")
            case i%2 == 1:
                fmt.Println("Odd")
            default:
                fmt.Println("Unkno")
        }
    }
}
```

# 6. 数组和切片

## 数组的声明

```go
func TestArrayInit() {
  var arr [3]int
  arr1 := [4]int{1, 2, 3, 4}
  arr3 := [...]int{1,3,4,5}
  arr1[1] = 5
  fmt.Println(arr[1], arr[2])
  fmt.Println(arr, arr3)
}
```

## 数组的遍历

```go
func TestArrayTravel()  {
  arr3 := [...]int{1, 3, 4, 5}

  // 写法一
  for i := 0; i < len(arr3); i++ {
    fmt.Println(arr3[i])
  }

  // 写法二
  for idx/*索引*/, e/*元素*/ := range arr3 {
    fmt.Println(idx, e)
  }

  // 写法三 不在乎索引
  for _/*索引*/, e/*元素*/ := range arr3 {
      fmt.Println(e)
  }
}
```

## 数组的截取

```go
func TestArraySection()  {
  arr3 := [...]int{1, 2, 3, 4, 5}
  fmt.Println(arr3[:3]) // [1 2 3]
  fmt.Println(arr3[3:]) // [4 5]
  fmt.Println(arr3[:]) // [1 2 3 4 5]
}
```

## 切片内部结构

![切片内部结构.png](/images/1598367130399.jpg)

## 切片声明

```go
func TestSliceInit() {
  var s0 []int
  fmt.Println(len(s0), cap(s0)) // 0 0
  s0 = append(s0, 1)
  fmt.Println(len(s0), cap(s0)) // 1 1
  
  s1 := []int{1, 2, 3, 4}
  fmt.Println(len(s1), cap(s1)) // 4 4
  
  /**
   * []type, len, cap
   * 其中len个元素会被初始化为默认零值，未初始化元素不可以访问
   */
  s2 := make([]int, 3, 5)
  fmt.Println(len(s2), cap(s2)) // 3 5
  fmt.Println(s2[0], s2[1], s2[2]) // 0 0
  s2 = append(s2, 1)
  fmt.Println(s2[0], s2[1], s2[2], s2[3]) // 0 0 1
  fmt.Println(len(s2), cap(s2)) // 4 5
}
```

## 切片共享存储结构

![切片共享存储结构.png](/images/1598367323596.jpg)

```go
func TestSliceShareMemory()  {
  year:= []string{"jan", "feb", "mar", "apr", "may","jun", "jul", "aug", "sep","oct","nov","dec"}
  Q2 := year[3:6]
  fmt.Println(Q2, len(Q2), cap(Q2)) // [apr may jun] 3 9
  summer := year[5:8]
  fmt.Println(summer, len(summer), cap(summer)) // [jun jul aug] 3 7
  summer[0] = "Unknow"
  fmt.Println(Q2) // [apr may Unknow]
  fmt.Println(year) // [jan feb mar apr may Unknow jul aug sep oct nov dec]
}
```

# 7. Map

## Map 声明

```go
func TestInitMap() {
  m1 := map[int]int{1: 1, 2: 4, 3: 9}
  fmt.Println(m1[2])                       // 4
  fmt.Printf("len m1=%d \n", len(m1))      // len m1=3
  m2 := map[int]int{}
  m2[4] = 16
  fmt.Printf("len m2=%d \n", len(m2))      // len m2=1
  m3 := make(map[int]int, 10)
  fmt.Printf("len m3=%d \n", len(m3))      // len m3=0
}
```

## Map 元素访问

**与其他主要编程语言的差异**
在访问的 Key 不存在时，仍会返回零值，不能通过返回 nil 来判断元素是否存在

```go
func TestAccessNotExistingKey() {
  m1 := map[int]int{}
  t.Log(m1[1])
  m1[2] = 0
  t.Log(m1[2])
  m1[3] = 0
  if v, ok := m1[3]; ok {
    fmt.Println(ok)
    fmt.Printf("key 3's value is %d \n", v)
  } else {
    fmt.Printf("key 3 is not existing. \n")
  }
}
```

## Map 遍历

```go
func TestTravelMap() {
  m1 := map[int]int{1: 1, 2: 4, 3: 9}
  for k, v := range m1 {
    fmt.Println(k, v)
  }
}
/**
控制台输出：
1 1
2 4
3 9
*/

```
