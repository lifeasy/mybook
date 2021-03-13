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
```

### lipo

```shell
# 查看架构
$ lipo -info BioAuthEngine.framework
# 架构分离
$ lipo 文件路径 -thin 架构类型 -output 输出文件路径
$ lipo BioAuthEngine.framework/BioAuthEngine -thin arm64 -o BioAuthEngine64
# 架构合并
$ lipo 文件路径1 文件路径2 -output 输出文件路径
```

### ar

```shell
# 分离静态库
$ ar -x BioAuthEngine64
```

strings

```shell
# 查看二进制文件中的strings
$ strings qihooloan_ios | grep --color=auto -ir "checkCachedLicense"
```

Xcode

```shell
# 查看当前环境的SDK版本
xcodebuild -showsdks
```

## 编译链接相关

