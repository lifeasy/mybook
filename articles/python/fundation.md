# Python基本数据类型

## 变量

变量名使用字母、数字、下划线命名。不能以数字开头

```python
message = "Hello World"
print(message)
message = "Hello new World"
print(message)
```

## 字符串

字符串可以使用单引号或者双引号，可以灵活混合使用

```python
"hello , this is 'a little' some wrong"
```

### 字符串的一些方法

* 首字母大写

```python
name = "ada"
# Ada 
print(name.title())
```

* 转换大小写

```python
name = "Kevin liang"
# kevin liang
# KEVIN LIANG
print(name.lower())
print(name.upper())
```
