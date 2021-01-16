# 字符串的常见操作

## String与基本数据类型转换

* 基本数据类型转String 直接使用String的初始化方法进行转换

```swift
print(String("a")) // a
        print(String(1)) // 1
        print(String(1.5)) // 1.5
        print(String(false)) // false
```

* String转基本数据类型

```swift
print(Character("a"))
print(Int("1") ?? -1)
print(Double("1.34") ?? 0.0)
print(Bool("false") ?? -1)
```

* String与ASCII的相互转化 比如 97->a、a->97

  * 字符串转ASCII数组 unicodeScalars

  ```swift
  let str = "abcdef"
  let arr = str.unicodeScalars
  for i in arr {
    print(i.value) //97 98 99 100 101 102
  }
  ```

  * 单个字符与ASCII互转

  ```swift
  let a : Character = "a"
  print(a.unicodeScalars.first?.value ?? 0)
  print(a.asciiValue ?? 0)
  let b : Character = Character(UnicodeScalar(97))
  print(b)
  ```

## String和数组

### 数组转Stirng

* 如果是字符串数组，可使用数组的join方法

```swift
let arr1 : [String] = ["1","23","45"]
let str1 = arr1.joined()
print(str1)
```

* 如果是字符数组，可以使用字符串的字符数组初始化方法

```swift
let arr2 : [Character] = ["1","2","5"]
let str2 = String(arr2)
print(str2)
```

* 如果是其他类型 比如 Int 或者 Double

```swift
let arr3 = [1,3,4]
let str3 = arr3.reduce(""){
  $0 + String($1)
}
```

### 字符串转字符数组 [Character]

```swift
let str = "abcde"
let arr = Array(str)
print(arr)
```

## String 与 集合 

> 可以用来过滤String中的重复字符

### 集合转String 

> 类似数组操作 注意无序

```swift
let set : Set<Character> = ["a","b","c","a","c"]
let str = String(set)
print(str)

let set1 : Set<String> = ["1","2","3","哈哈","哈哈"]
let str1 = set1.joined()
print(str1)
```

### String 转集合

> 可以去除重复字符

```swift
let str = "112233哈哈"
let set = Set(str)
print(set)
```

## Character的操作

```swift
let c : Character = "c"
print(c.uppercased())
print(c.isUppercase)
print(c.isLowercase)
print(c.isLetter)
print(c.isNumber)
print(c.asciiValue ?? 0)
```

## 字符串的操作

### 字符串格式化

```swift
let str = String(format: "%d个哈哈", 15)
print(str)
```

### 访问字符串中的字符

```swift
var str1 = "abcdefg"
print(str1[str1.startIndex])
print(str1[str1.index(before: str1.endIndex)])
```

### 字符串删除

```swift
str1.removeFirst()
print(str1)
str1.remove(at: str1.index(str1.startIndex, offsetBy: 1))
print(str1)
str1.removeLast()
print(str1)
str1.removeFirst(2)
print(str1)
str1.removeLast(2)
print(str1)
str1 = "Hello World"
str1.removeSubrange(str1.index(after: str1.startIndex)...str1.index(str1.startIndex, offsetBy: 3))
print(str1)
if let range = str1.range(of: "orl") {
  str1.removeSubrange(range)
}
print(str1)
str1.removeAll()
print(str1)
```

### 字符串比较

```swift
let str1 = "bbc"
let str2 = "adbc"
let compare = str1.compare(str2)
print(compare.rawValue)
#endif
```

### 字符串包含

```swift
let str1 = "Hello"
let str2 = "Hello World"
if str2.contains(str1) {
  print("contains")
}
```

### 字符串分割

```swift
let str1 = "Hello World"
let ret = str1.split(separator: " ")
print(ret)
```

### 字符串截取子串

```swift
let str1 = "Hello World"
// 截取头部
print(str1.prefix(2))
// 截取尾部
print(str1.suffix(2))
// 截取指定index
print(str1[str1.index(str1.startIndex, offsetBy: 2)...str1.index(str1.startIndex, offsetBy: 5)])
// 指定range
let range = str1.range(of: "llo W")
if let r = range {
  print(str1[r.lowerBound..<r.upperBound])
}
```

### 字符串替换

```swift
let str1 = "Hello World"
let str2 = str1.replacingOccurrences(of:"He",with:"St")
print(str2)
```

### 字符串插入

```swift
var str1 = "Hello World"
str1.insert("A", at: str1.startIndex)
print(str1)
str1.insert(contentsOf:"666",at: str1.endIndex)
print(str1)
```





