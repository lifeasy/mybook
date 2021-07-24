# 苹果证书相关

## iOS代码签名

1. 简单签名，不适合多渠道（debug、企业发布...）
2. 双向签名

p12：本地公私钥

![双向签名](https://gitee.com/dexport/blog-image/raw/master/img/20210724155114.png)

3. 描述文件

上述不能限制设备的安装数量，权限等等，所以有了描述文件

描述文件（Provisioning profile）一般包括三样东西：证书、App ID、设备。当我们在真机运行或者打包一个项目的时候，证书用来证明我们程序的安全性和合法性。

```shell
# 查看描述文件
$ security cms -D -i embedded.mobileprovision
# 导出描述文件到一个plist文件
$ security cms -D -i embedded.mobileprovision > temp.plist
```

![描述文件](https://gitee.com/dexport/blog-image/raw/master/img/20210724160039.png)

