# 函数

## 无参函数

```python
def sayHello():
    print('Hello!')
print(type(sayHello)) # <class 'function'>
sayHello()
```

## 有参函数

```python
def sayName(name):
    print(f'my name is {name}')
sayName('lily')
```

## 指定参数名称传参

```python
def selfIntroduce(name, age):
    print(f'my name is {name}, I\'m {age} year\'s old')
selfIntroduce(age=18, name='nick')
```

## 参数默认值

```python
def showUp(name, height=1.77):
    print(f'my name is {name}, height is {height}')
showUp('lucy')
```

## 返回值

```python
def sum(a, b):
    return a + b
c = sum(5, 6)
print(c)
```

## 列表传递

```python
nums = [2, 45, 17, 20]
def mySort(list_nums: list):
    list_nums.sort()
    return list_nums
# 会修改nums的顺序
mySort(nums)
print(nums)

# 如果不想修改，可以传递副本
nums = [2, 45, 17, 20]
ret = mySort(nums[:])
print(nums)
print(ret)
```

## 任意数量的参数

> 会将这些参数封装到一个元祖中

```python
def makeNoise(*noises):
    print(type(noises)) # <class 'tuple'>
    print(noises)
makeNoise('ooo', 'lalala', 'wowowwo')
```

## 任意数量的指定参数名的实参

> 会将这些参数和参数名封装到一个字典中

```python
def writeInfos(name, age, **others):
    print(type(others)) # <class 'dict'>
    others['name'] = name
    others['age'] = age
    print(others)
writeInfos('lily', 18, height=1.77, weight=102) # {'height': 1.77, 'weight': 102, 'name': 'lily', 'age': 18}
```

## 导入同一目录下的模块

### 导入整个模块

```python
import tool
print(tool.mySum(5, 6))
```

### 导入模块中的某一个方法

```python
from tool import mySum
print(mySum(6, 7))
```

### 导入模块中的某一个方法，并指定别名

```python
from tool import mySum as ms
print(ms(7, 8))
```

### 给模块指定别名

```python
import tool as t
print(t.mySum(2, 3))
```



## 导入不同目录下的模块

为方便表述，我们假设：a.py 要 import 文件 b.py

### b.py 在 子目录 test下

需要先在test目录下创建一个空文件 __init__.py。创建该文件的目的是将test目录变成一个Python包。

![子目录](https://cdn.jsdelivr.net/gh/lifeasy/ImageBed@master/img/202209200018058.jpeg)

然后我们就可以通过如下方式 import

```python
import test.b
```

如果test包中还有子目录 sub_test/，则**不需要**在sub_test/中创建 __init__.py 即可通过如下方式导入 sub_test/中的 c.py

```python
import test.sub_test.c
```

### b.py在任意路径下

假设 b.py 在路径 ~/test 下，则需要通过如下代码将路径加入到系统路径中，然后直接导入 b.py即可。

```python
import sys
sys.path.append(r"~/test")
import b
```

