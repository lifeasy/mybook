# Swift源码编译

## Clone源码

注意源码与本地Xcode版本，不对应的话会编译失败

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

