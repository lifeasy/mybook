# 类

对象-isa->类-isa->元类-isa->根NSObject元类

类对象的地址在编译时就确定了，使用MachOView分析，可以直接在Section64(\_DATA,\_Objc_classrefs)和Section64(\_DATA,\_objc_classlist)中可以看到地址

在符号表中也可以看到元类的符号，都是这编译期有编译器生成



isa流程图 objc_getClass

superclass流程 objc_getSuperclass



对象的结构：isa，实例变量

类似的结构：isa （8）、superclass（8）、cache（16）、bits

cache类型：cache_t，类型大小16字节

通过类地址平移0x20获得bits

对bits进行类型强转（class_data_bits_t *）0x0000000

调用data（）方法，0x00000->data()



```shell
# 打印对象内存信息
(lldb) x/8g p
0x10122abc0: 0x011d80010000838d 0x0000000000000000
0x10122abd0: 0x0000000000000000 0x0000000000000000
0x10122abe0: 0x0000000000000009 0x00007fff30010ad8
0x10122abf0: 0x00007fff3e2db6e0 0x00007fff3ccd8c98

# 获取对象isa指针
(lldb) p/x 0x011d80010000838d & 0x00007ffffffffff8ULL
(long) $2 = 0x0000000100008388

# bits相对类对象首地址偏移0x20 isa地址加20后进行bits类型强转
(lldb) p (class_data_bits_t *)0x00000001000083a8
(class_data_bits_t *) $5 = 0x00000001000083a8

# 调用bits的data()方法，获取class_rw_t * 
(lldb) p $5->data()
(class_rw_t *) $7 = 0x0000000101234810

# 从class_rw_t中获取methods
(lldb) p $7->methods()
(const method_array_t) $8 = {
  list_array_tt<method_t, method_list_t, method_list_t_authed_ptr> = {
     = {
      list = {
        ptr = 0x00000001000080b8
      }
      arrayAndFlag = 4295000248
    }
  }
}
# 从methods中获取method_list_t
(lldb) p $8.list.ptr
(method_list_t *const) $9 = 0x00000001000080b8
(lldb) p *$9
(method_list_t) $10 = {
  entsize_list_tt<method_t, method_list_t, 4294901763, method_t::pointer_modifier> = (entsizeAndFlags = 27, count = 13)
}
# 从method_list_t中获取method_t
(lldb) p $10.get(0).big()
(method_t::big) $11 = {
  name = "bycycle"
  types = 0x0000000100003f75 "s16@0:8"
  imp = 0x0000000100003cf0 (KCObjcBuild`-[Person bycycle] at main.m:25)
}


# 从class_rw_t中获取properties
(lldb) p $7->properties()
(const property_array_t) $14 = {
  list_array_tt<property_t, property_list_t, RawPtr> = {
     = {
      list = {
        ptr = 0x00000001000082c0
      }
      arrayAndFlag = 4295000768
    }
  }
}
# 从properties获取 property_list_t
(lldb) p $14.list.ptr
(property_list_t *const) $15 = 0x00000001000082c0
(lldb) p *$15
(property_list_t) $16 = {
  entsize_list_tt<property_t, property_list_t, 0, PointerModifierNop> = (entsizeAndFlags = 16, count = 6)
}
# 从property_list_t中获取property_t
(lldb) p $16.get(0)
(property_t) $17 = (name = "name", attributes = "Tc,N,V_name")

```





