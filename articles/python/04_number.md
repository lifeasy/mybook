# 数字

## 平方

```python
print(3 ** 2)
```

## 浮点数

```python
# 浮点精度
from decimal import Decimal

# 3.3000000000000003
n1=1.1
n2=2.2
print(n1+n2)
# 3.3
print(Decimal('1.1') + Decimal('2.2'))

# 两个任意数相除，结果都是浮点数，即使这两个数都是整数且能整除
num = 4 / 2
print(num)
print(type(num)) # <class 'float'>
```

## 数字中的下划线

```python
# 数字中的下划线
num = 1_000_000
print(num) # _ 不会输出
```
