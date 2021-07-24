# OC&Block反汇编

## OC方法的本质

OC方法调用，就是调用`objc_msgSend`

* 第一个参数为`self`，放在`x0`里面
* 第二参数为`SEL`，放在`x1`里面

```shell
0x1047160a8 <+24>:  adrp   x8, 7
    0x1047160ac <+28>:  ldr    x0, [x8, #0x560]
    0x1047160b0 <+32>:  adrp   x8, 7
    0x1047160b4 <+36>:  ldr    x1, [x8, #0x540]
->  0x1047160b8 <+40>:  bl     0x1047164e4               ; symbol stub for: objc_msgSend

(lldb) register read x0
      x0 = 0x000000010471d648  (void *)0x000000010471d620: Person
(lldb) register read x1
      x1 = 0x00000001d70bb5df
(lldb) p (SEL)0x00000001d70bb5df
(SEL) $1 = "person"
```

### alloc & init

* `[[self alloc] init]`在新的系统里直接调用`objc_alloc_init`
* 在老的系统里iOS11及以前，是分别调用`objc_alloc`和通过`objc_msgSend`调用`init`，其中`init`的x0并没有发现设置，这是应为x0为`alloc`的返回值，也就是`self`
* `objc_storeStrong`：OC中庸`strong`修饰的变量，都会调用这个函数。编译器优化，函数中默认的变量都是`strong`类型。这个函数既能让对象强引用，又能让对象销毁，结束的时候会调用，就是释放(第二个)

```c++
void			// Person *p = [[Person allo] init];
objc_storeStrong(id *location, id obj) // 这里两个参数，第一个参数相当于临时变量&p , 第二个就是目标obj
{
    id prev = *location; // 保存location指向的旧对象
    if (obj == prev) {  // 判断 旧对象是否是要保持的对象，如果是就返回
        return;
    }
    objc_retain(obj);	// 如果不是，保持新对象，引用计数+1
    *location = obj;	// 让location指向新对象
    objc_release(prev);// 老对象引用计数-1
}
// 当OC方法结束的时候也会调用这个函数，但是x1就传入的是0x0了，相当于objc_storeStrong(&p,nil)，最终就是类似调用objc_release(p)
```



### 工具静态分析Mach-O的方法

源代码：

```objective-c
int main(int argc, char * argv[]) {
    Person * p = [Person person];
    p.name = @"hank";
    p.age = 18;
    return 0;
}
```

Hopper反汇编：

```
00000001000061a4         adrp       x8, #0x10000d000
00000001000061a8         ldr        x0, [x8, #0x558]  ; objc_cls_ref_Person(类),_OBJC_CLASS_$_Person(结构体)，点击可进入
00000001000061ac         adrp       x8, #0x10000d000
00000001000061b0         ldr        x1, [x8, #0x538]  ; "person",@selector(person)
00000001000061b4         bl         imp___stubs__objc_msgSend  ; objc_msgSend 
// 这里可以看出，objc_msgSend的第一个参数是Person类，第二个是sel。
```

* 类地址

```
// 点击类名进入
000000010000d558         dq         _OBJC_CLASS_$_Person ; 左边的地址就是Person类在Mach-O中的地址

// 点击结构体进入 对应的就是结构体的信息
                     _OBJC_CLASS_$_Person:
000000010000d640         struct __objc_class {   ; DATA XREF=_main+28, 0x100008078, objc_cls_ref_Person
                             _OBJC_METACLASS_$_Person,            // metaclass
                             _OBJC_CLASS_$_NSObject,              // superclass
                             __objc_empty_cache,                  // cache
                             0x0,                                 // vtable
                             __OBJC_CLASS_RO_$_Person             // data
                         }
```

![类地址](https://gitee.com/dexport/blog-image/raw/master/img/20210724131121.png)

* 方法名地址

```
// 点击person方法名进入，查看person的cstring位置
000000010000712a         db         "person", 0   ; DATA XREF=_main+36, 0x10000cc30, 0x10000d538
```

![方法名地址](https://gitee.com/dexport/blog-image/raw/master/img/20210724131934.png)

## Block

### block数据结构

```c++
struct Block_layout {
    void * __ptrauth_objc_isa_pointer isa; // isa
    volatile int32_t flags; // contains ref count
    int32_t reserved;
    BlockInvokeFunction invoke; //函数指针
    struct Block_descriptor_1 *descriptor;
    // imported variables
};
```

### global block

> 不引用外部变量，在全局区

源码：

```objective-c
```

Hopper:

```
0000000100006188         adrp       x0, #dyld_stub_binder_100008000   ; 0x100008028@PAGE
000000010000618c         add        x0, x0, #0x28                     ; 0x100008028@PAGEOFF, ___block_literal_global
0000000100006190         bl         imp___stubs__objc_retainBlock     ; objc_retainBlock
```

点击`___block_literal_global`进入：

```
                     ___block_literal_global:
0000000100008028         dq         0x000000010001c040       ; DATA XREF=_main+28 这里是isa
0000000100008030         dd         0x50000000
0000000100008034         dd         0x00000000
0000000100008038         dq         0x00000001000061d4			 ; 这里是invoke 点击进入
0000000100008040         dq         0x0000000100008008
```

block的实现

```
 ___main_block_invoke:
00000001000061d4         sub        sp, sp, #0x20                               ; Objective C Block defined at 0x100008028, DATA XREF=0x100008038
00000001000061d8         stp        x29, x30, [sp, #0x10]
00000001000061dc         add        x29, sp, #0x10
00000001000061e0         str        x0, [sp, #0x10 + var_8]
00000001000061e4         str        x0, [sp, #0x10 + var_10]
00000001000061e8         adrp       x0, #dyld_stub_binder_100008000             ; 0x100008088@PAGE
00000001000061ec         add        x0, x0, #0x88                               ; 0x100008088@PAGEOFF, @"block"
00000001000061f0         bl         imp___stubs__NSLog                          ; NSLog
00000001000061f4         ldp        x29, x30, [sp, #0x10]
00000001000061f8         add        sp, sp, #0x20
00000001000061fc         ret
                        ; endp
```

### stack block

