# Swift源码编译

## Clone源码

注意源码与本地Xcode版本，不对应的话会编译失败

```shell
# 查看本机swift版本
$ xcrun swift -version
Apple Swift version 5.3.1 (swiftlang-1200.0.41 clang-1200.0.32.8)
Target: x86_64-apple-darwin20.1.0
```

```shell
$ mkdir swift-source
$ cd swift-source
$ git clone --branch swift-5.3.1-RELEASE https://github.com/apple/swift.git
....
# 等待clone完成

# 克隆swift编译相关的库
$ ./swift/utils/update-checkout --tag swift-5.3.1-RELEASE --clone
...
# 等待clone完成


```

* 遇到ascii相关报错，修改路径，不能出现中文
* 遇到部分依赖库clone出现SSL链接失败，使用翻墙解决

## 编译

```shell
$ ./swift/utils/build-script -r --debug-swift-stdlib --lldb
```

