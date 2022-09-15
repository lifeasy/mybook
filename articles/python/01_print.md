# print输出函数

## 输出数字

```python
print(1234)
```

## 输出字符串

```python
print('123')
print("123")
```

## 计算表达式结果

```python
print(1+3)
```

## 数据输入到文件

* `a+`表示不存在就创建，如果存在就追加

```python
ft = open('./file.txt', 'a+')
print('hello python', file=ft)
ft.close()
```

## 可以打印多个 分隔符默认为空格，可以使用seq指定

```python
print('hello', 'world', '666', sep='*')
```

## 转义字符

```python
print('hello\tworld')
```

## 原始字符

```python
print(r'hello\tworld')
```

## 输出字符编码（python应该用的是unicode）

```python

```
