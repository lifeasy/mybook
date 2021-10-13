# Shell脚本

## 脚本解释器

> 在 shell 脚本，`#!` 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 解释器。`#!` 被称作shebang（也称为 Hashbang ）。

* 指定sh解释器

```shell
#!/bin/bash
#!/bin/sh
#!/bin/zsh
```

> **注意**
>
> 上面的指定解释器的方式是比较常见的，但有时候，你可能也会看到下面的方式：
>
> ```shell
> #!/usr/bin/env bash
> ```
>
> 这样做的好处是，系统会自动在 `PATH` 环境变量中查找你指定的程序（本例中的`bash`）。相比第一种写法，你应该尽量用这种写法，因为程序的路径是不确定的。这样写还有一个好处，操作系统的`PATH`变量有可能被配置为指向程序的另一个版本。比如，安装完新版本的`bash`，我们可能将其路径添加到`PATH`中，来“隐藏”老版本。如果直接用`#!/bin/bash`，那么系统会选择老版本的`bash`来执行脚本，如果用`#!/usr/bin/env bash`，则会使用新版本。

* 模式
  * 交互模式：终端中执行命令
  * 非交互模式：脚本

## 基本语法

### 解释器

`#!` 决定了脚本可以像一个独立的可执行文件一样执行，而不用在终端之前输入`sh`, `bash`, `python`, `php`等。

```shell
# 以下两种方式都可以指定 shell 解释器为 bash，第二种方式更好
#!/bin/bash
#!/usr/bin/env bash
```

## 注释

- 单行注释 - 以 `#` 开头，到行尾结束。
- 多行注释 - 以 `:<<EOF` 开头，到 `EOF` 结束。

```shell
#--------------------------------------------
# shell 注释示例
# author：zp
#--------------------------------------------

# echo '这是单行注释'

########## 这是分割线 ##########

:<<EOF
echo '这是多行注释'
echo '这是多行注释'
echo '这是多行注释'
EOF
```

## echo

* 输出普通字符串

```shell
echo "hello, world"
# Output: hello, world
```

* 字符串中引号转义

```shell
echo "hello, \"zp\""
# Output: hello, "zp"
```

* 带变量

```shell
name=zp
echo "hello, \"${name}\""
# Output: hello, "zp"
```

* 换行符

```shell
# 输出含换行符的字符串
echo "YES\nNO"
#  Output: YES\nNO

echo -e "YES\nNO" # -e 开启转义
#  Output:
#  YES
#  NO

echo -e "YES\c" # -e 开启转义 \c 不换行
echo "NO"
#  Output:
#  YESNO
```

* 输出重定向至文件

```shell
echo "Hello World" > hello.txt
```

* 输出执行结果

```shell
echo `pwd`
```



## 参考文献

[一篇文章让你彻底掌握 shell 语言](https://juejin.cn/post/6844903784158593038)

