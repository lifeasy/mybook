# 用户输入和While循环

## input

```python
name = input('please input your name:')
print(f'your name is {name}')
```

## While循环

```python
import time

#中断循环 break 跳过 continue
num = 5
while True:
    num = num + 1
    if num > 10 :
        break
    elif num == 9 :
        continue
    time.sleep(1) #休眠一秒
    print(num)
```
