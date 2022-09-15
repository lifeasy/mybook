# python基本数据类型

## 输出数据的类型信息

```python
name = 1.0
print('标识', id(name))
print('类型', type(name))
print('值', name)
```

## 浮点精度

```python
from decimal import Decimal
n1 = 1.1
n2 = 2.2
# 3.3000000000000003
print(n1 + n2)

# 3.3
print((Decimal('1.1') + Decimal('2.2'))
```

## 字符串

```python
str1='123'
str2="123"
str3="""1231"""
str4="""123123123
123123123123"""
print(str4)
```




