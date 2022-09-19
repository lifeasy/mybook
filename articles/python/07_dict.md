# 字典

## 创建一个空字典

```python
myDic = {}
print(type(myDic)) # <class 'dict'>
```

## 添加键值对

```python
myDic = {}
myDic['name'] = 'lily'
print(myDic)
```

## 修改

```python
myDic = {'name': 'lily'}
myDic['name'] = 'Lily'
print(myDic)
```

## 删除

```python
myDic = {'name': 'lily', 'age': 35}
del myDic['name']
print(myDic)
```

## 安全访问

> 如果直接[]访问了不存在的key，会报错
> 
> 使用 get() 方法

```python
myDic = {}
# print(myDic['name']) # KeyError: 'name'
print(myDic.get('name')) # None
if myDic.get('name') == None:
    print('name not in myDic')
print(myDic.get('name', 'name not in myDic'))
```

# 遍历

```python
myDic = {
    'name': 'lily',
    'age': 18,
    'height': 1.77,
    'number': 18
    }
for key, value in myDic.items():
    print(f'key = {key}, value = {value}')

for key in myDic.keys():
    print(f'key = {key}')

for value in myDic.values():
    print(f'value = {value}')
```

## 集合

```python

```
