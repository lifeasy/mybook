# 异常处理

## 捕获单个异常

```python
try:
    answer = 5 / 0
except ZeroDivisionError:
    print('除数不能为0')
except FileExistsError:
    print('文件不存在')
except FileNotFoundError:
    pass
else:
    print(f'answer = {answer}')
finally:
    print('finished calc')
```

## 捕获所有异常

```python
# 捕获所有异常
try:
    answer = 5 / 0
except Exception as e:
    print(e)
else:
    print(f'answer = {answer}')
```

