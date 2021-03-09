# 动态库共享缓存

iOS3.1开始，为了提高性能，绝大部分的系统动态库文件都打包到一个缓存文件中（dyld shared cache）

缓存文件路径：`/System/Library/Caches/com.apple.dyld/dyld_shared_cache_armX`

* 从动态共享缓存中抽取动态库

在dyld源码中寻找 dsc_extractor.cpp 编译出可执行文件a.out

usage: ./a.out <path-to-cache-file> <path-to-device-dir>

