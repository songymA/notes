golang细节入门

[TOC]





### 

# 一、关于package

## 1.1 理解package

### 什么是Package？

想象一下，**包**就像学校里的**班级**。每个班级里会有不同的学生（代码），他们有各自的特点和功能。通过将学生分到不同的班级，我们可以更好地管理和组织这些学生。同样，Go语言中的包（package）用来组织和管理相关的代码。

### Package的作用

1. **组织代码**：
   - 在一个大的项目中，如果把所有的代码都放在一起，就会变得非常混乱。使用包可以把相关的代码放在一起。例如，你可以有一个叫做`math`的包，里面放着所有与数学相关的函数，比如加法、减法等。

2. **避免命名冲突**：
   - 假设有两个班级都有一个叫“John”的学生，如果不加以区分我们就不知道指的是哪一位。同样，在程序中，两个不同的包可以有相同的函数名，但因为它们属于不同的包，所以不会发生冲突。比如，你可以在一个包里有一个叫`Print`的函数，在另一个包里也有一个这样名字的函数，它们不会互相影响。

3. **重用代码**：
   - 如果你写了一个很有用的函数，比如计算面积的函数，你可以把这个函数放在一个包里，以后在其他项目中也可以直接使用这个包，而不用重复写代码。这就像你在学校里学习的知识，可以在未来的课程中继续使用。

4. **提高可读性**：
   - 使用包可以让程序的结构更加清晰。开发者一看到包的名字，就能大致知道这个包里存放了什么样的功能。比如，`fmt`包通常用于格式化输入输出，这样其他人阅读你的代码时能够更快理解。

5. **模块化开发**：
   - 包允许开发者将复杂的程序拆分成多个小部分（模块），每个包负责自己的功能。这样做不仅让程序更加简洁，也便于多人协作开发，因为每个人可以专注于自己负责的包。

### 举个例子

假设你正在开发一个小游戏。在这个游戏中，你可能需要处理玩家的角色、敌人、道具等内容。你可以创建几个包：

- `player`: 处理与玩家相关的功能，比如移动、攻击等。
- `enemy`: 处理敌人的行为。
- `item`: 用于管理游戏中的道具。

这样，每个包只关注自己的职责，使得整个游戏的代码结构更加清晰、易于管理和维护。

### 总结

在Go语言中，`package`就像学校里的班级，帮助我们组织、管理并重用代码。通过使用包，我们可以确保代码的整洁性、可读性和可维护性，让编程变得更加高效和愉快！



## 1.2 package的用法细节

### Package的作用

在Go语言中，`package`（包）是一种组织和管理代码的方式，类似于学校里的班级。它有以下几个主要作用：

1. **组织代码**：将相关的代码放在同一个包中，方便管理和维护。
2. **避免命名冲突**：不同包中可以有相同的函数名或变量名，而不会相互冲突。
3. **重用代码**：可以将常用的功能封装成包，以便在不同的项目中重复使用。
4. **提高可读性**：通过包名可以快速了解代码的功能和用途。
5. **模块化开发**：将程序拆分成多个包，便于多人协作和独立开发。

### Package的用法细节

#### 1. 声明与命名

- **声明**：每个Go源文件的开头必须声明其所属的包。
    ```go
    package mypackage // 声明该文件属于mypackage包
    ```
- **命名规范**：包名应简短、清晰，并使用全小写字母，通常与所在目录名相同。

#### 2. 主包（`main`）

- **特殊性**：`main`包用于构建可执行程序，包含程序的入口点`main`函数。
    ```go
    package main // 声明main包
    
    import "fmt"
    
    func main() { // 程序入口点
        fmt.Println("Hello, Go!")
    }
    ```

#### 3. 同一目录下的包

- **一致性**：同一目录下所有非测试的`.go`文件必须声明相同的包名。
    ```
    // 文件: greeting/hello.go
    package greeting // 声明属于greeting包
    
    import "fmt"
    
    func SayHello(name string) {
        fmt.Printf("Hello, %s!\n", name)
    }
    
    // 文件: greeting/goodbye.go
    package greeting // 声明属于greeting包
    
    import "fmt"
    
    func SayGoodbye(name string) {
        fmt.Printf("Goodbye, %s!\n", name)
    }
    ```

#### 4. 测试文件的包名

- **内部测试**：测试文件与被测试文件在同一个包内，可以访问私有成员。
    ```go
    // 文件: greeting/greeting_test.go
    package greeting // 与被测试文件相同的包名
    
    import "testing"
    
    func TestSayHello(t *testing.T) {
        SayHello("Go") // 可以调用包内的私有和公开函数
    }
    ```
- **外部测试**：测试文件包名为`被测试包名_test`，只能访问公开成员。
    ```go
    // 文件: greeting/external_test.go
    package greeting_test // 使用_test后缀的包名
    
    import (
        "testing"
        "your_module/greeting" // 导入被测试的包，your_module是你的模块名
    )
    
    func TestSayHello(t *testing.T) {
        greeting.SayHello("Go") // 只能调用公开的函数
    }
    ```

#### 5. 包的导入（`import`）

- **导入路径**：使用`import`语句导入其他包。
    ```go
    import "fmt" // 导入标准库中的fmt包
    import "math/rand" // 导入math包下的rand子包
    ```
- **多包导入**：使用括号一次性导入多个包。
    ```go
    import (
        "fmt"
        "math/rand"
    )
    ```

#### 6. 别名导入

- **简化名称**：为导入的包设置别名。
    ```go
    import (
        r "math/rand" // 为math/rand包设置别名r
    )
    
    // 使用别名
    randomNumber := r.Intn(100) // 调用rand包中的Intn函数
    ```

#### 7. 空白导入

- **执行init函数**：仅执行包的初始化函数而不使用其成员。
    ```go
    import _ "github.com/some/package" // 执行该包的init函数，但不直接使用其成员
    ```

#### 8. 可见性

- **大小写控制**：首字母大写的标识符（变量、函数等）是公开的，可被外部包访问；首字母小写的标识符是私有的，只能在声明它的包内部访问。
    ```go
    package mypackage
    
    var PublicVar = "Public" // 公开的变量
    var privateVar = "Private" // 私有变量
    
    func PublicFunction() { // 公开的函数
        // ...
    }
    
    func privateFunction() { // 私有函数
        // ...
    }
    ```

#### 9. 包的初始化

- **`init`函数**：包可以包含`init`函数，在包被导入时自动执行。
    ```go
    package mypackage
    
    import "fmt"
    
    func init() {
        fmt.Println("mypackage initialized") // 包初始化时执行
    }
    ```

#### 10. 模块和依赖管理

- **Go模块**：使用`go.mod`文件管理项目的模块和依赖。
    ```
    module mymodule // 声明模块名
    
    go 1.16 // 指定Go版本
    
    require (
        github.com/some/package v1.2.3 // 声明依赖及其版本
    )
    ```

### 总结

`package`在Go语言中起着组织代码、避免命名冲突、代码重用、提高可读性和模块化开发的作用。理解`package`的声明规则、命名规范、导入方式、可见性控制以及初始化机制，对于编写清晰、可维护的Go代码至关重要。通过上述的详细解释和代码示例，希望能够帮助你更好地理解和运用Go语言的`package`机制。



## 1.3 package的一些其他常见误区理解

### package声明

每一个文件必须有一个package声明：

- 同一个目录下，go文件package声明必须一致。可以不一致的情况为：
  - 测试包后缀
- package名字可以和目录名字不一样
- 一般用法时候是少有不一致的情况的

在Go语言中，同一个目录下的`.go`文件，其`package`声明的一致性规则主要取决于这些文件是**用于构建可执行程序**还是**作为库的一部分**，以及是否涉及**测试文件**。我们来详细探讨一下这些情况，并给出相应的例子。

### 必须一致的情况

当你在开发一个库或者一个可执行程序时，同一个目录下的所有`.go`文件（除了测试文件）的`package`声明必须相同。这是因为这些文件共同构成了一个包的完整实现。

**例子：**

假设我们正在开发一个名为`greeting`的包，该包提供了一个简单的问候功能。我们可能会有以下文件结构：

```
greeting/
├── hello.go
└── goodbye.go
```

- `hello.go`文件的内容可能如下：

```go
package greeting

import "fmt"

func SayHello(name string) {
    fmt.Printf("Hello, %s!\n", name)
}
```

- `goodbye.go`文件的内容可能如下：

```go
package greeting

import "fmt"

func SayGoodbye(name string) {
    fmt.Printf("Goodbye, %s!\n", name)
}
```

在这个例子中，`hello.go`和`goodbye.go`都声明了`package greeting`，因为它们共同实现了`greeting`包的功能。

### 可以不一致的情况

当涉及到测试文件时，`package`声明可以与被测试的包不同。具体来说，测试文件有两种命名方式，对应的`package`声明规则也不同：

1. **内部测试**：如果测试文件与被测试的文件位于同一个包内（即`package`声明相同），那么这个测试被称为内部测试。它可以访问被测试包中的私有函数和变量。

2. **外部测试**：如果测试文件的`package`声明为`被测试包名_test`，那么这个测试被称为外部测试。它只能访问被测试包中的公开函数和变量。

**例子：**

假设我们继续为`greeting`包编写测试。我们可以创建以下文件：

```
greeting/
├── hello.go
├── goodbye.go
├── greeting_test.go  // 内部测试
└── external_test.go // 外部测试
```

- `greeting_test.go`文件的内容可能如下：

```go
package greeting

import "testing"

func TestSayHello(t *testing.T) {
    SayHello("Go") // 可以调用私有和公开的函数
}
```

- `external_test.go`文件的内容可能如下：

```go
package greeting_test

import (
    "testing"
    "your_module/greeting" // 假设的模块路径
)

func TestSayHello(t *testing.T) {
    greeting.SayHello("Go") // 只能调用公开的函数
}
```

在这个例子中，`greeting_test.go`与`hello.go`和`goodbye.go`的`package`声明相同，都是`greeting`，因此它可以访问这些文件中的所有函数。而`external_test.go`的`package`声明为`greeting_test`，它只能访问`greeting`包中的公开函数。

### 总结

- **同一目录下非测试文件的`package`声明必须一致**：当这些文件共同构成一个包的实现时。
- **测试文件的`package`声明可以不同**：测试文件可以选择与被测试包相同的`package`（内部测试），或者使用`被测试包名_test`作为`package`（外部测试）。外部测试提供了一种从外部角度测试包的方式，只能访问包的公开API。

理解这些规则对于组织和构建Go项目非常重要，特别是在涉及到大型项目和团队协作时。正确的`package`声明和项目结构可以提高代码的可读性、可维护性和可测试性。







# 二、golang的基本类型

## 2.1 基本类型介绍

好的，下面是对Go语言（Golang）基本类型的详细解释。Go语言是一门静态类型语言，它的基本类型包括整数、浮点数、复数、布尔值、字符串，以及这些基础类型衍生出的一些特殊类型，如字节（byte）和Unicode字符（rune）。

### 1. 整数类型（Integer）

Go语言提供了多种整数类型，分为有符号和无符号两类，以及不同的大小（位宽）。

#### 有符号整数：

- `int8`: 8位有符号整数（-128 到 127）
- `int16`: 16位有符号整数（-32768 到 32767）
- `int32`: 32位有符号整数（-2147483648 到 2147483647）
- `int64`: 64位有符号整数（-9223372036854775808 到 9223372036854775807）
- `int`: 32位或64位有符号整数，具体大小取决于编译器和操作系统（在32位系统上是32位，在64位系统上是64位）

#### 无符号整数：

- `uint8`: 8位无符号整数（0 到 255）
- `uint16`: 16位无符号整数（0 到 65535）
- `uint32`: 32位无符号整数（0 到 4294967295）
- `uint64`: 64位无符号整数（0 到 18446744073709551615）
- `uint`: 32位或64位无符号整数，具体大小取决于编译器和操作系统（在32位系统上是32位，在64位系统上是64位）
- `uintptr`: 无符号整数，用于表示指针值，其大小足以存储任何指针的位模式。

**示例：**

```go
package main

import "fmt"

func main() {
    var a int8 = 10
    var b uint32 = 1000
    var c int = -500
    var ptr uintptr = 0x12345678

    fmt.Println(a, b, c, ptr)
}
```

### 2. 浮点数类型（Floating-Point）

Go语言提供了两种精度的浮点数类型：

- `float32`: 32位浮点数（IEEE 754标准），大约有7个十进制数的精度
- `float64`: 64位浮点数（IEEE 754标准），大约有15个十进制数的精度

默认情况下，浮点数字面量（如`3.14`）的类型是`float64`。

**示例：**

```go
package main

import "fmt"

func main() {
    var x float32 = 3.14
    var y float64 = 2.71828

    fmt.Println(x, y)
}
```

### 3. 复数类型（Complex）

Go语言内置了对复数的支持，有两种复数类型：

- `complex64`: 实部和虚部都是`float32`类型的复数
- `complex128`: 实部和虚部都是`float64`类型的复数

复数可以通过内置函数`complex`创建，实部和虚部可以通过`real`和`imag`函数获取。

**示例：**

```go
package main

import "fmt"

func main() {
    var c1 complex64 = complex(1, 2) // 1 + 2i
    var c2 complex128 = complex(3, 4) // 3 + 4i

    fmt.Println(c1, c2)
    fmt.Println(real(c1), imag(c1)) // 输出c1的实部和虚部
    fmt.Println(real(c2), imag(c2)) // 输出c2的实部和虚部
}
```

### 4. 布尔类型（Boolean）

布尔类型`bool`表示逻辑值，只有两个可能的值：`true`和`false`。

**示例：**

```go
package main

import "fmt"

func main() {
    var isTrue bool = true
    var isFalse bool = false

    fmt.Println(isTrue, isFalse)
}
```

### 5. 字符串类型（String）

字符串类型`string`表示一串不可变的UTF-8编码的字符序列。字符串可以用双引号`""`或反引号``` `` ```表示。

- 双引号表示的字符串可以包含转义字符，如`\n`表示换行。
- 反引号表示的字符串是原始字符串，其中的转义字符不会被转义，可以用于表示多行文本。

**示例：**

```go
package main

import "fmt"

func main() {
    str1 := "Hello, \nGo!" // 使用双引号，\n会被转义为换行符
    str2 := `Hello, 
Go!` // 使用反引号，原样输出

    fmt.Println(str1)
    fmt.Println(str2)
}
```

### 6. 字节类型（Byte）

字节类型`byte`是`uint8`的别名，用于表示单个字节的值。在处理字节流或ASCII字符时常用。

**示例：**

```go
package main

import "fmt"

func main() {
    var b byte = 'A' // 字符'A'的ASCII值是65

    fmt.Println(b)
    fmt.Printf("%c\n", b) // 使用%c格式化输出字符
}
```

### 7. Unicode字符类型（Rune）

`rune`类型是`int32`的别名，用于表示一个Unicode码点。在处理Unicode字符时，特别是那些超过一个字节的字符，`rune`类型很有用。

**示例：**

```go
package main

import "fmt"

func main() {
    var r rune = '世' // '世'是一个Unicode字符

    fmt.Println(r)
    fmt.Printf("%c\n", r) // 输出字符'世'
}
```









## 2.2 string类型的具体操作

### `[]byte` 是什么？

在 Go 语言中，`[]byte` 表示一个**字节切片（byte slice）**。

*   **字节（byte）：**  如前所述，`byte` 是 `uint8` 的别名，表示一个 8 位无符号整数，通常用于表示一个字节的数据。
*   **切片（slice）：**  切片是 Go 语言中一种灵活的、动态大小的序列。你可以把它看作是对数组的引用，但切片本身可以增长或缩小。

所以，`[]byte` 就是一个**字节的序列**，它可以用来存储和操作二进制数据。

**与字符串 `string` 的关系:**

在 Go 中，字符串 `string` 在底层也是通过字节数组来表示的。你可以将字符串转换为字节切片，也可以将字节切片转换为字符串。

**示例：**

```go
package main

import "fmt"

func main() {
	// 将字符串转换为字节切片
	str := "Hello, Go!"
	byteSlice := []byte(str)
	fmt.Println(byteSlice) // 输出: [72 101 108 108 111 44 32 71 111 33] (每个字节的十进制表示)

	// 将字节切片转换为字符串
	newStr := string(byteSlice)
	fmt.Println(newStr) // 输出: Hello, Go!
}
```

**`[]byte` 的常见用途:**

*   **处理二进制数据：** 例如读取或写入文件、网络通信等。
*   **修改字符串内容：**  由于字符串在 Go 中是不可变的，因此如果你想要修改字符串中的某些字节，你需要先将其转换为 `[]byte`，修改后再转换回 `string`。
*   **字节级别的操作：**  当需要对数据进行字节级别的操作时，例如加密、压缩等。

### `string` 的具体操作

Go 语言的 `string` 类型提供了许多内置的操作，以下是一些常见的操作：

#### 1. 访问字符串中的字符（实际上是字节）

你可以使用索引来访问字符串中的字节，索引从 0 开始。请注意，由于 Go 字符串使用 UTF-8 编码，一个字符可能由多个字节组成。

```go
str := "Hello, 世界"
fmt.Println(str[0]) // 输出: 72 (H的ASCII值)
fmt.Println(str[7]) // 输出: 228 (世的UTF-8编码的第一个字节)
```

#### 2. 获取字符串长度

使用内置函数 `len()` 可以获取字符串的**字节长度**（不是字符个数）。

```go
str := "Hello, 世界"
fmt.Println(len(str)) // 输出: 13 (因为"世界"每个字占3个字节)
```

#### 3. 字符串拼接

可以使用 `+` 运算符拼接字符串。

```go
str1 := "Hello"
str2 := "Go"
str3 := str1 + ", " + str2 + "!"
fmt.Println(str3) // 输出: Hello, Go!
```

#### 4. 字符串比较

可以使用 `==`, `!=`, `<`, `>`, `<=`, `>=` 等比较运算符比较字符串。

```go
str1 := "apple"
str2 := "banana"
fmt.Println(str1 == str2) // 输出: false
fmt.Println(str1 < str2)  // 输出: true (按字典序比较)
```

#### 5. 字符串切片

可以像数组一样对字符串进行切片操作，得到一个子串。切片操作是基于字节的。

```go
str := "Hello, Go!"
subStr := str[0:5]
fmt.Println(subStr) // 输出: Hello
```

#### 6. 字符串遍历

可以使用 `for range` 循环遍历字符串。每次迭代会返回一个字符的起始字节索引和对应的 `rune` 值（Unicode 码点）。

```go
str := "Hello, 世界"
for index, char := range str {
    fmt.Printf("Character: %c, Index: %d\n", char, index)
}
```

#### 7. `strings` 包中的常用函数

`strings` 包提供了许多用于操作字符串的实用函数：

*   **`strings.Contains(s, substr string) bool`**: 判断字符串 `s` 是否包含子串 `substr`。
*   **`strings.Index(s, substr string) int`**: 返回子串 `substr` 在字符串 `s` 中第一次出现的索引，如果不存在则返回 -1。
*   **`strings.LastIndex(s, substr string) int`**: 返回子串 `substr` 在字符串 `s` 中最后一次出现的索引，如果不存在则返回 -1。
*   **`strings.Replace(s, old, new string, n int) string`**: 将字符串 `s` 中的 `old` 子串替换为 `new` 子串，`n` 表示替换次数，如果 `n` 为 -1 则替换所有。
*   **`strings.Split(s, sep string) []string`**: 使用 `sep` 作为分隔符将字符串 `s` 分割成一个字符串切片。
*   **`strings.Join(a []string, sep string) string`**: 使用 `sep` 作为分隔符将字符串切片 `a` 拼接成一个字符串。
*   **`strings.ToLower(s string) string`**: 将字符串 `s` 转换为小写。
*   **`strings.ToUpper(s string) string`**: 将字符串 `s` 转换为大写。
*   **`strings.Trim(s string, cutset string) string`**：从字符串 `s` 的开头和结尾移除所有包含在 `cutset` 中的字符。
*   **`strings.TrimSpace(s string) string`**：从字符串 `s` 的开头和结尾移除所有空白字符（空格、制表符、换行符等）。
*   **`strings.HasPrefix(s, prefix string) bool`**：检查字符串 `s` 是否以 `prefix` 开头。
*   **`strings.HasSuffix(s, suffix string) bool`**：检查字符串 `s` 是否以 `suffix` 结尾。

**示例：**

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	str := "Hello, Go! This is a test string."

	fmt.Println(strings.Contains(str, "Go"))         // 输出: true
	fmt.Println(strings.Index(str, "is"))          // 输出: 12
	fmt.Println(strings.Replace(str, " ", "-", -1)) // 输出: Hello,-Go!-This-is-a-test-string.
	fmt.Println(strings.Split(str, " "))           // 输出: [Hello, Go! This is a test string.]
	fmt.Println(strings.ToLower(str))              // 输出: hello, go! this is a test string.
	fmt.Println(strings.ToUpper(str))              // 输出: HELLO, GO! THIS IS A TEST STRING.
	fmt.Println(strings.TrimSpace("   Hello, Go!   ")) // 输出: Hello, Go!
    fmt.Println(strings.HasPrefix("filename.txt", "file")) // 输出: true
    fmt.Println(strings.HasSuffix("example.com", "com")) // 输出: true
}
```

**总结:**

*   `[]byte` 是字节切片，用于存储和操作字节序列。
*   `string` 类型在底层也是通过字节数组表示的，可以与 `[]byte` 相互转换。
*   `string` 类型提供了丰富的内置操作和 `strings` 包中的函数来处理字符串。

