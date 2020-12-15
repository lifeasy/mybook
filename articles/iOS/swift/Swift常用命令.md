# Swift常用命令

## swiftc

> swift编译相关

```shell
# 源码编译为sil中间文件 | 编译后的符号解析
swiftc -emit-sil main.swift | xcrun swift-demangle >> ./main.sil && open main.sil
```

```shell
# 生成抽象语法树
$ swiftc -dump-ast main.swift
```

## cat address

> 使用lldb插件 libfooplugin.dylib

```shell
(lldb) cat address 0x100511630
&0x100511630, (char *) $4 = 0x00007ffeefbff2a0 "0x100511630 heap pointer, (0x30 bytes), zone: 0x7fff88ac1000"
```

