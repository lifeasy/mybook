# 集合

> 去重

## 初始化

```swift
let set1 : Set<String> = ["1","2","2","3"]
print(set1) // ["3", "1", "2"]
// 从序列初始化（数组、字符串等）
let set2 = Set(["1","2","2","3"])
print(set2)
```

## 常用方法

```swift
var set : Set<String> = ["1","2","2","3","哈哈","呵呵","🌶"]
print(set.isEmpty)
print(set.count)
print(set.contains("🌶"))
print(set.max()!,set.min()!)
for value in set {
  print(value)
}
// 插入
set.insert("😴")
print(set)
set.remove("2")
print(set)
```

## 交集 并集 补集 差集

```swift
var set1 : Set<String> = ["1","2","2","3","哈哈","呵呵","🌶"]
var set2 : Set<String> = ["1","7","2","3","嘻嘻","呵呵","☺"]

// 交集
print(set1.intersection(set2)) //["呵呵", "2", "3", "1"]
// 并集
print(set1.union(set2)) //["7", "哈哈", "3", "☺", "呵呵", "🌶", "1", "嘻嘻", "2"]
// 补集 set1中有而set2中没有的
print(set1.subtracting(set2)) // ["哈哈", "🌶"]
// 差集 并集-交集=差集 相互没有的
print(set1.symmetricDifference(set2))
```

