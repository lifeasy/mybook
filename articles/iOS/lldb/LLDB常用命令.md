# LLDB常用命令

## LLDB语法

```shell
<command> [<subcommand> [<subcommand>...]] <action> [-options [option-value]] [argument[argument...]]
```

- `<command>`（命令）和`<subcommand>`（子命令）：LLDB调试命令的名称
- `<action>`：执行命令的操作
- `<options>`：命令选项
- `<arguement>`：命令的参数
- `[]`：表示命令是可选的，可以有也可以没有

## bt

> 查看函数调用栈 

```shell
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
  * frame #0: 0x000000010012e6bc InjectDemo`-[ViewController test2](self=0x0000000100709bd0, _cmd="test2") at ViewController.m:41:5
    frame #1: 0x000000010012e694 InjectDemo`-[ViewController test1](self=0x0000000100709bd0, _cmd="test1") at ViewController.m:38:5
    ...
```

## up/down

> up、down挪动函数调用 1 个距离

```shell

(lldb) up
frame #1: 0x000000010012e694 InjectDemo`-[ViewController test1](self=0x0000000100709bd0, _cmd="test1") at ViewController.m:38:5
   35  	    [self test1];
   36  	}
   37  	- (void)test1 {
-> 38  	    [self test2];
    	    ^
   39  	}
   40  	- (void)test2 {
   41  	    NSLog(@"haha");
(lldb) down
frame #1: 0x000000010012e694 InjectDemo`-[ViewController test1](self=0x0000000100709bd0, _cmd="test1") at ViewController.m:38:5
   35  	    [self test1];
   36  	}
   37  	- (void)test1 {
-> 38  	    [self test2];
    	    ^
   39  	}
   40  	- (void)test2 {
   41  	    NSLog(@"haha");
```

## frame

> `frame select 数字`——跳转函数调用（0表示当前方法，2表示2个方法前）
>
> `frame variable`——查看当前函数属性：内存地址、方法名、参数

```shell
(lldb) frame select 1
frame #1: 0x000000010012e694 InjectDemo`-[ViewController test1](self=0x0000000100709bd0, _cmd="test1") at ViewController.m:38:5
   35  	    [self test1];
   36  	}
   37  	- (void)test1 {
-> 38  	    [self test2];
    	    ^
   39  	}
   40  	- (void)test2 {
   41  	    NSLog(@"haha");
(lldb) frame variable
(ViewController *) self = 0x0000000100709bd0
(SEL) _cmd = "test1"   
```

## thread

> `thread return`——结束当前函数调用，跳转回上一个调用栈的下一步

## watchpoint

> 通过`watchpoint set variable self->_name`就可以给对应的属性内存下断点
>
> 修改内存地址的值就会来到断点处（此时相当于KVO机制）同时也能看到函数调用栈
>
> watchpoint list
>
> watchpoint delete

```shell
(lldb) watchpoint set variable self->_name
Watchpoint created: Watchpoint 1: addr = 0x100d07eb0 size = 8 state = enabled type = w
    watchpoint spec = 'self->_name'
    new value: 0x0000000000000000

Watchpoint 1 hit:
old value: 0x0000000000000000
new value: 0x00000001006fc0a0
```





## breakpoint 

> 设置断点

| 命令                                                         | 意义                               | 简写                                                     |
| ------------------------------------------------------------ | ---------------------------------- | -------------------------------------------------------- |
| breakpoint list                                              | 查看断点列表                       | breakpoint l                                             |
| breakpoint set -n cMethod                                    | 设置单个断点                       | b -n cMethod                                             |
| breakpoint set -n "[ViewController ocMethod1]"               |                                    |                                                          |
| -n "[ViewController ocMethod2]"                              | 设置一组断点                       | b -n "[ViewController ocMethod1]"                        |
| -n "[ViewController ocMethod2]"                              |                                    |                                                          |
| breakpoint set --selector touchesBegan:withEvent:            | 设置某一个方法的断点               | b -selector touchesBegan:withEvent:                      |
| breakpoint set --file ViewController.m --selector touchesBegan:withEvent: | 设置某文件下某一个方法的断点       | b -f ViewController.m --selector touchesBegan:withEvent: |
| breakpoint set -r ocMethod                                   | 设置所有匹配方法名的断点           | b -r ocMethod                                            |
| breakpoint enable 1                                          | 启用某一组断点                     | breakpoint en 1                                          |
| breakpoint disable 1                                         | 禁用某一组断点                     | breakpoint dis 1                                         |
| breakpoint enable 1.1                                        | 启用某一个断点                     | breakpoint en 1.1                                        |
| breakpoint disable 1.1                                       | 禁用某一个断点                     | breakpoint dis 1.1                                       |
| breakpoint delete                                            | 删除所有断点                       | breakpoint d                                             |
| continue                                                     | 退出LLDB模式                       | c                                                        |
| next                                                         | 单步运行，将子函数当做整体一起执行 | n                                                        |
| stpe                                                         | 单步运行，将子函数当做整体一起执行 | s                                                        |

关于简写：

- `b`：`breakpoint set`
- `l`：`list`
- `-n`：`--name`
- `--f`：`--file`
- `dis`：`disable`
- `en`：`enable`

> ## 断点添加命令
>
> 通过`breakpoint command add 1`就可以给指定断点添加命令 `1`是对应的断点编号，不传默认给所有的加
>
> 通过`DONE`结束断点命令添加
>
> 类似于给程序添加脚本命令，断点执行也可以添加命令，断点到来时就会执行先前的命令了

```shell
(lldb) breakpoint command add 4
Enter your debugger command(s).  Type 'DONE' to end.
> po self
> po temp
> p temp = @"666"
> DONE
po self
....
# 执行断点4之前打印
 po self
<ViewController: 0x10610bc20>
 po temp
hehe
 p temp = @"666"
(NSTaggedPointerString *) $2 = 0xbbc20ccc64e318e6 @"666"

2021-03-13 15:27:29.681323+0800 InjectDemo[5226:1237648] 666
```

## target stop-hook

> `target stop-hook`是一个给所有断点添加命令的指令——在每次stop的时候去执行一些命令（只对breakpoint、watchpoint有效）
>
> target stop-hook list 
>
> target stop-hook disable
>
> target stop-hook delete`/`undisplay 1

```shell
(lldb) target stop-hook add
Enter your stop hook command(s).  Type 'DONE' to end.
> frame variable
> DONE
Stop hook #1 added.
....
(ViewController *) self = 0x0000000115f076a0
(SEL) _cmd = "test1"

(ViewController *) self = 0x0000000115f076a0
(SEL) _cmd = "test2"
```

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

## memory read 

> memory read 内存地址
>
> 读取内存中的值

## dis -s 

>  dis -s 内存地址
>
> 查看汇编

## image list

> 程序运行加载image

```shell
(lldb) image list
[  0] BB914D93-708D-35F5-BD97-2AEE2D59FC3B 0x0000000100000000 /Users/360jr/Library/Developer/Xcode/DerivedData/LGSwift-fvgrncxxsmdtgkdnysjqhlyzosii/Build/Products/Debug/LGSwift 
[  1] 2705F0D8-C104-3DE9-BEB5-B1EF6E28656D 0x0000000100014000 /usr/lib/dyld 
...
```

