# Hook

## Method Swizzle



## FishHook

> 重新绑定符号表
>
> 通过修改懒加载和非懒加载两个表的指针达到C函数HOOK的目的

### 流程

```objective-c
NSLog(@"123");
//创建rebinding 结构体
struct rebinding nslog;
nslog.name = "NSLog";
nslog.replacement = my_NSLog;
//保存NSLog系统函数地址的指针!
nslog.replaced = (void *)&sys_nslog;
//需求:HOOK NSLog
struct rebinding bds[] = {nslog};
rebind_symbols(bds, 1);
NSLog(@"end");
```



1. 程序编译之后，在Mach-O中有懒加载和非懒加载两张表。

![](https://gitee.com/dexport/blog-image/raw/master/img/20210725123449.png)

这两张表中记录了需要bind的符号，我们这里演示的`NSLog`属于懒加载符号表，在使用的时候才进行绑定

2. 在首次执行`NSLog`之前，先通过Mach-O和程序的ASLR查看内存中的存储值：

* 通过lldb查看ASLR

```shell
# slide 为 0x25ac000
(lldb) image list
[  0] B5C04260-B346-3AEE-B640-D648C77F2D87 0x00000001025ac000 /Users/360jr/Library/Developer/Xcode/DerivedData/001--fishhookDemo-enjeubscnnocrabjzuikobbkipgr/Build/Products/Debug-iphoneos/001--fishhookDemo.app/001--fishhookDemo 
```

* Mach-O中找到NSLog的函数地址信息：

![符号信息](https://gitee.com/dexport/blog-image/raw/master/img/20210725124058.png)

* 通过通过程序起始地址`0x00000001025ac000`加上Offset`0xc000`，就的到NSLog符号在运行时的内存地址，查看内存空间

```shell
(lldb) x/g 0x00000001025ac000+0xc000
0x1025b8000: 0x00000001025b24ec
```

* 发现与Mach-O中的Data不一样，原因是因为这个Data其实是一个Mach-O中的函数地址，在dyld加载阶段进行了rebase，所有使用slide机上Data中的地址，即可的到相同的运行时的地址

```shell
(lldb) p/x 0x1000064ec + 0x25ac000
(long) $5 = 0x00000001025b24ec
```

* NSLog在第一次执行的会后，会跳到`0x00000001025b24ec`这个地址指向的函数，执行绑定工作，过掉第一个NSLog之后，会发现Data的值发生了改变

```shell
(lldb) x/g 0x00000001025ac000+0xc000
0x1025b8000: 0x000000018932b3c4
```

* 查看汇编，发现已经变成了NSLog的地址

```shell
(lldb) dis -s 0x000000018932b3c4
Foundation`NSLog:
    0x18932b3c4 <+0>:  pacibsp 
    0x18932b3c8 <+4>:  sub    sp, sp, #0x20             ; =0x20 
    0x18932b3cc <+8>:  stp    x29, x30, [sp, #0x10]
    0x18932b3d0 <+12>: add    x29, sp, #0x10            ; =0x10 
    0x18932b3d4 <+16>: adrp   x8, 347853
    0x18932b3d8 <+20>: ldr    x8, [x8, #0xe0]
    0x18932b3dc <+24>: ldr    x8, [x8]
    0x18932b3e0 <+28>: str    x8, [sp, #0x8]
```

* 然后同样的方法，过掉fishhook之后的NSLog，发现地址有变化了，成为了我们自定义的地址

```shell
(lldb) x/g 0x00000001025ac000+0xc000
0x1025b8000: 0x00000001025b1614

(lldb) dis -s 0x00000001025b1614
001--fishhookDemo`my_NSLog:
    0x1025b1614 <+0>:  sub    sp, sp, #0x30             ; =0x30 
    0x1025b1618 <+4>:  stp    x29, x30, [sp, #0x20]
    0x1025b161c <+8>:  add    x29, sp, #0x20            ; =0x20 
    0x1025b1620 <+12>: sub    x8, x29, #0x8             ; =0x8 
    0x1025b1624 <+16>: mov    x9, #0x0
    0x1025b1628 <+20>: stur   xzr, [x29, #-0x8]
    0x1025b162c <+24>: str    x0, [sp, #0x10]
    0x1025b1630 <+28>: mov    x0, x8
```

### 汇编层面分析最终流程

源代码：

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"123");
    NSLog(@"end");
}
```

* 断点在第一个NSLog，进入汇编代码查看，会发现跳转到一个symbol stub的地址，通过lldb获取程序起始地址`0x00000001005fc000`

```
0x100601e38 <+68>: bl     0x1006024d4               ; symbol stub for: NSLog

// 相减获取在macho中的偏移值
(lldb) p/x 0x1006024d4-0x00000001005fc000
(long) $1 = 0x00000000000064d4
```

* macho中查看地址信息

![](https://gitee.com/dexport/blog-image/raw/master/img/20210725135449.png)

* 这里的Data实际上是代码，通过汇编进入调试查看代码，发现最终跳转了一个地址，查看x16的地址

```
// 这段代码的意思就是去执行懒加载符号表中的地址
001--fishhookDemo`NSLog:
->  0x1006024d4 <+0>: nop    
    0x1006024d8 <+4>: ldr    x16, #0x5b28              ; (void *)0x0000000100602588
    0x1006024dc <+8>: br     x16
    
(lldb) register read x16
     x16 = 0x0000000100602588  
// 获取macho中的偏移地址     
(lldb) p/x 0x0000000100602588-0x00000001005fc000
(long) $2 = 0x0000000000006588
```

![](https://gitee.com/dexport/blog-image/raw/master/img/20210725142357.png)

* 发现这个Symbol Stubs代码执行后跳转的是`0x0000000000006588`

![](https://gitee.com/dexport/blog-image/raw/master/img/20210725140209.png)

* `0x0000000000006588`在stub helper中执行两行代码，最终跳转到0x6579处执行代码，发现，最终都跳转到0x100008000处。**这里的外部符号执行代码最终都是跳转到相同的地址**
* 查看`0x8000`处，执行的是一个非懒加载绑定的`dyld_stub_binder`符号，非懒加载符号即是在dyld加载阶段就进行绑定的，这个函数执行后，懒加载符号表中的地址就被改变，下一次调用的时候，经过stub函数，直接就跳转到绑定后的位置了

![](https://gitee.com/dexport/blog-image/raw/master/img/20210725140641.png)

### 使用

```objective-c
//创建rebinding 结构体
    struct rebinding nslog;
    nslog.name = "NSLog";
    nslog.replacement = my_NSLog;
    //保存NSLog系统函数地址的指针!
    nslog.replaced = (void *)&sys_nslog;

    //需求:HOOK NSLog
    struct rebinding bds[] = {nslog};

    rebind_symbols(bds, 1);
```

### 源码解析

```c++
// Hook的结构体
struct rebinding {
  const char *name;//需要HOOK的函数名称，C字符串
  void *replacement;//新函数的地址
  void **replaced;//原始函数地址的指针！
};

// fishhook.c

#include "fishhook.h"
#include <stdio.h>

#include <dlfcn.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <mach-o/dyld.h>
#include <mach-o/loader.h>
#include <mach-o/nlist.h>

#ifdef __LP64__
typedef struct mach_header_64 mach_header_t;
typedef struct segment_command_64 segment_command_t;
typedef struct section_64 section_t;
typedef struct nlist_64 nlist_t;
#define LC_SEGMENT_ARCH_DEPENDENT LC_SEGMENT_64
#else
typedef struct mach_header mach_header_t;
typedef struct segment_command segment_command_t;
typedef struct section section_t;
typedef struct nlist nlist_t;
#define LC_SEGMENT_ARCH_DEPENDENT LC_SEGMENT
#endif

#ifndef SEG_DATA_CONST
#define SEG_DATA_CONST  "__DATA_CONST"
#endif

struct rebindings_entry {
  struct rebinding *rebindings;
  size_t rebindings_nel;
  struct rebindings_entry *next;
};

static struct rebindings_entry *_rebindings_head;

static int prepend_rebindings(struct rebindings_entry **rebindings_head,
                              struct rebinding rebindings[],
                              size_t nel) {
  struct rebindings_entry *new_entry = (struct rebindings_entry *) malloc(sizeof(struct rebindings_entry));
  if (!new_entry) {
    return -1;
  }
  new_entry->rebindings = (struct rebinding *) malloc(sizeof(struct rebinding) * nel);
  if (!new_entry->rebindings) {
    free(new_entry);
    return -1;
  }
  memcpy(new_entry->rebindings, rebindings, sizeof(struct rebinding) * nel);
  new_entry->rebindings_nel = nel;
  new_entry->next = *rebindings_head;
  *rebindings_head = new_entry;
  return 0;
}

static void perform_rebinding_with_section(struct rebindings_entry *rebindings,
                                           section_t *section,
                                           intptr_t slide,
                                           nlist_t *symtab,
                                           char *strtab,
                                           uint32_t *indirect_symtab) {
    //nl_symbol_ptr和la_symbol_ptrsection中的reserved1字段指明对应的indirect symbol table起始的index
  uint32_t *indirect_symbol_indices = indirect_symtab + section->reserved1;
    //slide+section->addr 就是符号对应的存放函数实现的数组也就是我相应的__nl_symbol_ptr和__la_symbol_ptr相应的函数指针都在这里面了，所以可以去寻找到函数的地址
  void **indirect_symbol_bindings = (void **)((uintptr_t)slide + section->addr);
    //遍历section里面的每一个符号
  for (uint i = 0; i < section->size / sizeof(void *); i++) {
      //找到符号在Indrect Symbol Table表中的值
      //读取indirect table中的数据
    uint32_t symtab_index = indirect_symbol_indices[i];
    if (symtab_index == INDIRECT_SYMBOL_ABS || symtab_index == INDIRECT_SYMBOL_LOCAL ||
        symtab_index == (INDIRECT_SYMBOL_LOCAL   | INDIRECT_SYMBOL_ABS)) {
      continue;
    }
      //以symtab_index作为下标，访问symbol table
      uint32_t strtab_offset = symtab[symtab_index].n_un.n_strx;
      //获取到symbol_name
      char *symbol_name = strtab + strtab_offset;
      //判断是否函数的名称是否有两个字符，为啥是两个，因为函数前面有个_，所以方法的名称最少要1个
      bool symbol_name_longer_than_1 = symbol_name[0] && symbol_name[1];
      //遍历最初的链表，来进行hook
      struct rebindings_entry *cur = rebindings;
      while (cur) {
          for (uint j = 0; j < cur->rebindings_nel; j++) {
              //这里if的条件就是判断从symbol_name[1]两个函数的名字是否都是一致的，以及判断两个
              if (symbol_name_longer_than_1 &&
                  strcmp(&symbol_name[1], cur->rebindings[j].name) == 0) {
                  //判断replaced的地址不为NULL以及我方法的实现和rebindings[j].replacement的方法不一致
                  if (cur->rebindings[j].replaced != NULL &&
                      indirect_symbol_bindings[i] != cur->rebindings[j].replacement) {
                      //让rebindings[j].replaced保存indirect_symbol_bindings[i]的函数地址
                      *(cur->rebindings[j].replaced) = indirect_symbol_bindings[i];
                  }
                  //将替换后的方法给原先的方法，也就是替换内容为自定义函数地址
                  indirect_symbol_bindings[i] = cur->rebindings[j].replacement;
                  goto symbol_loop;
        }
      }
      cur = cur->next;
    }
  symbol_loop:;
  }
}
//回调的最终就是这个函数！ 三个参数：要交换的数组  、 image的头 、 ASLR的偏移
static void rebind_symbols_for_image(struct rebindings_entry *rebindings,
                                     const struct mach_header *header,
                                     intptr_t slide) {
    
    /*dladdr() 可确定指定的address 是否位于构成进程的进址空间的其中一个加载模块（可执行库或共享库）内，如果某个地址位于在其上面映射加载模块的基址和为该加载模块映射的最高虚拟地址之间（包括两端），则认为该地址在加载模块的范围内。如果某个加载模块符合这个条件，则会搜索其动态符号表，以查找与指定的address 最接近的符号。最接近的符号是指其值等于，或最为接近但小于指定的address 的符号。
     */
    /*
     如果指定的address 不在其中一个加载模块的范围内，则返回0 ；且不修改Dl_info 结构的内容。否则，将返回一个非零值，同时设置Dl_info 结构的字段。
     如果在包含address 的加载模块内，找不到其值小于或等于address 的符号，则dli_sname 、dli_saddr 和dli_size字段将设置为0 ； dli_bind 字段设置为STB_LOCAL ， dli_type 字段设置为STT_NOTYPE 。
     */
    //这个dladdr函数就是在程序里面找header
  Dl_info info;
  if (dladdr(header, &info) == 0) {
    return;
  }
    //下面就是定义好几个变量，准备从MachO里面去找！
  segment_command_t *cur_seg_cmd;
  segment_command_t *linkedit_segment = NULL;
  struct symtab_command* symtab_cmd = NULL;
  struct dysymtab_command* dysymtab_cmd = NULL;
    //跳过header的大小，找loadCommand
  uintptr_t cur = (uintptr_t)header + sizeof(mach_header_t);
  for (uint i = 0; i < header->ncmds; i++, cur += cur_seg_cmd->cmdsize) {
    cur_seg_cmd = (segment_command_t *)cur;
    if (cur_seg_cmd->cmd == LC_SEGMENT_ARCH_DEPENDENT) {
      if (strcmp(cur_seg_cmd->segname, SEG_LINKEDIT) == 0) {
        linkedit_segment = cur_seg_cmd;
      }
    } else if (cur_seg_cmd->cmd == LC_SYMTAB) {
      symtab_cmd = (struct symtab_command*)cur_seg_cmd;
    } else if (cur_seg_cmd->cmd == LC_DYSYMTAB) {
      dysymtab_cmd = (struct dysymtab_command*)cur_seg_cmd;
    }
  }
   //如果刚才获取的，有一项为空就直接返回
  if (!symtab_cmd || !dysymtab_cmd || !linkedit_segment ||
      !dysymtab_cmd->nindirectsyms) {
    return;
  }

  // Find base symbol/string table addresses
//链接时程序的基址 = __LINKEDIT.VM_Address -__LINKEDIT.File_Offset + silde的改变值
  uintptr_t linkedit_base = (uintptr_t)slide + linkedit_segment->vmaddr - linkedit_segment->fileoff;
//    printf("地址:%p\n",linkedit_base);
    //符号表的地址 = 基址 + 符号表偏移量
  nlist_t *symtab = (nlist_t *)(linkedit_base + symtab_cmd->symoff);
     //字符串表的地址 = 基址 + 字符串表偏移量
  char *strtab = (char *)(linkedit_base + symtab_cmd->stroff);

  // Get indirect symbol table (array of uint32_t indices into symbol table)
    //动态符号表地址 = 基址 + 动态符号表偏移量
  uint32_t *indirect_symtab = (uint32_t *)(linkedit_base + dysymtab_cmd->indirectsymoff);

  cur = (uintptr_t)header + sizeof(mach_header_t);
  for (uint i = 0; i < header->ncmds; i++, cur += cur_seg_cmd->cmdsize) {
    cur_seg_cmd = (segment_command_t *)cur;
    if (cur_seg_cmd->cmd == LC_SEGMENT_ARCH_DEPENDENT) {
        //寻找到data段
      if (strcmp(cur_seg_cmd->segname, SEG_DATA) != 0 &&
          strcmp(cur_seg_cmd->segname, SEG_DATA_CONST) != 0) {
        continue;
      }
        
      for (uint j = 0; j < cur_seg_cmd->nsects; j++) {
        section_t *sect =
          (section_t *)(cur + sizeof(segment_command_t)) + j;
          //找懒加载表
        if ((sect->flags & SECTION_TYPE) == S_LAZY_SYMBOL_POINTERS) {
          perform_rebinding_with_section(rebindings, sect, slide, symtab, strtab, indirect_symtab);
        }
          //非懒加载表
        if ((sect->flags & SECTION_TYPE) == S_NON_LAZY_SYMBOL_POINTERS) {
          perform_rebinding_with_section(rebindings, sect, slide, symtab, strtab, indirect_symtab);
        }
      }
    }
  }
}

static void _rebind_symbols_for_image(const struct mach_header *header,
                                      intptr_t slide) {
    rebind_symbols_for_image(_rebindings_head, header, slide);
}

int rebind_symbols_image(void *header,
                         intptr_t slide,
                         struct rebinding rebindings[],
                         size_t rebindings_nel) {
    struct rebindings_entry *rebindings_head = NULL;
    int retval = prepend_rebindings(&rebindings_head, rebindings, rebindings_nel);
    rebind_symbols_for_image(rebindings_head, (const struct mach_header *) header, slide);
    if (rebindings_head) {
      free(rebindings_head->rebindings);
    }
    free(rebindings_head);
    return retval;
}

int rebind_symbols(struct rebinding rebindings[], size_t rebindings_nel) {
    //prepend_rebindings的函数会将整个 rebindings 数组添加到 _rebindings_head 这个链表的头部
    //Fishhook采用链表的方式来存储每一次调用rebind_symbols传入的参数，每次调用，就会在链表的头部插入一个节点，链表的头部是：_rebindings_head
    int retval = prepend_rebindings(&_rebindings_head, rebindings, rebindings_nel);
    //根据上面的prepend_rebinding来做判断，如果小于0的话，直接返回一个错误码回去
    if (retval < 0) {
    return retval;
  }
    //根据_rebindings_head->next是否为空判断是不是第一次调用。
  if (!_rebindings_head->next) {
      //第一次调用的话，调用_dyld_register_func_for_add_image注册监听方法.
      //已经被dyld加载的image会立刻进入回调。
      //之后的image会在dyld装载的时候触发回调。
    _dyld_register_func_for_add_image(_rebind_symbols_for_image);
  } else {
      //遍历已经加载的image，进行的hook
    uint32_t c = _dyld_image_count();
    for (uint32_t i = 0; i < c; i++) {
      _rebind_symbols_for_image(_dyld_get_image_header(i), _dyld_get_image_vmaddr_slide(i));
    }
  }
  return retval;
}

```





## inlineHook





## Cydia Substrate



