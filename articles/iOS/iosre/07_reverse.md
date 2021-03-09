# iOS逆向思路

## 一、App从代码到安装流程

源代码-->编译、链接、签名-->prj.app-->zip压缩-->prj.ipa(.app放入Payload中在进行zip压缩)-->上传App Store --> 加密（加壳） -->安装

## 二、逆向思路

* 界面分析
  * Cycript、Reveal
* 代码分析
  * Mach-O文件静态分析
  * MachOView、class-dump、Hopper Disassembler、ida
* 动态调试
  * 对运行中的App进行代码调试
  * debugserver、LLDB
* 代码注入
  * 注入代码到App中
  * 重签名