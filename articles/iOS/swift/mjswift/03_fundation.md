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

## 属性

### 存储属性

* 类似于成员变量的概念
* 存储在实例的内存中
* 结构体和类可以定义存储属性
* 枚举不能定义存储属性（因为枚举变量的存储空间已经存储了关联值和枚举值）
* 存储属性必须进行初始化（默认值或者初始化器中）

#### 延迟存储属性

* 使用`lazy`可以定义一个延迟存储属性，在第一次用到属性的时候才会初始化
* `lazy`属性必须使用`var`，因为`let`属性必须在舒适化方法完成之前初始化
* `lazy`是线程不安全的，在多线程访问的情况下，无法保证只初始化一次
* 因为结构体是值类型，所以当接口体内部声明了一个`lazy`时，结构体变量必须为`var`,因为`lazy`属性会在使用到的时候才初始化，会改变结构体变量的内存，这在`let`的情况下是不允许的，所以结构体变量须为`var`

#### 属性观察器

* 可以为非`lazy`的存储属性设置属性观察器
* 在初始化器中设置属性值，不会触发属性观察器
* 在属性定义时赋值，不会触发属性观察器
* 属性观察器可以用在全局变量和局部变量上
* **父类的属性在它自己的初始化器中赋值不会触发属性观察器，但在子类的初始化器中赋值会触发属性观察器**

### 计算属性

* 本质就是方法
* 不占用实例的内存
* 结构体、枚举、类都可以定义计算属性
* 定义计算属性只能用`var`，不能用`let`
* 枚举`rawValue`的本质，就是只读计算属性
* 计算属性可以用在全局变量和局部变量上

### inout和属性观察器/计算属性

* 如果实参有**物理内存地址**，且没有设置属性观察器，则直接将实参的地址传入（引用传递）
* 如果实参是计算属性或者设置了属性观察器
  * 采取Copy In Copy Out
    * 调用该函数时，先复制实参的值到当前函数调用的栈空间
    * 将副本的地址传入到函数，在函数内部可以修改副本的值
    * 函数调用完成后，将该副本的值，设置到实参的值中
* 总结：inout的本质就是地址传递

### 类型属性

* 类型属性的存储属性，全局只有一份，类似于全局变量
* 可以通过`static`定义类型属性，如果是类，也可以用关键字`class`
* **必须给类型存储属性设置初始值**，因为类型没有实例那样的初始化方法
* 类型属性默认就是`lazy`，会在第一次使用的时候初始化
* 只会初始化一次，底层调用`swift_once`进行初始化操作，实际最终调用的就是`dispatch_oncen `
* 可以是`let`
* 枚举类型也可以定义类型属性（存储类型属性、计算类型属性）

#### 单例

```swift
public class FileManager {
  public static let shared = FileManager()
  private init() {}
}
```

## 方法

* 枚举、结构体、类**都可以定义类型方法和实例方法**
* 类型方法通过`class`或者`static`修饰

* 枚举和结构体为值类型，默认情况下，值类型不允许被自身实例方法修改，如果需要修改，使用`mutating`修饰方法

```swift
struct Point {
    var x: Int = 0
    var y: Int = 0
    mutating func move(delta: Int) {
        x += delta
        y += delta
    }
}
// 需要用var修饰
var p = Point()
p.move(delta: 3)
```

## 下标

* 使用`subscript`可以给任意类型（枚举、结构体、类）添加下标方法
* 本质是方法（函数），类似于计算属性
* 下标可以没有`set`,但是必须要有`get`，如果只有`get`，可以省略`get`
* 下标中的返回值类型也是`set`中的`newValue`类型
* 下标可以是类型方法
* 下标可以设置多个值
* 下标函数可以设置参数标签

```swift
struct Sum {
    var array = [
        [1,2,3],
        [4,5,6],
        [7,8,9]
    ]
    subscript(row: Int, column: Int) -> Int {
        set {
            guard row >= 0 && row <= 2 && column >= 0 && column <= 2 else {
                return
            }
            array[row][column] = newValue
        }
        get {
            guard row >= 0 && row <= 2 && column >= 0 && column <= 2 else {
                return -1
            }
            return array[row][column]
        }
    }
}
var sum = Sum()
print(sum[1,1])
sum[1,1] = 99
print(sum[1,1])
```

## 继承

* 只有类可以继承，值类型没有继承
* 子类可以重写父类的`方法`、`属性`、`下标`，重写必须加上`override`

* 类型方法也可以重写
  * 被`class`修饰的类型方法，可以被子类重写，`static`修饰的类型方法，不能被子类重写
* 重写属性
  * 子类可以将父类的存储属性、计算属性重写为计算属性
  * 子类不可以将父类的属性重写为存储属性
  * 只能重写`var`属性，不能重写`let`属性
  * 重写时，属性名、类型要一致
  * 子类重写后的属性权限，不能小于父类属性的权限
    * 如果父类属性是只读的，那么重写后的子类属性可以是只读的，也可以是可读写的
    * 如果父类属性是可读写的，那么重写后的子类属性，只能是可读写的
  * 如果将父类的存储属性重写为了计算属性，那么在子类的实例内存中，仍然有父类属性的存储空间。可以通过`super.属性名`访问这个属性
* 属性观察器
  * 可以在子类中为父类属性（出了只读计算属性、`let`属性）增加属性观察器

* `final`
  * 被`final`修饰的方法、下标、属性禁止被子类重写
  * 被`final`修饰的类，不能被继承

## 初始化

* 类、结构体、枚举都可以定义初始化器，因为值类型没有继承，相对比较简单，重点关注类的初始化器
* 类有两种初始化器：指定初始化器和便捷初始化器，类的设计原则就是要少量的指定初始化器
  * 每个类至少有一个指定初始化器，指定初始化器是类的主要初始化器，**指定初始化器的目标就是要完成类的初始化工作**
  * 指定初始化器**首先**完成自有属性的初始化工作，**然后**必须调用直接父类的指定初始化器，**然后**再完成一些自定义操作
  * 便捷初始化器必须**首先**横向调用其他初始化器（包括便捷初始化器和指定初始化器），最终调用链**必须**要调用到自身的一个指定初始化器。调用完其他的初始化器之后，**然后**再进行一些自定义的操作
  * 上述规则保证了安全性，确保了任何一个初始化器，都能安全完成实例的初始化工作

<img src="https://gitee.com/dexport/blog-image/raw/master/img/202203191635560.png" style="zoom:50%;" />

### 两段式初始化过程

* 初始化所有存储属性
* 自定义初始化过程

### 重写

* 当重写父类的指定初始化器时，必须加上`override`,且子类可以将父类的指定初始化器重写为便捷初始化器
* 因为子类不能直接调用父类的便捷初始化器，所以不存在重写父类便捷初始化器的操作，因为子类可以定义和父类便捷初始化器一样名称的初始化器，且不用加`override`

### 自动继承

* 如果子类没有自定义任何指定初始化器，那么它将自动继承父类的所有指定初始化器
* 如果子类提供了父类所有指定初始化器的实现（要么自动继承，要么重写，重写包含重写为便捷初始化器），那么子类将自动继承父类所有的便捷初始化器

### required

* 用`required`修饰指定初始化器，表明其所有子类都必须实现该初始化器(通过继承或者重写实现)，可以重写为便捷初始化器。
* 如果子类重写了required初始化器，也必须加上required，不用加override

```swift
class Person {
    required init(age: Int) {}
}

class Student : Person {
//    init() {
//        super.init(age: 0)
//    }
// 可以重写为便捷初始化器
//    required convenience init(age: Int) {
//        print("1")
//        self.init()
//        print("2")
//    }
    required init(age: Int) {
        super.init(age: age)
    }
}
```

* 使用元类型调用初始化方法，必须是`required`的，因为必须要确保有此方法

```swift
class Animal {
    // 确保子类必有这个初始化器
    required init() {}
}
class Dog: Animal {}
class Cat: Animal {}
class Pig: Animal {}

func create(animals: [Animal.Type]) -> [Animal] {
    var arr: [Animal] = []
    for cls in animals {
        arr.append(cls.init())
    }
    return arr
}
create(animals: [Dog.self, Pig.self, Cat.self])
```



### 可失败初始化器

* 类、结构体、枚举都可以使用`init?`定义可失败初始化器

```swift
class Person {
    var name: String
    init?(name: String) {
        if name.isEmpty {
            return nil
        }
        self.name = name
    }
}
var p = Person(name: "") // Person?
```

* 不允许同时定义参数标签、参数个数、参数类型相同的可失败初始化器和非可失败初始化器
* 可以用`init!`定义隐式解包的可失败初始化器
* 可失败初始化器可以调用非可失败初始化器，非可失败初始化器调用可失败初始化器需要进行解包
* 如果初始化器调用一个可失败初始化器导致初始化失败，那么整个初始化过程都失败，并且之后的代码都停止执行
* 可以用一个非可失败初始化器重写一个可失败初始化器，但反过来是不行的

## 反初始化（deinit）

* `deinit`不接受任何参数，不能写小括号，不能自行调用
* 父类的`deinit`能被子类继承
* 子类的`deinit`实现执行完毕后会调用父类的`deinit`，无需手动调用

```swift
class Person {
    deinit {
        print("Person deinit")
    }
}
class Student: Person {
    deinit {
        print("Student deinit")
    }
}
func test() {
    var _ = Student()
}
test()
// Student deinit
// Person deinit
```

## 可选链

* 如果可选项为`nil`，调用方法、下标、属性失败，结果为`nil`
* 如果可选项不为`nil`，调用方法、下标、属性成功，结果会被包装成可选项
  * 如果结果本来就是可选项，不会进行再次包装
* 多个?可以链接在一起
  * 如果链中任何一个节点是`nil`，那么整个链就会调用失败，且`nil`后的代码不会再执行
* 无返回值的函数，其实也是有返回值的，是一个可选空元祖`()`

```swif
if let _ = person?.eat() { // ()?
	print("eat调用成功")
} else {
	print("eat调用失败")
}
```

* 可选链的应用

```swift
var scores = ["Jack": [86, 82, 84], "Rose": [79, 94, 81]]
scores["Jack"]?[0] = 100
scores["Rose"]?[2] += 10
scores["Kate"]?[0] = 88

var dict: [String : (Int, Int) -> Int] = [
"sum" : (+),
"difference" : (-)
]
var result = dict["sum"]?(10, 20) // Optional(30), Int?
```

* 可以在可选变量赋值的时候添加`?`，表明如果可选变量如果是`nil`，就不进行赋值运算

```swift
var num1: Int? = 5
num1? = 10 // Optional(10)
var num2: Int? = nil 
// num2为nil，所有后续的赋值操作不进行调用
num2? = 10 // nil
```

## 协议

* 协议可以用来定义方法、属性、下标的声明，协议可以被枚举、结构体、类遵守（多个协议之间用逗号隔开）
  * 协议中定义方法时不能有默认参数值
  * 协议中定义属性时必须用`var`关键字，因为可以是计算属性
  * 实现协议时的属性权限要不小于协议中定义的属性权限，即协议规定了至少可以做什么
    * 协议定义`get`、`set`，用`var`存储属性或`get`、`set`计算属性去实现
    * 协议定义`get`，用任何属性都可以实现
  * 为了保证通用，协议中必须用`static`定义类型方法、类型属性、类型下标，不能使用`class`，因为值类型也可以遵守协议
    * 类实现`static`定义的类型方法、类型属性、类型下标时，可以使用`class`

```swift
// 协议定义
protocol Person {
    func run()
    var name: String { set get } // 可以为存储属性、也可以为计算属性
    var height: Int { get } // 可以为存储属性（let）、也可以为计算属性
    subscript(index: Int) -> Int { set get }
}
class Student: Person {
    var name: String
    let height: Int
    init(name: String, height: Int) {
        self.name = name
        self.height = height
    }
    func run() {
        print("\(name) run")
    }
    subscript(index: Int) -> Int {
        set {
            
        }
        get {
            index
        }
    }
}
var stu = Student(name: "lisi", height: 178)
stu.run()
// 实现多个协议
protocol Test1 {}
protocol Test2 {}
protocol Test3 {}
class TestClass : Test1, Test2, Test3 {}
```

* 只有将协议中的实例方法标记为`mutating`，才可以允许值类型方法修改自身，类类型可以无视这个关键字，不用添加

* 协议中还可以定义初始化器`init`
  * 非`final`类实现时必须加上`required`

```swift
protocol Animal {
    init()
}
class Dog: Animal {
    required init() {
        
    }
}
final class Cat: Animal {
    init() {
        
    }
}
```

* 如果从协议实现的初始化器，刚好是重写了父类的指定初始化器，那么这个初始化必须同时加`required`、`override`

```swift
protocol Animal {
    init()
}
class Dog {
    init() {
        
    }
}
class ErHa: Dog, Animal {
    required override init() {
        
    }
}
```

* 协议中定义的`init?`、`init!`，可以用`init`、`init?`、`init!`去实现
* 协议中定义的`init`，可以用`init`、`init!`去实现

```swift
protocol Livable {
	init()
	init?(age: Int)
	init!(no: Int)
}
class Person : Livable {
	required init() {}
	// required init!() {}
	required init?(age: Int) {}
	// required init!(age: Int) {}
	// required init(age: Int) {}
	required init!(no: Int) {}
	// required init?(no: Int) {}
	// required init(no: Int) {}
}
```

* 协议可以继承

### 协议组合

* 协议组合，可以包含1个类类型**（最多1个）**

```swift
protocol Livable {}
protocol Runnable {}
class Person {}

// 接收Person或者其子类的实例
func fn0(obj: Person) {}
// 接收遵守Livable协议的实例
func fn1(obj: Livable) {}
// 接收同时遵守Livable、Runnable协议的实例
func fn2(obj: Livable & Runnable) {}
// 接收同时遵守Livable、Runnable协议、并且是Person或者其子类的实例
func fn3(obj: Person & Livable & Runnable) {}

typealias RealPerson = Person & Livable & Runnable
// 接收同时遵守Livable、Runnable协议、并且是Person或者其子类的实例
func fn4(obj: RealPerson) {}
```

### CaseIterable 协议

* 让枚举遵守`CaseIterable`协议，可以实现遍历枚举值

```swift
enum Season: CaseIterable {
    case Spring, Summer, Autum, Winter
}
for season in Season.allCases {
    print(season)
}
```

### CustomStringConvertible、CustomDebugStringConvertible 协议

* 遵守`CustomStringConvertible`、 `CustomDebugStringConvertible`协议，都可以自定义实例的打印字符串
  * `print`调用的是`CustomStringConvertible`协议的`description`
  * `debugPrint`、`po`调用的是`CustomDebugStringConvertible`协议的`debugDescription`

```swift
class Person: CustomStringConvertible, CustomDebugStringConvertible {
    var name: String
    var age: Int
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    var description: String {
        return "Person who name is \(name), age is \(age)"
    }
    var debugDescription: String {
        return "Debug that person name = \(name), age = \(age)"
    }
}
var p = Person(name: "lisi", age: 19)
print(p)
```

## Any / AnyObject

* Swift提供了2种特殊的类型：Any、AnyObject
  * Any：可以代表任意类型（枚举、结构体、类，也包括函数类型）
  * AnyObject：可以代表任意类类型（在协议后面写上: AnyObject代表只有类能遵守这个协议,在协议后面写上: class也代表只有类能遵守这个协议）

## is、as?、as!、as

* is用来判断是否为某种类型，as用来做强制类型转换

## X.self、X.Type、AnyClass

* X.self是一个元类型（metadata）的指针，metadata存放着类型相关信息
* X.self属于X.Type类型
* `public typealias AnyClass = AnyObject.Type`
* type(of: ) 返回元类型

```swift
class Person {}
class Student : Person {}
var perType: Person.Type = Person.self
var stuType: Student.Type = Student.self
perType = Student.self

var anyType: AnyObject.Type = Person.self
anyType = Student.self
var anyType2: AnyClass = Person.self
anyType2 = Student.self

var per = Person()
var perType = type(of: per) // Person.self
print(Person.self == type(of: per)) // true
```

* Swift还有个隐藏的基类：`Swift._SwiftObject`

```swift
class Person {
	var age: Int = 0
}
class Student : Person {
	var no: Int = 0
}
print(class_getInstanceSize(Student.self)) // 32
print(class_getSuperclass(Student.self)!) // Person
print(class_getSuperclass(Person.self)!) // Swift._SwiftObject
```

## Self

* Self代表当前类型

```swift
class Person {
    var name: String = "lisi"
    static let category: String = "Human"
    func test() {
        print(self.name)
        print(Self.category)
    }
}
var p = Person()
p.test()
```

* Self一般用作返回值类型，限定返回值跟方法调用者必须是同一类型（也可以作为参数类型）

```swift
protocol Runnable {
	func test() -> Self
}
class Person : Runnable {
	required init() {}
	func test() -> Self { type(of: self).init() }
}
class Student : Person {}

var p = Person()
// Person
print(p.test())
var stu = Student()
// Student
print(stu.test())
```

## 错误处理

* Swift中，可以通过实现`Error`协议，自定义运行时错误

```swift
enum SomeError : Error {
    case illegalArg(String)
    case outOfBounds(Int, Int)
    case outOfMemory
}
```

* 函数内部通过`throw`抛出错误，可能抛出错误的函数必须加上`throws`声明

```swift
func devide(_ num1: Int, _ num2: Int) throws -> Int {
    if num2 == 0 {
        throw SomeError.illegalArg("0 不能作为除数")
    }
    return num1 / num2
}
```

* 可能抛出错误的函数，需要使用`try`调用

```swift
var ret = try devide(10, 2)
```

* 可以使用`do-catch`捕捉错误

```swift
func test() {
    do {
        print("start")
        try devide(10, 0)
        print("end")
    } catch let SomeError.illegalArg(msg) {
        print(msg)
    } catch let SomeError.outOfBounds(size, index) {
        print(size, index)
    } catch {
        // 默认参数 error, 也可以使用匹配接收
        print("其他错误：\(error)")
    }
}
test()
// start
// 0 不能作为除数
```

* 抛出`Error`后，`try`开始到作用域结束的代码都不会执行了

* 可以使用`try?`,`try!`调用可能会抛出`Error`的函数，这样就不用处理`Error`了
  * `try?`返回一个可选值，当出现`Error`时，返回nil
  * 当确认不会出现错误时，可以使用`try!`隐式解包

* `rethrows`表明函数本身不会抛出错误，是内部调用量闭包参数抛出错误，那么它将重新向上抛出错误。作用等价于`throws`，用于语法表明其实是内部调用闭包参数发生的错误，可以使用`rethrows`替换

```swift
func test(fn: (Int, Int) throws -> Int, num1: Int, num2: Int) rethrows -> Int {
    return try fn(num1, num2)
}
try test(fn: devide, num1: 10, num2: 2)
```

## defer

* `defer`定义一个离开函数时执行的代码块。可以定义多个，执行顺序和定义顺序相反
* 必须定义在离开函数之前，假如函数提前返回，那么定义在提前返回之后的`defer`不会执行

```swift
func test(num: Int) {
    defer {
        print("1 do something")
    }
    print("start")
    if num == 0 {
        return
    }
    defer {
        print("2 do something")
    }
    print("end")
}
// return 之后的defer不会执行
test(num: 0)

// try 后面的defer不会调用
func test(num: Int) throws{
    defer {
        print("1 do something")
    }
    print("start")
    try devide(10, num)
    defer {
        print("2 do something")
    }
    print("end")
}
func test() {
    do {
        try test(num: 0)
    } catch {
        print(error)
    }
}
test()
```

## `fatalError`

* 可以通过`fatalError`抛出错误，不用`throw / throws`，不能通过`do-catch`捕获这个错误
* 那么不得不实现，但是又不想别人调用的代码，可以通过`fatalError`抛出错误

```swift
class Person {
    required init() {}
}
class Student: Person {
    required init() {
        fatalError()
    }
    init(name: String) {
    }
}
var stu1 = Student(name: "lisi")
var stu2 = Student() // Fatal error
```

## do{}

* 可以使用`do`定义局部作用域

```swift
func test() {
    do {
        let a = 0
        print(a)
    }
    print("do something")
}
```

## 泛型

* 泛型可以将类型参数化，提高代码复用性，减少代码量

```swift
func swapValue<T>(_ num1:inout T, _ num2:inout T) {
    (num1, num2) = (num2, num1)
}
var a = 1
var b = 2
// 这里可以唯一确定泛型T为Int，所以不用传入。使用swapValue<Int>(&a, &b)也可以
swapValue(&a, &b)
print(a, b)
```

* 只要可以确定泛型的值，泛型函数就可以调用成功

```swi
var n1 = 10
var n2 = 20
// 这里可以赋值成功的原因就是因为编译器可以推断出swapValue中的T为Int
var fn: (_ num1: inout Int, _ num2: inout Int) -> () = swapValue
fn(&n1, &n2)
print(n1, n2)
```

* 泛型可以修饰协议、类、结构体、枚举

```swift
struct Point<T> {
    var x: T
    var y: T
}
var p1: Point<Double> = Point(x: 10, y: 20)
var p2 = Point(x: "2", y: "1")
print(p1.x)
print(p2.x)
```

## 关联类型（Associated Type）

* 作用：给协议中用到的类型定义一个占位名称
* 协议中可以定义多个关联类型

```swift
protocol Stackable {
    associatedtype Element
    mutating func push(_ element: Element)
    mutating func pop() -> Element?
    func top() -> Element?
    func size() -> Int
}

struct Stack<T> : Stackable {
    var arr: Array<T> = []
    typealias Element = T
    func size() -> Int {
        arr.count
    }
    mutating func push(_ element: T) {
        arr.append(element)
    }
    mutating func pop() -> T? {
        guard size() > 0 else {
            return nil
        }
        return arr.removeLast()
    }
    func top() -> T? {
        arr.first
    }
}

var s = Stack<String>()
if let ele = s.pop() {
    print(ele)
} else {
    print("s is empty")
}
```

## 泛型类型约束

* 泛型函数类型约束

```swift
protocol Runnable {}
class Person {}

func swapValue<T: Runnable & Person>(_ value1: inout T, _ value2: inout T) {
    (value1, value2) = (value2, value1)
}

class Student: Person, Runnable {}

var stu1 = Student()
var stu2 = Student()

swapValue(&stu1, &stu2)
```

* 关联类型约束

```swift
protocol Stackable {
    associatedtype Element: Equatable
}
struct Stack<T: Equatable> : Stackable {
    typealias Element = T
}
var s = Stack<Int>()
```

* 泛型函数通过`where`语句约束

```swift
protocol Stackable {
    associatedtype Element
}
struct Stack<T> : Stackable {
    typealias Element = T
}
func calcValue<T1: Stackable, T2: Stackable>(_ value1: T1, _ value2: T2) -> Bool where T1.Element == T2.Element, T2.Element : Hashable {
    return true
}
var s1 = Stack<Int>()
var s2 = Stack<Int>()
var s3 = Stack<String>()
calcValue(s1, s2)
// error: global function 'calcValue' requires the types 'Stack<Int>.Element' (aka 'Int')
// and 'Stack<String>.Element' (aka 'String') be equivalent
calcValue(s1, s3)
```

* 协议作为返回类型

```swift
protocol Runnable {}

class Dog: Runnable {}
class Cat: Runnable {}

func get(_ type: Int) -> Runnable {
    if type == 0 {
        return Dog()
    }
    return Cat()
}
// a: Runnable
var a = get(1)
// b: Runnable
var b = get(0)
```

* 当协议作为返回值，但是协议中有`Self`或者关联类型时，需要注意要唯一确认`Self`或者关联类型的类型

```swift
protocol Runnable {
    associatedtype SpeedType
    var speed: SpeedType { get }
}
class Dog : Runnable {
    typealias SpeedType = Double
    var speed: Double {
        10.1
    }
}
class Cat : Runnable {
    typealias SpeedType = Int
    var speed: Int {
        21
    }
}
// error: protocol 'Runnable' can only be used as a generic constraint
// because it has Self or associated type requirements
// 当协议中有 Self 或者 关联类型时，编译器不能确定其中的Self或者关联类型 的类型，所以会报错
// 比如这里无法确定返回值中的 SpeedType 究竟是 Double 还是 Int
func get(_ type: Int) -> Runnable {
  // 这里不管返回一个还是两个都会报错，例如注释下面这个if语句也会报错
    if type == 0 {
        return Dog()
    }
    return Cat()
}
```

* 解决方案一：使用泛型

```swift
// 解决，使用泛型确认，当调用get是，需要传入泛型T，即可唯一确认返回值T
func get<T: Runnable>(_ type: Int) -> T {
    if type == 0 {
        return Dog() as! T
    }
    return Cat() as! T
}
var dog: Dog = get(0)
var cat: Cat = get(1)
```

* 解决方案二：使用`some`关键字声明一个不透明类型
  * `some`限制只能返回一种类型，如果返回两种类型还是会报错
  * `some`用于隐藏内部实现细节
  * `some`除了用在返回值上，还可以用在类型属性上

```swift
func get(_ type: Int) -> some Runnable {
    return Cat()
}

// 属性
class Person {
    var pet: some Runnable {
        Dog()
    }
}
```

## 可选项的本质

* 可选项的本质是枚举

```swift
var a: Int?
a = 10
a = .none
a = .some(11)
a = nil
```

