# 文件

## with 关键字

推荐使用`with`关键字，无需手动调用`close`关闭文件，`python`会在合适的时机关闭文件

## 读取文件

> r 只读模式 
>
> r+ 读写模式 

```python
# 读取文件
file_path = './python_learn/file.txt'
# 读取整个文件
with open(file_path) as file_object:
    content = file_object.read()
print(content.rstrip())

print('--------------------------------------------------')

# 按行读取文件
with open(file_path) as file_object:
    for line in file_object:
        print(line.rstrip())
        
print('--------------------------------------------------')

# 读取所有行，存储到列表
with open(file_path) as file_object:
    lines = file_object.readlines()
for line in lines:
    print(line.rstrip())
```

## 写入文件

>w 写入模式，返回文件对象前，会清空文件
>
>a 附加模式，返回文件对象前，不会清空文件
>
>a+ 附加读写模式

```python
# 写入文件 
file_path = './python_learn/file.txt'

# w 写入模式 ，在返回文件对象前，会清空文件
with open(file_path, 'w') as file_object:
    file_object.write('I love programming.\n')
    
print('--------------------------------------------------')

# a 附加模式 ，在返回文件对象前，不会清空文件
with open(file_path, 'a') as file_object:
    file_object.write('I love programming.\n')
```

