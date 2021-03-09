# Mach-O

## Mach-O Header格式

```C
struct mach_header_64 {
	uint32_t	magic;		/* mach magic number identifier */
	cpu_type_t	cputype;	/* cpu specifier */
	cpu_subtype_t	cpusubtype;	/* machine specifier */
	uint32_t	filetype;	/* type of file */
	uint32_t	ncmds;		/* number of load commands */
	uint32_t	sizeofcmds;	/* the size of all the load commands */
	uint32_t	flags;		/* flags */
	uint32_t	reserved;	/* reserved */
};
```



## 常见Mach-O文件格式

```C
#define	MH_OBJECT	0x1		/* relocatable object file */
#define	MH_EXECUTE	0x2		/* demand paged executable file */
#define	MH_FVMLIB	0x3		/* fixed VM shared library file */
#define	MH_CORE		0x4		/* core file */
#define	MH_PRELOAD	0x5		/* preloaded executable file */
#define	MH_DYLIB	0x6		/* dynamically bound shared library */
#define	MH_DYLINKER	0x7		/* dynamic link editor */
#define	MH_BUNDLE	0x8		/* dynamically bound bundle file */
#define	MH_DYLIB_STUB	0x9		/* shared library stub for static
					   linking only, no section contents */
#define	MH_DSYM		0xa		/* companion file with only debug
					   sections */
#define	MH_KEXT_BUNDLE	0xb		/* x86_64 kexts */
#define MH_FILESET	0xc		/* a file composed of other Mach-Os to
					   be run in the same userspace sharing
					   a single linkedit. */
```

* .o    目标文件
* .a    静态库（.o文件的合集）
* executable    可执行文件
* .dylib    动态库
* .framework/xxx    动态库
* /usr/lib/dyld    dyld动态连接器
* .dSYM/Contents/Resources/DWARF/xx    dsym

## 常用命令

### file

```shell
$ file /usr/bin/xcode-select
/usr/bin/xcode-select: Mach-O universal binary with 2 architectures: [x86_64:Mach-O 64-bit executable x86_64] [arm64e:Mach-O 64-bit executable arm64e]
/usr/bin/xcode-select (for architecture x86_64):	Mach-O 64-bit executable x86_64
/usr/bin/xcode-select (for architecture arm64e):	Mach-O 64-bit executable arm64e
```

### otool

```shell
$ otool -h /usr/bin/xcode-select
/usr/bin/xcode-select:
Mach header
      magic  cputype cpusubtype  caps    filetype ncmds sizeofcmds      flags
 0xfeedfacf 16777223          3  0x00           2    20       1648 0x00200085
```

```shell
$ otool -l /usr/bin/xcode-select
/usr/bin/xcode-select:
Load command 0
      cmd LC_SEGMENT_64
  cmdsize 72
  segname __PAGEZERO
   vmaddr 0x0000000000000000
   vmsize 0x0000000100000000
  fileoff 0
 filesize 0
  maxprot 0x00000000
 initprot 0x00000000
   nsects 0
    flags 0x0
```

### lipo

```shell
$ lipo -info /usr/bin/xcode-select
Architectures in the fat file: /usr/bin/xcode-select are: x86_64 arm64e
```

```shell
# 架构分离
$ lipo 文件路径 -thin 架构类型 -output 输出文件路径
```

```shell
# 架构合并
$ lipo 文件路径1 文件路径2 -output 输出文件路径
```

### nm

```shell
$ nm /usr/bin/xcode-select
                 U _CFRelease
                 U _CFStringCreateWithCString
                 U _MDItemCreate
                 U __MDItemMarkAsUsed
```



## 参考资料

[Mach-O 文件格式探索](https://www.desgard.com/iOS-Source-Probe/C/mach-o/Mach-O%20%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F%E6%8E%A2%E7%B4%A2.html)

[Apple 操作系统可执行文件 Mach-O](https://ming1016.github.io/2020/03/29/apple-system-executable-file-macho/)



