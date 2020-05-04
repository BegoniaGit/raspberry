# Golang 基础

\*\*\*\*

**iota用作枚举会自动递增**

```go
const (
    Sunday = iota
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
  )
```

**获取运行参数**

```go
var (
        HOME = os.Getenv("HOME") 
    USER = os.Getenv("USER") 
    GOROOT = os.Getenv("GOROOT")
)
```

**初始化init函数**

每一个源文件都可以包含一个或多个 init 函数。初始化总是以单线程执行，并且按照包的依赖关系顺序执行。 一个可能的用途是在开始执行程序之前对数据进行检验或修复，以保证程序状态的正确性。

```go
func init() {
        Pi = 4 * math.Atan(1) // init() function computes Pi
    }
```

**字符串的高效拼接**

```text
#### 字符串 前缀 包含 索引 替换 次数 重复
```Go
strings.HasPrefix(str, "Th")
strings.Contains(s, substr string)
strings.LastIndex(s, str string) int
strings.Replace(str, old, new, n) string
strings.Count(s, str string) int
strings.Repeat(s, count int) string
```

**字符串类型转换**

```go
strconv.Itoa(i int) string
strconv.Atoi(s string) (i int, err error)
```

**时间对象**

获取当前时间

```go
time.Now()
```

时间格式转换

```go
fmt.Printf("%02d.%02d.%4d\n", t.Day(), t.Month(), t.Year())
```

或

```go
fmt.Println(t.Format("02 Jan 2006 15:04"))
```

[文档传送门](https://docs.studygolang.com/pkg/time/)

**switch 不需要特别使用 break**

**switch 的特别使用法，可代替 if else**

```go
switch a, b := x[i], y[j]; {
    case a < b: t = -1
    case a == b: t = 0
    case a > b: t = 1
}
```

**Go 中 for 的特别之处**

1. for 代替 where 的写法

   ```go
   fori>=0{
    i=i-1
    fmt.Printf("The variable i is now: %d\n", i) }
   ```

2. 无限循环

   ```go
   for{}
   ```

3. range 迭代

   ```go
   for pos, char := range str {
   }
   ```

   **defer 在 return 时执行相关操作**

4. 当有多个 defer 行为被注册时，它们会以逆序执行\(类似栈，即后进先出\)
5. 关键字 defer 允许我们进行一些函数执行完成后的收尾工作。

   **Go 的函数闭包我看作 Scala 中的高阶函数**

   当我们不希望给函数起名字的时候，可以使用匿名函数，例如: func\(x, y int\) int { return x + y } 。

   这样的一个函数不能够独立存在\(编译器会返回错误: non-declaration statement outside function body \)，但可以被 赋值于某个变量，即保存函数的地址到变量中: fplus := func\(x, y int\) int { return x + y } ，然后通过变量名对函 数进行调用: fplus\(3,4\) 。

   当然，您也可以直接对匿名函数进行调用: func\(x, y int\) int { return x + y } \(3, 4\) 。

**Go 中的切片**

1. 对于 切片 s 来说该不等式永远成立: 0 &lt;= len\(s\) &lt;= cap\(s\) 。
2. 多个切片如果表示同一个数组的片段，它们可以共享数据;因此一个切片和相关数组的其他切片是共享存储的，相 反，不同的数组总是代表不同的存储。数组实际上是切片的构建块。

   **Go 中的 map 操作**

3. 如果要排序，就使用结构体切片

   **Go 中的锁**

   变量中加上锁对象

   ```go
   import  "sync"
    type Info struct { 
    mu sync.Mutex
    // ... other fields, e.g.: Str string
   }
   ```

   使用对象

   ```go
   func Update(info *Info) {
        info.mu.Lock()
        // critical section:
        info.Str = // new value
        // end critical section
        info.mu.Unlock()
   }
   ```

   在 sync 包中还有一个 RWMutex 锁:他能通过 RLock\(\) 来允许同一时间多个线程对变量进行读操作，但是只能一个线程进行写操作。

   **包管理**

   将一个名为 codesite.ext/author/goExample/goex 的 map 安装在 $GOROOT/src/ 目录下。

   ```text
   go install codesite.ext/author/goExample/goex
   ```

   通过以下方式，一次性安装，并导入到你的代码中

   ```text
   import goex "codesite.ext/author/goExample/goex"
   ```

