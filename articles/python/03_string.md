# 字符串

## 多行字符串

```python
str1='123'
str2="123"
str3="""1231"""
str4="""123123123
123123123123"""
print(str4)
```

## 字符串大小写

```python
name = "ada lovelace"
# Ada Lovelace 首字母大写
print(name.title())
# ADA LOVELACE 大写
print(name.upper())
# ada lovelace 小写
print(name.lower())
```

## 字符串格式化

> f字符串（f = format）Python 3.6引入，如果版本小于3.6，使用format()方法

```python
first_name = "kevin"
second_name = "liang"
# version >= 3.6
full_name = f"{first_name} {second_name}"
print(full_name)
print(f"Hello, {full_name.title()}")
# version <= 3.5
print("{} {}".format(first_name, second_name))
```

## 除去字符串首尾指定字符

### rstrip 尾部去除

```python
# 去除字符串尾部的空格 rstrip方法，可以指定去除字符，默认空格
space_string = "begin_end  "
tail_string = "*****"
print(f"{space_string}{tail_string}")
print(f"{space_string.rstrip()}{tail_string}")
# 移除逗号(,)、点号(.)、字母 s、q 或 w，这几个都是尾随字符
txt = "banana,,,,,ssqqqww....."
print(txt.rstrip(",.qsw")) # banana
```

### lstrip 首部去除

```python
# 删除开头空白 lstrip
txt = "   banana"
print(txt.lstrip())
```

### strip 首尾去除

```python
# 删除两头空白 strip
tail_string = "*****"
txt = "    banana   "
print(f"{txt.strip()}{tail_string}")
```
