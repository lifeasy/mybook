# Swift基础

## 科学计数法

```swift
let a = 1.25e2
let b = 0xFp2

```

## 元组

```swift
let httpError = (404, "Not Fund")
print(httpError.0, httpError.1)
let (status, msg) = httpError
print(status, msg)
let person: (age: Int, name: String) = (18, "lisi")
print(person.age, person.name)
let school = (level: "Primary", address: "ShenZhen")
print(school.level, school.address)
```

## 区间运算符

```swift
let range = 1...3
for i in range {
    print(i)
}
for var i in range {
    i += 2
    print(i)
}
let a = 3
for i in a..<5 {
    print(i)
}
```

### 区间运算符作用在数组上

```swift
let names = ["lisi", "zhangsan", "wangwu", "Alex", "John"]
// ArraySlice类型
let subNames: ArraySlice = names[1...3]
print(subNames)
```

### 单侧区间

```swift
// 单侧区间
for name in names[...3] {
    print(name)
}
for name in names[3...] {
    print(name)
}

// var range1: PartialRangeFrom<Int>
var range1 = 3...
var c4 = range1.contains(4)

// var range2: PartialRangeThrough<Int>
var range2 = ...3
c4 = range2.contains(4)
```

* 字符和字符串也可以用在区间字符上，但是不能使用在forin循环中

```swift
// ClosedRange<String>
let range1 = "aa"..."cc"
print(range1.contains("ac"))
let range2 = "a"..."d"
print(range2.contains("f"))
```

* 从`\0`到`~`包含了所有可能用到的ASCII字符

```swift
let charRange = "\0"..."~"
print(charRange.contains("G"))
```

* 间隔区间

```swift
//StrideTo<Int>
let strideToRange = stride(from: 0, to: 10, by: 2)
for i in strideToRange {
    print(i)
}
//StrideThrough<Int>
let strideThrough = stride(from: 0, through: 10, by: 2)
for i in strideThrough {
    print(i)
}
```

## 标签语句

```swift
outer: for i in 0...7 {
    for j in 0...7 {
        if i == 3 {
            continue outer
        }
        if j == 3 {
            break outer
        }
        print("i = \(i), j = \(j)")
    }
}
```

## 函数

* 函数的注释文档

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202203061507076.png" style="zoom:50%;" />

```swift
/// 求和【概述】
///
/// 将两个数相加，并返回和【详细描述】
///
/// - Parameters:
///   - arg1: 加数
///   - arg2: 被加数
/// - Returns: 返回和
///
/// - Note: 传入两个整数即可【批注】
func sum(arg1: Int, arg2: Int) -> Int {
    return arg1 + arg2
}
```

### 可变参数

* 一个函数最多只能有一个可变参数
* 紧跟在可变参数后的参数，不能省略参数标签

```swift
func sum(_ args: Int...) -> Int {
    var total = 0
    for i in args {
        total += i
    }
    return total
}

print(sum(1,2,3,4,5))
```

### inout参数

* 本质是
* 地址传递

```swift
var a = 10
var b = 20
func swapValue(_ num1: inout Int, _ num2: inout Int) {
    let tmp = num1
    num1 = num2
    num2 = tmp
}
swapValue(&a, &b)
print(a, b)
```

### 函数重载

* 函数名相同
* 参数个数不同 || 参数类型不同 || 参数标签不同
* 返回值类型与函数重载无关

## 查看类型占用空间大小

```swift
MemoryLayout<Int>.size // 实际用到的空间 size <= stride
MemoryLayout<Int>.stride // 分配的空间
MemoryLayout<Int>.alignment // 对齐字节
```

## 枚举

### 关联值

> 可以外部设置的，保存在变量空间内部的

```swift
enum Date {
    case digit(month: Int, day: Int, year: Int)
    case string(String)
}
var date = Date.digit(month: 3, day: 11, year: 2021)
switch date {
  // 值绑定，可以使用let、var等修饰，可以将let放到具体的关联值前修饰
case .digit(let month, let day, let year):
    print("\(month)-\(day)-\(year)")
  // 也可以将let放到最前面修饰，则修饰的是所有的关联值
case let .string(str):
    print(str)
}
```

### 原始值

> 使用默认值预先设置，和枚举项绑定，不能修改，不会存储在枚举变量的空间中
>
> 如果枚举的原始值是Int、String类型，如果不设置，会自动分配原始值。Int会从0开始自增，或者后面一个在前面一个的基础上+1，如果是String，不设置的话，等于枚举值得字符串

```swift
enum Season: Int {
    case Spring, Summer, Autumn, Winter
}
print(Season.Autumn.rawValue)

enum Direction: String {
    case north
    case south
    case west
    case easy
}
print(Direction.west.rawValue)
```

### 枚举的汇编本质

* 原始值不占用枚举变量的内存空间
* 关联值占用枚举变量的内存空间,关联值存储在前面，枚举值存储在后面

## 可选项

### 空合运算符??

a ?? b

* a是可选项
* b是可选项或者不是可选项
* b和a的存储类型必须一致
* 如果a不为nil，就返回a
* 如果a为nil，就返回b
* 如果b不是可选项，返回a时会自动解包

### 隐式解包

* 某些情况下，可选项一旦被设值，就会一直有值，就可以声明为隐式解包类型`let a: Int! = 10`
* 后续使用可以直接使用，不用解包

## 结构体

* 所有结构体都有一个编译器默认生成的初始化器

```swift
struct Person {
    var name: String
    var age: Int
}
// 默认初始化器
let p = Person(name: "lisi", age: 18)
```

* 编译器会根据成员变量是否已经赋值，生成其他的初始化器。宗旨是：**保证所有的成员都有初始值**

```swift
struct Person {
    var name: String = "default"
    var age: Int = 0
}

var p1 = Person(name: "lisi", age: 20)
// 编译器根据变量值生成的其他的初始化器
var p2 = Person()
```

* 一旦结构体自定义了初始化器，编译器不会在生成初始化器

* 结构体是值类型，传递属于深拷贝，不会影响到之前的变量

## 类

* 类和结构体类似，但是编译器没有为类生成可以传入参数的初始化器
* 如果所有的成员在定义的时候有一个初始化值，则编译器会默认生成一个无参的初始化器（成员的初始化都是在这个初始化器中完成的）

```swift
class Point {
    var x: Int = 0
    var y: Int = 0
}
let point = Point()
// 这两种等价，生成的汇编代码是一样的
class Point {
    var x: Int
    var y: Int
    init() {
        x = 0
        y = 0
    }
}
let point = Point()
```

* 类是引用类型
* 方法不占用对象或者结构体内存，方法存储在代码段

## 闭包表达式

### 定义

可以通过func定义一个函数，也可以使用闭包表达式定义一个函数

> ```swift
> {
>   (参数列表) -> 返回值类型 in
>   	函数体
> }
> ```

```swift
// 闭包表达式
var fn = { (a: Int, b: Int) -> Int in
    return a + b
}
fn(10, 20)
// 匿名闭包表达式
var sum = { (a: Int, b: Int) -> Int in
    return a + b
}(11, 22)
```

### 尾随闭包

```swift
// 尾随闭包
func exec(_ a: Int, _ b: Int, fn:(Int, Int) -> Int) -> Int {
    return fn(a, b)
}
let ret = exec(10, 20) {$0 + $1}
```

* 如果闭包表达式是函数唯一实参，而且使用了尾随闭包的语法，那么就不用再函数后面写()

```swift
func exec(fn:(Int, Int) -> Int) -> Int {
    return fn(1, 2)
}
// 可以省略()
var ret = exec {$0 + $1}
```

## 闭包

一个函数，和它所捕获的变量/常量环境组合起来，称之为闭包

* 可以把闭包想象成一个类的实例对象
  * 内存在堆空间
  * 捕获的变量/常量就是对象的成员（存储属性）
  * 组成闭包的函数，就是类内部定义的方法
  * 如果返回值是函数类型，注意函数参数的类型要一致，比如都是inout类型的参数
* 关于汇编查看闭包底层原理总结
  * 捕获了局部变量的闭包，实际上是调用swiftAlloc，在堆空间生成了一个实例对象，前面16个字节，存储了类的指针、引用计数、从16个字节开始，存储捕获的变量的值。后续访问变量，实际上是访问这个地址空间里的内容
  * 一个函数类型的变量（闭包），栈空间实际占用了16个字节，前面8个字节为函数地址，后面8个字节一般为0，如果是捕获了局部变量的闭包，后8个字节存储了一个地址，通过这个地址可以访问到堆上的闭包对象，从而访问到捕获的变量
  * 在一个函数的内部，存在两个闭包函数，同时捕获了相同的局部变量，则被捕获的局部变量，只生成一份堆对象，两个闭包公用，会同时修改
  * 如果被捕获了两个局部变量，实际上会生成两个堆对象，每个里面存储了一个被捕获的值

### 自动闭包

* 一个语法糖，方便调用。允许传入一个指定类型的值，并将它自动转为闭包，比如自动将`20`封装成`{ 20 }`
* 自动闭包只有使用到了才会执行，所以，在有`@autoclosure`的参数，建议加上注释标明这个值会延迟执行或者不执行
* `@autoclosure`只支持`() -> T`格式的参数
* 空合运算符`??`使用了自动闭包技术
* 有`@autoclosure`和无`@autoclosure`可以构成函数重载

```swift
// 自动闭包
func getFirstPositive(a: Int, b: @autoclosure () -> Int) -> Int {
    return a > 0 ? a : b()
}
print(getFirstPositive(a: 10, b: 20))
```

