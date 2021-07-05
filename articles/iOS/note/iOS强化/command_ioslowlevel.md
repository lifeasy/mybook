# 常用命令

## Mach-O操作相关

### otool

```shell
# 查看header
$ otool -h /usr/bin/xcode-select
/usr/bin/xcode-select:
Mach header
      magic  cputype cpusubtype  caps    filetype ncmds sizeofcmds      flags
 0xfeedfacf 16777223          3  0x00           2    20       1648 0x00200085
# 查看LC
 $ otool -l /usr/bin/xcode-select
 /usr/bin/xcode-select:
 Load command 0
```

### objdump

```shell
# objdump 更容易阅读
# 查看header & lc
objdump --macho --private-header <path>
Mach header
      magic cputype cpusubtype  caps    filetype ncmds sizeofcmds      flags
MH_MAGIC_64  X86_64        ALL  0x00     EXECUTE    20       1960   NOUNDEFS DYLDLINK TWOLEVEL WEAK_DEFINES BINDS_TO_WEAK PIE
# 查看__text
objdump --macho -d <path>
(__TEXT,__text) section
-[ViewController viewDidLoad]:
100005d40:    ff c3 00 d1    sub    sp, sp, #48
100005d44:    fd 7b 02 a9    stp    x29, x30, [sp, #32]
...
# 查看符号
# objdump --macho -syms <path>
objdump --macho -syms /Users/360jr/Library/Developer/Xcode/DerivedData/LGQiangHuaDemo-cbqtsqzmnxlyglewsnjzasrkwdhh/Build/Products/Debug-iphoneos/LGQiangHuaDemo.app/LGQiangHuaDemo
/Users/360jr/Library/Developer/Xcode/DerivedData/LGQiangHuaDemo-cbqtsqzmnxlyglewsnjzasrkwdhh/Build/Products/Debug-iphoneos/LGQiangHuaDemo.app/LGQiangHuaDemo:
SYMBOL TABLE:
0000000100005d40 l     F __TEXT,__text -[ViewController viewDidLoad]
...
# 查看导出符号
objdump --macho --exports-trie ${MACH_PATH}
# 查看间接符号
objdump --macho --indirect-symbols ${MACH_PATH}

```

### nm

```shell
# 人类友好的方式输出符号表
nm -m <path>
# -n 排序 -p 不排序
```

### lipo

```shell
# 查看架构
$ lipo -info BioAuthEngine.framework
# 架构分离
$ lipo 文件路径 -thin 架构类型 -output 输出文件路径
$ lipo BioAuthEngine.framework/BioAuthEngine -thin arm64 -o BioAuthEngine64
# 架构合并
$ lipo -create 文件路径1 文件路径2 -output 输出文件路径
```

### libtool

```shell
# 将目标文件.o合并为静态库
libtool -static -o xxxx.a *.o
```

### ar

```shell
# 分离静态库
$ ar -x BioAuthEngine64
```

### strings

```shell
# 查看二进制文件中的strings
$ strings qihooloan_ios | grep --color=auto -ir "checkCachedLicense"
```

### Xcode

```shell
# 查看当前环境的SDK版本
$ xcodebuild -showsdks
# 查看编译时间
$ defaults write com.apple.dt.Xcode ShowBuildOperationDuration YES

# Xcode环境变量
# 查看App启动时间
DYLD_PRINT_STATISTICS DYLD_PRINT_STATISTICS_DETAILS
```

### Xcrun

```shell
# 查看Rebase和Bind信息
$ xcrun dyldinfo -bind InjectDemo
bind information:
segment section          address        type    addend dylib            symbol
__DATA  __objc_data      0x100008E18    pointer      0 libobjc          _OBJC_METACLASS_$_NSObject
__DATA  __objc_data      0x100008E40    pointer      0 libobjc          _OBJC_METACLASS_$_NSObject
__DATA  __objc_data      0x100008E00    pointer      0 libobjc          __objc_empty_cache
__DATA  __objc_data      0x100008E28    pointer      0 libobjc          __objc_empty_cache
__DATA  __objc_data      0x100008E50    pointer      0 libobjc          __objc_empty_cache
...
```



## 编译链接相关

### 预处理

```shell
# 查看预处理
xcrun clang -E main.c
```

### 词法分析(lexical anaysis)

生成token

```shell
# 词法分析
$ xcrun clang -fmodules -fsyntax-only -Xclang -dump-tokens main.c
```

### 语法分析(semantic analysis)

词法分析的Token流会被解析成一颗抽象语法树(abstract syntax tree - AST)。

有了抽象语法树，clang就可以对这个树进行分析，找出代码中的错误。比如类型不匹配，亦或Objective C中向target发送了一个未实现的消息。

```shell
# 语法分析
$ xcrun clang -fsyntax-only -Xclang -ast-dump main.c | open -f
```

### CodeGen （生成IR）

```shell
$ xcrun clang -S -emit-llvm main.c -o main.ll
```

### 生成汇编代码

```shell
$ xcrun clang -S main.c -o main.s
```

### 汇编器（机器码、目标文件）

```shell
$ xcrun clang -fmodules -c main.c -o main.o
```

### 链接（生成mach-o）

```shell
$ xcrun clang main.o -o main
```

## 





