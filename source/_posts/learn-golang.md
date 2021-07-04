---
title: Go 语言入门
date: 2019-06-27 22:34:30
tags:
  - Go
---

# 前言

# 基本数据类型

## 基本数据类型

```golang
// 布尔类型, 默认值为 false
// 布尔类型的值包括 true 和 false
bool

// 字符串，默认值为空字符串 ""
string

// 有符号整形，默认值为 0
int int8 int16 int32 int64

// 无符号整形，默认值为 0
uint uint8 uint16 uint32 uint64 uintptr

// int, uint 和 uintptr 在 32 位系统上通常为 32 位宽，在 64 位系统上则为 64 位宽。
// 推荐使用指定容量的类型

// 字节，uint8 的别名，默认值为 0
byte

// 表示一个 Unicode 码，为 int32 别名，默认值为 0
rune

// 浮点型，默认值为 0
float32 float64

// 复数，默认值为 0
complex64 complex128
```

## 零值

> 数值类型为 0
>
> 布尔类型为 false
>
> 字符串为 ""（空字符串）

## 类型转换

go 中需要显示进行类型转换，没有隐式类型转换

```golang
a := 1
b := float64(a)
c := uint(b)
```

# 变量与常量

## 变量

声明一个 int 类型的变量，其初始值为 0

```go
var a int

// 如果声明多个类型相同的变量，可采用如下形式
var a, b, c int
```

声明一个 int 类型的变量，并为其赋值 9000

```golang
var a int = 9000
var a, b, c int = 1, 2, 3

// 或者，此时 go 会自动推导出变量类型
var a = 1000
var a, b, c = 1, 2, 3
```

你也可以使用简洁赋值语句，这种写法和上面的结果一致，但是更简洁

```golang
// 此处变量的类型会被自动推到出来
a := 9000
```

当你有多个变量需要声明并且赋值时，可以采用如下写法

```golang
// 声明、赋值
a, b := 1, "hello"

// 只有赋值
a, b = 2, "world"
```

在同一个作用域中，变量智能被声明一次。如下写法会报错

```golang
a := 1

// no new variables on left side of :=
a := 2
```

如果使用多变量声明赋值语句，其中可以出现一声明过的变量

```golang
a := 1

// 这样写是没问题的。此处不能传入与 a 类型不匹配的值
a, b := 2, "hello"
```

忽略多返回值中的某个值，可以使用 `-` 操作符

```golang
_, b := moreReturnFunc()
b, _ := moreReturnFunc()
b := moreReturnFunc() // 和上面的写法等价
```

## 常量

常量使用如下方式定义，其格式和变量差不多，只不过需要使用 const 关键字

```golang
const a = "world"
const result = true // 未指定类型，由上下文推导得出
const b float64 = 0.2 // 指定类型
```

> 常量不可以使用 := 语法声明

常量的类型可以是字符、字符串、布尔值或着数值

当未指定常量类型时，其类型有上下文推导得出

# 流程控制

## if else 语句

完整形态

```golang
// else if 和 else 均可省略
if x == 0 {

} else if x > 0 {

} else {

}
```

> **左大括号必须在 `if`、`else if` 或 `else` 的后面**

可以使用如下的简短形式，其中声明的变量只在 `if` 范围内可以使用

```golang
if v := math.Pow(x, n); v < lim {
  return v
}
```

## switch 语句

`switch` 语句的一般表现形式。与 C 语言不同，go 执行完一个 case 之后，会只接退出。

```golang
switch finger := 4; finger {
  case 1:
    fmt.Println("Thumb")
  case 2:
    fmt.Println("Index")
  case 3:
    fmt.Println("Middle")
  case 4:
    fmt.Println("Ring")
  case 5:
    fmt.Println("Pinky")
  default:
    fmt.Println("Error")
}
```

`case` 中可以包含多个表达式，每个表达式用逗号分隔

```golang
letter := "i"
switch letter {
  case "a", "e", "i", "o", "u": //multiple expressions in case
    fmt.Println("vowel")
  default:
    fmt.Println("not a vowel")
}
```

`switch` 中的表达式是可选的，可以省略。如果省略表达式，则相当于 `switch true`，这种情况下会将每一个 `case` 的表达式的求值结果与 `true` 做比较，如果相等，则执行相应的代码。

```golang
num := 75
switch { // expression is omitted
  case num >= 0 && num <= 50:
    fmt.Println("num is greater than 0 and less than 50")
  case num >= 51 && num <= 100:
    fmt.Println("num is greater than 51 and less than 100")
  case num >= 101:
    fmt.Println("num is greater than 100")
}
```

如果想执行完一个 case 之后需要继续匹配之后的 case，需要使用 fallthrough 语句。
此时的效果类似 c 语言的 case 内不写 break

```golang
switch num := number(); { //num is not a constant
  case num < 50:
    fmt.Printf("%d is lesser than 50\n", num)
    fallthrough
  case num < 100:
    fmt.Printf("%d is lesser than 100\n", num)
    fallthrough
  case num < 200:
    fmt.Printf("%d is lesser than 200", num)
}
```

## for 循环语句

for 循环完整形态

```golang
for 初始化语句; 条件表达式; 后置语句 {
  循环体
}
```

> **左大括号必须在 for 的后面**

```golang
sum := 0
for i := 0; i < 10; i++ {
  sum += i
}
```

初始化语句和后置语句均为可选

```golang
sum := 0
for ; sum < 1000; {
  sum += 1
}
```

此时 ';' 也可以省略。这种写法和 C 语言中的 while 语句相同

```golang
sum := 0
for sum < 1000 {
  sum += 1
}
```

如果需要使用死循环，可用如下形式

```golang
for true {

}

// true 可省略
for {

}
```

# 数组、切片与指针

## 数组的声明、初始化

go 可以使用如下方式初始化数组

> Go 中数组是值类型，不是引用类型，切记。当把数组变量被赋值时，获得的是愿数组的拷贝。改变新数组不会导致就数组的内容改变

```golang
var a [10]int // 此时数组元素值为零值

// 多维数组
var a [1][2][3]int

primes := [6]int{2, 3, 5, 7, 11, 13} // 声明并初始化数组
```

可以使用如下方式进行初始化

```golang
a = {'1','2','3'}
d = {{1,2,3},{4,5,6}}

// 声明并初始化一个长度为3的byte数组
a := [3]byte{'1', '2', '3'}

// 可以省略长度而采用`...`的方式，Go会自动根据元素个数来计算长度
a := [...]byte{'1', '2', '3'}
d := [2][3]int{[3]int{1, 2, 3}, [3]int{4, 5, 6}}

// 如果内部的元素和外部的一样，那么上面的声明可以简化，直接忽略内部的类型
d := [2][3]int{{1, 2, 3},{4, 5, 6}}
d := [...][3]int{{1, 2, 3}, {4, 5, 6}}
```

## 切片

> 切片本身不包含任何内容，它是对原数组数据的部分引用，修改切片的内容会导致原数组的内容被修改。

```golang
var a []int // 此时 a 的值为 nil，但它确实是切片，大小为 0

a := []int{1, 2, 3, 4}

// 以下切片形式等价
a := [4]int{1, 2, 3, 4}

a[0:4]
a[0:]
a[:4]
a[:]
```

## 切片的长度和容量

切片的长度是指切片中元素的个数。切片的容量是指从切片的起始元素开始到其底层数组中的最后一个元素的个数。

## 数组、切片的元素访问

```golang
// 可以使用内置函数获取数组长度
var a [10]int{1, 2, 3, 4, 5}
arrLength := len(a)

// 获取第一个位置的元素
b := a[0]
```

# 映射

# 函数相关

## defer 语句

# 结构体

# 包管理相关
