# 列表

## 列表定义

```python
names = ['jack', 'lily', 'kevin']
print(names)
print(type(names)) # <class 'list'>
```

## 下标

```python
# 可以使用 -1 访问最后一个元素
print(names[-1])
```

## 增删改

```python
# 改
names[1] = 'david'
print(names)

# 增
names.append('Ravi')
print(names)

# 插入
names.insert(1, 'Monica')
print(names)

# 删除
del names[1]
print(names)

# pop
last = names.pop()
print(last)
print(names)

# 删除第一个出现的值
remove = names.remove('david')
print(remove)
print(names)
print(type(names))
```

## 排序

```python
# 排序
nums = [45, 67, 100, 2, 29]
nums.sort()
print(nums)
nums.sort(reverse=True)
print(nums)

nums = [45, 67, 100, 2, 29]
print(sorted(nums))
print(nums)
print(sorted(nums, reverse=True))
```

## 翻转

```python
# 翻转
cars = ['bmw', 'audi', 'toyota', 'subaru']
cars.reverse()
print(cars)
```

## 长度

```python
# 长度
cars = ['bmw', 'audi', 'toyota', 'subaru']
print(len(cars))
```

## 遍历

```python
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
    print(magician.title())
    print(len(magician))
print('loop end')
```

## 创建数值列表(range)

```python
# 创建数值列表 
for num in range(1, 4): # [1, 4)
    print(num)

for num in range(4): # [0, 4)
    print(num)

nums = list(range(3, 7))
print(nums)

nums= list(range(1, 10, 2))
print(nums)
```

## 列表解析

```python
# 列表解析
nums = [num ** 2 for num in range(2, 7)]
print(nums)
```

## 简单统计

```python
# 简单统计
nums = [3, 33, 65, 12, 18, 2]
print(max(nums))
print(min(nums))
print(sum(nums))
```

## 列表切片（子列表）

```python
# 列表切片（子列表）
nums = [3, 33, 65, 12, 18, 2]
# [2, 4) 列表中的第2,3个元素
print(nums[2:4])
print(nums[2:])
print(nums[:4])
for num in nums[:4]:
    print(num)
```

## 列表复制

```python
# 复制列表
nums = [3, 33, 65, 12, 18, 2]
copy_nums = nums[:]
print(copy_nums)
```

## 元祖

```python

```
