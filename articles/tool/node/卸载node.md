# 卸载重装node

> npm 出现问题，这里演示卸载，并通过brew安装

## 卸载

如果不是通过brew安装的

```shell
# 新建脚本并执行
#!/bin/bash
lsbom -f -l -s -pf /var/db/receipts/org.nodejs.pkg.bom \
| while read i; do
  sudo rm /usr/local/${i}
done
sudo rm -rf /usr/local/lib/node \
     /usr/local/lib/node_modules \
     /var/db/receipts/org.nodejs.*
```

如果是通过[官方网站](https://nodejs.org/en/)安装的执行下面命令

```shell
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```

## 安装node

```shell
brew install node
```

安装后有可能在终端中输入node找不到该命令，执行如下命令

```shell
brew link node
```

得到如下结果

```shell
Linking /usr/local/Cellar/node/11.2.0...
Error: Could not symlink include/node/common.gypi
Target /usr/local/include/node/common.gypi
already exists. You may want to remove it:
 rm '/usr/local/include/node/common.gypi'
To force the link and overwrite all conflicting files:
 brew link --overwrite node
To list all files that would be deleted:
 brew link --overwrite --dry-run node
localhost:wkdir meng$ brew link --overwrite node
Linking /usr/local/Cellar/node/11.2.0...
Error: Could not symlink include/node/common.gypi
/usr/local/include/node is not writable.
```

根据提示执行

```shell
brew link --overwrite --dry-run node
```

根据提示删除这些冲突：

```shell
rm -rf /usr/local/include/node/common.gypi
rm -rf /usr/local/include/node/config.gypi
rm -rf /usr/local/include/node/libplatform/libplatform-export.h
rm -rf /usr/local/include/node/libplatform/libplatform.h
rm -rf /usr/local/include/node/libplatform/v8-tracing.h
...
```

删除上面的冲突的文件再运行

```shell
brew link --overwrite node
```

如果报错

```shell
$ brew link node
Linking /usr/local/Cellar/node/15.11.0...
Error: Could not symlink share/systemtap/tapset/node.stp
/usr/local/share/systemtap/tapset is not writable.
```

解决：

```shell
$ sudo chown -R 360jr:admin /usr/local/share/systemtap
$ sudo rm -rf /usr/local/share/systemtap/tapset/node.stp
# 再执行
$ brew link node
Linking /usr/local/Cellar/node/15.11.0... 7 symlinks created.
```

## 参考文献

[brew install node brew link node 遇到问题解决](https://www.cnblogs.com/yuzhaoblog/p/14005453.html)

[Mac卸载node并使用brew重新安装](https://cloud.tencent.com/developer/article/1410438)