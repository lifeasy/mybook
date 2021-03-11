# LLVM编译

> 参考自：[LLVM编译踩坑](https://note.youdao.com/ynoteshare1/index.html?id=3295dab53f628dc0684a12ec39526738&type=note?auto)

## 获取源码

```shell
git clone --depth 1 https://github.com/llvm/llvm-project.git
```

## 配置和构建 LLVM 和 Clang

```shell
# 向~/.zshrc文件导入 OSX_COMMANDLINE_SDKROOT变量（指定为 # "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"）
$ echo 'export OSX_COMMANDLINE_SDKROOT="/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"' >> ~/.zshrc 
$ source ~/.zshrc
```

如果`/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk` 路径不存在，则重新安装`CommandLineTools`

```shell
xcode-select --install
```

或者直接官网下载

## 安装Cmake

```shell
brew install cmake 
```

## 编译

```shell
# 参数 2 代表电脑 CPU 核心数，自行查看
cmake -G Xcode -j 2 -DLLVM_ENABLE_PROJECTS='libcxx;libc++;clang;lldb' -DLLDB_USE_SYSTEM_DEBUGSERVER=ON -DLLDB_TEST_COMPILER=clang++ -DCMAKE_OSX_SYSROOT=$OSX_COMMANDLINE_SDKROOT ../llvm
```

* 编译出现CMakeCache.txt文件报错、表示已经编译过、出现了缓存文件。去 /llvm-Xcode/llvm-project/build/ 目录下删除CMakeCache.txt即可
* 编译完成后的项目打开项目： 选择Manually 方式、Automatically方式在增删target的时候会很卡很卡 

