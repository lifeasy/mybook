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

