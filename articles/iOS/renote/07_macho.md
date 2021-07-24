# Mach-O

## 格式

编译顺序会影响Mach-O文件的内容

查看Mach-O代码段可以发现不同编译顺序出来的可执行文件的__TEXT的代码段排列就不一样

```shell
# 查看__text
objdump --macho -d <path>
```

## 通用二进制

Universal binary

多种架构的二进制文件

## Mach-O文件格式

### VM 

VM Addr : 虚拟内存地址

VM Size： 虚拟内存大小

File offset: 数据在文件中偏移量

File size: 数据在文件中的大小

### FatHeader

offset：距离文件开始地方的偏移

size：文件的大小

分页：x86_64 4096，iOS 16K

### Header

```c++
struct mach_header_64 {
	uint32_t	magic;		/* mach magic number identifier 魔数 快速定位32位还是64位*/
	cpu_type_t	cputype;	/* cpu specifier cpu类型 比如arm*/
	cpu_subtype_t	cpusubtype;	/* machine specifier cpu具体类型 比如arm64*/
	uint32_t	filetype;	/* type of file  文件类型 比如可执行文件*/
	uint32_t	ncmds;		/* number of load commands  LC数量*/
	uint32_t	sizeofcmds;	/* the size of all the load commands LC总大小*/
	uint32_t	flags;		/* flags 二进制文件支持的功能，主要用于加载、链接等等*/
	uint32_t	reserved;	/* reserved 保留字段*/
};
```

### Load Command

pagezero：64和32位兼容，64位在地址上加上一个4G，vm size 4g，file size不占空间

LC_DYLD_INFO_ONLY：动态链接相关信息

| PAGEZERO              | 64和32位兼容，64位在地址上加上一个4G，vm size 4g，file size不占空间 |
| --------------------- | ------------------------------------------------------------ |
| LC_SEGMENT_64         | 将文件中（32位或64位）的段映射到进程地址空间中               |
| LC_DYLD_INFO_ONLY     | 动态链接相关信息                                             |
| LC_SYMTAB             | 符号地址                                                     |
| LC_DYSYMTAB           | 动态符号表地址                                               |
| LC_LOAD_DYLINKER      | 使用谁加载，我们使用dyld                                     |
| LC_UUID               | 文件的UUID                                                   |
| LC_VERSION_MIN_MACOSX | 支持最低的操作系统版本                                       |
| LC_SOURCE_VERSION     | 源代码版本                                                   |
| LC_MAIN               | 设置程序主线程的入口地址和栈大小                             |
| LC_LOAD_DYLIB         | 依赖库的路径，包含三方库                                     |
| LC_FUNCTION_STARTS    | 函数起始地址表                                               |
| LC_CODE_SIGNATURE     | 代码签名                                                     |

### Data

* dyld绑定符号函数绑定，符号绑定也是一个函数，需要提前绑定，才能使用这个函数，绑定其他的符号

![dyld绑定符号函数绑定](https://gitee.com/dexport/blog-image/raw/master/img/20210724235956.png)