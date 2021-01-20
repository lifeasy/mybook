# 数组

## 初始化

```swift
let arr : [Int] = [1,2,3]
let arr1 : [Any] = [1,"2"]
let arr3 = Array(repeating: -1, count: 10)
let arr4 = [Int]()
let arr5 : Array<Int> = []
```

## 访问指定位置数据

```swift
let arr = [1,3,4,5,6,7]
print(arr[1])//下标访问
print(arr.first!)//可选值
print(arr.last!)//可选值
```

## 遍历

```swift
let arr = [1,3,4,5,6,7]
// 不需要index
for value in arr {
  print(value)
}
// 需要index
for i in 0..<arr.count {
  print(arr[i])
}
// 同时需要
for (index,value) in arr.enumerated() {
  print("(\(index),\(value))")
}
// forEach
arr.forEach{print($0+10)}
```

## 添加

```swift
var arr = [1,3,4]
arr.append(4)
print(arr) // [1, 3, 4, 4]
arr.append(contentsOf: [4,5,6])
print(arr) // [1, 3, 4, 4, 4, 5, 6]
let ret = stride(from: 10, to: 20, by: 2).map{$0}
print(ret) // [10, 12, 14, 16, 18]
let ret2 = stride(from: 10, through: 20, by: 2).map{$0}
print(ret2) // [10, 12, 14, 16, 18, 20]
arr.append(contentsOf: stride(from: 10, to: 20, by: 3))
print(arr) //[1, 3, 4, 4, 4, 5, 6, 10, 13, 16, 19]
```

## 插入

```swif
var arr = [1,3,4]
arr.insert(7, at: 1)
print(arr) // [1, 7, 3, 4]
arr.insert(contentsOf: [2,3,4], at: 1)
print(arr) // [1, 2, 3, 4, 7, 3, 4]
```

## 删除

```swift
var arr = [1, 2, 3, 4, 7, 3, 4, 1, 2, 3, 4, 7]
arr.removeFirst()
print(arr)//[2, 3, 4, 7, 3, 4, 1, 2, 3, 4, 7]
arr.removeLast()
print(arr)//[2, 3, 4, 7, 3, 4, 1, 2, 3, 4]
arr.removeFirst(2)
print(arr)//[4, 7, 3, 4, 1, 2, 3, 4]
arr.removeLast(3)
print(arr)//[4, 7, 3, 4, 1]
arr.remove(at: 2)
print(arr)//[4, 7, 4, 1]
arr.removeSubrange(0...2)
print(arr)//[1]
arr.removeAll()
print(arr)//[]
let arr1 = [1, 2, 3, 4, 7, 3]
print(arr1.drop{$0<4})// drop掉第一个给定表达式的值，返回后面的子序列 [4, 7, 3]
print(arr1.dropFirst())//[2, 3, 4, 7, 3]
print(arr1.dropFirst(2))//[3, 4, 7, 3]
print(arr1.dropLast())//[1, 2, 3, 4, 7]
print(arr1.dropLast(2))//[1, 2, 3, 4]
```

## 运算符操作

```swift
let arr = [1, 2, 3, 4, 7, 3]
var ret = arr + [5]
print(ret)//[1, 2, 3, 4, 7, 3, 5]
ret = [6] + arr
print(ret)//[6, 1, 2, 3, 4, 7, 3] //reduce可以利用这个操作进行魔法
```

## 属性&常用操作

```swift
let arr = [1, 2, 3, 4, 7, 3]
print(arr.count) // 6
print(arr.isEmpty) // false
print(arr.capacity) // 6
print(arr.min()!) // 1
print(arr.max()!) // 7
print(arr.contains(3))//true
print(arr.lastIndex(of: 2)!)//1
print(arr.firstIndex(of: 4)!)//3

```

## 高阶函数

### Map

> **对集合进行循环，并对集合中的每个元素采取相同的操作**。

```swift
//Map
let arr = [1,3,4,5,6,7]
var newArr = arr.map({(value:Int)->Int in
                      return value + 1
                     })
print(newArr)//[2, 4, 5, 6, 7, 8]
newArr = arr.map{$0+1}//尾随闭包
print(newArr)//[2, 4, 5, 6, 7, 8]
```

### compactMap

> 执行转换，解包所有可选选项并丢弃nil值

```swift
let arr = ["1","2","3","a","b","6"]
var newArr = arr.map{Int($0)}
print(newArr) // [Optional(1), Optional(2), Optional(3), nil, nil, Optional(6)]
let newArr2 = arr.compactMap{Int($0)}
print(newArr2) // [1, 2, 3, 6]

```

### flatMap

> 对于集合的集合，进行降纬处理
>
> flatMap还有一种使用方式可以去除nil，目前已经废弃，由compactMap替代

```swift
let arr = [[1,3,4],[4,5,6]]
print(arr.flatMap{$0}) // [1, 3, 4, 4, 5, 6]
```

### filter

> 使用规则过滤集合

```swift
let arr = [1,3,4,5,6,7]
print(arr.filter{$0>4}) //[5,6,7]
```

### reduce

> 归纳合并成一个元素

```swift
let arr = [1,3,4,5,6,7]
print(arr.reduce(0){$0+$1}) // 26
```

### sort

> 排序

```swif
let arr = [4,1,8,3,2,10]
print(arr.sorted{$1<$0})
print(arr.sorted()) // 默认升序
print(arr.sorted(by: <))
var arr1 = [4,1,8,3,2,10]
arr1.sort()
print(arr1)
```

### reverse

> 翻转

```swift
var arr = [4,1,8,3,2,10]
arr.reverse()
print(arr)
let arr1 = [4,1,8,3,2,10]
print(arr1.reversed()) // 注意类型
for value in arr1.reversed() {
  print(value)
}
for (index,value) in arr1.enumerated().reversed() {
  print("\(index),\(value)")
}
```

