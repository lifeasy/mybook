# 字典

## 初始化

```swift
let dic : Dictionary<String,Int> = [:]
let dic1 : [String : Int] = [:]
let dic2 = [String : Int]()
var dic3 = Dictionary<String,Int>()
```

## 常用属性

```swift
let dic = [String:Int]()
print(dic.isEmpty) // true
print(dic.count) // 0
```

## 所有keys values

```swift
let dic = ["name":"lisi","class":"3"]
print(dic.keys) // ["name", "class"]
print(dic.values) // ["lisi", "3"]
```

## 遍历 

> 遍历的输入是一个元组

```swift
let dic = ["name":"lisi","class":"3"]
for (key,value) in dic {
  print("\(key),\(value)")
}
dic.forEach{(key,value) in print("\(key),\(value)")}
dic.map{print("\($0.0),\($0.1)")}
```

## 字典取值

> 字典取值是可选值

```swift
let dic = ["name":"lisi","class":"3"]
// 字典取值是可选值
print(dic["name"] ?? "") // list
print(dic["age"]) // nil
```

## 增删改

```swift
var dic = ["name":"lisi","class":"3","book":"yuwen"]
dic["name"] = "zhangsan"
print(dic) //["class": "3", "name": "zhangsan", "book": "yuwen"]
dic.updateValue("wangwu", forKey: "name")
print(dic)//["class": "3", "name": "wangwu", "book": "yuwen"]
dic["name"] = nil
print(dic)//["class": "3", "book": "yuwen"]
dic.removeValue(forKey:"book")
print(dic)//["class": "3"]
```

