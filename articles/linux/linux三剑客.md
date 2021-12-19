# linux三剑客

## 普通剑客

```bash
$ cat student.txt
id12-张三-25-男-178-55
id24-李四-34-男-189-89
id05-王五-28-男-168-65
id19-赵华-20-男-179-60
id45-牛莉-28-女-160-45
```

### cut

* 用指定规则来切分文本

```bash
# 用 "-" 切分文本的每行，展示前三列
$ cut -d '-' -f1,2,3 student.txt
id12-张三-25
id24-李四-34
id05-王五-28
id19-赵华-20
id45-牛莉-28
```

### sort

* 对文本中的行进行排序

```bash
$ sort student.txt
id05-王五-28-男-168-65
id12-张三-25-男-178-55
id19-赵华-20-男-179-60
id24-李四-34-男-189-89
id45-牛莉-28-女-160-45

# sort -r 逆序
$ sort -r student.txt
id45-牛莉-28-女-160-45
id24-李四-34-男-189-89
id19-赵华-20-男-179-60
id12-张三-25-男-178-55
id05-王五-28-男-168-65

# 使用 '-' 切分 然后用第三列排序 默认比较的是字符串
$ sort -t '-' -k3 student.txt
id19-赵华-20-男-179-60
id12-张三-25-男-178-55
id45-牛莉-28-女-160-45
id05-王五-28-男-168-65
id24-李四-34-男-189-89

# 使用 '-' 切分 然后用第三列排序 -n 是按照数字大小进行比较
$ sort -t '-' -n -k3 student.txt
id19-赵华-20-男-179-60
id12-张三-25-男-178-55
id05-王五-28-男-168-65
id45-牛莉-28-女-160-45
id24-李四-34-男-189-89
```

### wc

* word count 统计单词

```bash
$ wc student.txt
#     行      单词数量  字符数量
       5       6     130 student.txt
# 也可以使用 -l 统计行
$ wc -l student.txt
       5 student.txt
# 使用 -w 统计单词 以空格分隔单词      
$ wc -w student.txt
       6 student.txt
# 使用 -c 统计字符
$ wc -c student.txt
     130 student.txt
```

## 高级剑客

### grep

[正则参考](https://www.cnblogs.com/chensiqiqi/p/6285060.html)

```bash
# -n 显示行号 可以查询多个文本
$ grep 1 -n student.txt /Users/360jr/Downloads/https.txt
student.txt:1:id12-张三-25-男-178-55
student.txt:2:id24-李四-34-男-189-89
student.txt:3:id05-王五-28-男-168-65
student.txt:4:id19-赵华-20-男-179-60
student.txt:5:id45-牛莉-28-女-160-45
/Users/360jr/Downloads/https.txt:1:http://cdnimg.3g.qq.com/qq_product_operations/nettest/index.html?r=1525275329
/Users/360jr/Downloads/https.txt:2:http://cdnimg.3g.qq.com/qq_product_operations/nettest/index.html?r=1525275330

# -v 显示不匹配 -i忽略大小写
$ grep 45 -nvi student.txt
1:id12-张三-25-男-178-55
2:id24-李四-34-男-189-89
3:id05-王五-28-男-168-65
4:id19-赵华-20-男-179-60

# -E 使用正则匹配
$ grep -E "张" -n student.txt
1:id12-张三-25-男-178-55
```

### sed

* 对文本增删改查
* 普通操作只是显示在终端，不会真实改变文本，如果改变需要使用 sed -i

[sed参考](https://www.cnblogs.com/chensiqiqi/p/6382080.html)

* Sed命令是操作，过滤和转换文本内容的强大工具。常用功能有增删改查（增加，删除，修改，查询），其中查询的功能中最常用的2大功能是过滤（过滤指定字符串），取行（取出指定行）。

* 语法

```bash
sed [options] [sed -commands][input -file]
sed [选项]  【sed命令】 【输入文件】
```

* Sed软件从文件或管道中读取一行，处理一行，输出一行；再读取一行，再处理一行，再输出一行....，不会把文件全部读入内存，对内存的压力很小

### awk

[awk参考](https://www.cnblogs.com/chensiqiqi/p/6481647.html)

* awk 是一门语言

* awk -F 操作什么数据，执行什么动作

```bash
# 以 '-' 分组 ，行号大于等于2 小于等于4
$ awk -F '-' 'NR>=2&&NR<=4' student.txt
id24-李四-34-男-189-89
id05-王五-28-男-168-65
id19-赵华-20-男-179-60
```

* 使用awk获取本机ip