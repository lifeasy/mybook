# LLDB常用命令

## x/8g

> x/8g 内存地址
>
> 查看内存地址里的值

```shell
(lldb) x/8g 0x101840890
0x101840890: 0x0000000100008168 0x0000000200000003
0x1018408a0: 0x0000000000000014 0x0002000000000000
0x1018408b0: 0x0000000080080000 0x00007fff806e4fa0
0x1018408c0: 0x0000000000000000 0x00007fff8849b4f8
```

## register read

> 读取寄存器

```shell
(lldb) register read
General Purpose Registers:
       rax = 0x00007fff80be7fe8  libswiftCore.dylib`type metadata for Any + 8
       rbx = 0x0000000000000000
       rcx = 0x0000000000000000
       rdx = 0x000000000003efe0
       rdi = 0x0000000100435080
       rsi = 0x00000000000bcfa0
       rbp = 0x00007ffeefbff440
       rsp = 0x00007ffeefbff3c0
        r8 = 0x0000000000005d09
        r9 = 0x0000000000000003
       r10 = 0x0000000100500000
       r11 = 0x0000000000000000
       r12 = 0x0000000000000000
       r13 = 0x0000000000000000
       r14 = 0x0000000000000000
       r15 = 0x0000000000000000
       rip = 0x0000000100003c12  LGSwift`main + 178 at main.swift:19:7
    rflags = 0x0000000000000206
        cs = 0x000000000000002b
        fs = 0x0000000000000000
        gs = 0x0000000000000000
```

## image list

> 程序运行加载image

```shell
(lldb) image list
[  0] BB914D93-708D-35F5-BD97-2AEE2D59FC3B 0x0000000100000000 /Users/360jr/Library/Developer/Xcode/DerivedData/LGSwift-fvgrncxxsmdtgkdnysjqhlyzosii/Build/Products/Debug/LGSwift 
[  1] 2705F0D8-C104-3DE9-BEB5-B1EF6E28656D 0x0000000100014000 /usr/lib/dyld 
...
```

