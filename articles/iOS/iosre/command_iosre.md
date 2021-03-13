#  iOS逆向常用命令

##security 

```shell
# 列出电脑上可用证书
$ security find-identity -v -p codesigning
  1) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
  2) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
  3) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
  4) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
     4 valid identities found
     
# 系统已安装描述文件路径
$ ~/Library/MobileDevice/Provisioning Profiles

# 查看描述文件
$ security cms -Di xxx.mobileprovision

# 从embedded.mobileprovision文件中提取出entitlements.plist权限文件。
$ security cms -D -i embedded.mobileprovision > temp.plist
$ /usr/libexec/PlistBuddy -x -c 'Print :Entitlements' temp.plist > entitlements.plist
```

## codesign

```shell
# 用证书签名
codesign --force --verify --verbose --sign "找到的可用证书名称" dumpdecrypted.dylib
# 查看app签名信息
$ codesign -vv -d Payload/WeChat.app
Executable=/Users/360jr/Desktop/iOSRe/tool/demo/WeChat/Payload/WeChat.app/WeChat
Identifier=com.tencent.xin
# 重签名
# codesign -fs "（证书名称或者id）" （FrameWork名称）
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" mars.framework

```

## ldid

```shell
# 导出原有权限文件
$ ldid -e debugserver > debugserver.entitlements

# 使用权限文件重签名
$ ldid -Sdebugserver.entitlements debugserver

```