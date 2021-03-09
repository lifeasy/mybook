# iPhone终端中文显示问题

新建`~/.inputrc`

编辑文件内容：

```shell
# 不将中文转化为转义序列
set convert-meta off
# 允许向终端输出中文
set output-meta on
# 允许向终端输入中文
set meta-flag on
set input-meta on
```



