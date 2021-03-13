# 重签名

## 一、iOS App 签名

> 参考：[iOS App 签名的原理](http://blog.cnbang.net/tech/3386/)

> 代码签名是对可执行文件或脚本进行数字签名，用来确认软件在签名后未被修改或损坏的措施。和数字签名原理一样，只不过签名的数据是代码而已

### 1.简单的代码签名

![img](15_resign.assets/1)

1. 苹果官方生成一对非对称加密的公私钥，在iOS的系统中内置一个公钥，私钥由苹果后台保存；
2. 开发者上传App到App Store时，苹果后台用私钥对App数据进行签名（即先进行Hash得到hash值，再用私钥加密hash值得到“RSAHash”）；
3. iOS系统下载这个App后，用内置的公钥验证这个签名，若签名正确，那么这个App肯定是由苹果后台认证的，并且没有被修改过，也就达到了苹果的需求:保证安装的每一个APP都是经过苹果官方允许的。（公钥解密“RSAHash”得到hash1，再对应用包进行相同hash算法得到hash2,验证两次hash值是否相同）

### 2.双层签名

1) 安装包不需要上传到App Store，可以直接安装到手机上

* 开发App时直接真机调试安装
* 企业内部分发的渠道企业证书签名的APP也是需要顺利安装的

2) 苹果为了保证系统的安全性,又必须对安装的APP有绝对的控制权

* 经过苹果允许才可以安装
* 不能被滥用导致非开发APP也能被安装

![img](15_resign.assets/1-20210312151851022)

1）苹果自己有固定的一对公私钥，跟之前App Store原理一样，私钥存储在苹果后台，公钥放在每个iOS系统中。这里称为`公钥A`、`私钥A`；在Mac系统中创建`CSR`文件时生成非对称加密算法的一对公钥/私钥，这里称为`公钥M`、`私钥M`；A=Apple M = Mac

2）开发者通过`CSR`文件向开发者中心申请`证书`

3）苹果服务器生成`证书`，也就是我们平时所说的`开发者证书`，其原理就是用`私钥A`对`公钥M`非对称加密。请求到`证书`之后，`钥匙串访问`就会将`证书`与本地`私钥M`（ 就是我们所熟知的`p12`） 进行相关联

4）当我们`Command+B`生成一个应用时，Xcode就会用本地的`私钥M`对这个应用进行签名，同时将证书放到app里面打包成ipa包

5）iPhone手机安装ipa包

6）iOS系统里的`公钥A`对证书进行验证

7）验证通过拿到`证书`中的`公钥M`

8）`公钥M`验证app的签名。这里就间接验证了这个APP的安装行为是否经过苹果官方允许（这里只验证安装行为，不验证APP是否被改动，因为开发阶段 APP 内容总是不断变化的，苹果不需要管）

简化流程：

![img](15_resign.assets/1-20210312152424192)

CSR文件和公钥M的关系：

当CSR文件创建的时候，会自动生成一对私钥和公钥，私钥存储在Mac上，私钥存储默认存储在登录钥匙串中，可以在钥匙串的分类钥匙下查看，请求到的证书会包含公钥部分

### 3. 描述文件

苹果为了解决应用滥用的问题，所以苹果又加了两个限制

- 在苹果后台注册过的设备才可以安装
- 签名只能针对某一个具体的App，并且苹果还想控制App里面的iCloud/PUSH/后台运行/调试器附加这些权限，所以苹果把这些权限开关统一称为Entitlements（授权文件）

因此才有了Provisioning Profile（描述文件）

![img](15_resign.assets/1-20210312152707835)

描述文件一般包括证书、AppID、设备。当我们在真机运行或者打包一个项目的时候，证书用来证明我们程序的安全性和合法性

描述文件是在AppleDevelop网站创建的（在Xcode中填上AppleID它会进行创建），Xcode运行时会打包进入App内。所以我们使用CSR申请证书时,我们还要申请一个东西！！就是描述文件！

在开发时，编译完一个App后，用本地的私钥M对这个App进行签名，同时把从苹果服务器得到的 Provisioning Profile文件打包进APP里，文件名为embedded.mobileprovision，把 APP 安装到手机上，最后系统进行验证

```shell
# 系统已安装描述文件路径
$ ~/Library/MobileDevice/Provisioning Profiles
# 查看描述文件
$ security cms -Di xxx.mobileprovision
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>AppIDName</key>
	<string>XC Wildcard</string>
	<key>ApplicationIdentifierPrefix</key>
	<array>
	<string>A572RLB7UB</string>
	</array>
	<key>CreationDate</key>
	<date>2021-02-22T03:59:53Z</date>
	<key>Platform</key>
	....
```

p12格式为本地私钥，可以导入到其他电脑，用于团队开发。

## 二、重签名

### 1. 重签名前期准备

如果需要将破坏了签名的安装包，安装到非越狱的手机上，需要对安装包进行重签名的操作。

**进行重签名的包，必须是未加壳的。**

1）查看app的签名信息

```shell
$ codesign -vv -d Payload/WeChat.app
Executable=/Users/360jr/Desktop/iOSRe/tool/demo/WeChat/Payload/WeChat.app/WeChat
Identifier=com.tencent.xin
Format=app bundle with Mach-O thin (arm64)
CodeDirectory v=20500 size=3385627 flags=0x0(none) hashes=52895+7 location=embedded
Signature size=4390
Authority=Apple iPhone OS Application Signing
Authority=Apple iPhone Certification Authority
Authority=Apple Root CA
Info.plist entries=77
TeamIdentifier=88L2Q4487U
Sealed Resources version=2 rules=25 files=2652
Internal requirements count=1 size=96
```

2）列出电脑上的可用证书

```shell
$ security find-identity -v -p codesigning
  1) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
  2) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
  3) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
  4) 123xxxxxxxxxx456 "Apple Development: xxx (2342xxxx)"
     4 valid identities found
```

### 2. 重签名步骤

#### 1.删除插件和带有插件的.app包（比如Watch，PlugIns）

因为普通账号不能对插件进行签名，所以需要删除

在`包内容`路径中找到`Watch`和`PlugIns`文件夹直接删除

#### 2.重签framework

```shell
# 进入FrameWork目录
$ cd Frameworks
# 列举当前FrameWork
$ ls
# 覆盖签名
# codesign -fs "（证书名称或者id）" （FrameWork名称）
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" mars.framework
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" marsbridgenetwork.framework
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" matrixreport.framework
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" OpenSSL.framework
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" ProtobufLite.framework
$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" andromeda.framework
```

#### 3.给MachO上可执行权限

在`包内容`路径下执行`$ Chmod +x WeChat`

#### 4.添加描述文件

新建工程，真机运行即可得到描述文件

将ipa包中的`描述文件`放到微信的`包内容`中去

#### 5.修改应用包的bundid

修改微信`包内容`中的info.plist->BundleId，改为与`描述文件`的BundleId一致

#### 6.授权文件重签app包

从`embedded.mobileprovision`文件中提取出`entitlements.plist`权限文件。

```shell
$ security cms -D -i embedded.mobileprovision > temp.plist
$ /usr/libexec/PlistBuddy -x -c 'Print :Entitlements' temp.plist > entitlements.plist
```

用生成的权限文件签名app包

```shell
# $ codesign -fs 证书ID或名称 --entitlements entitlements.plist xxx.app
$ codesign -fs BC4FF0F29******A5271F71D64894B60 --entitlements entitlements.plist CodesignApp.app

$ codesign -fs "iPhone Developer: 840385400@qq.com (NZJQGFWAYE)" --no-strict --entitlements=ent.plist WeChat.app

```

#### 7.安装.app包

Xcode界面下`shift+cmd+2`调出手机设备

![img](15_resign.assets/1-20210312155848835)

如果出现安装失败，删除之前app在重新安装

app运行后利用Xcode附加进程

![img](15_resign.assets/1-20210312155909530)

### 3. GUI工具一步达成

只需要将`embedded.mobileprovision`拷贝到`.app`包内。然后，采用[iOS App Signer](https://github.com/DanTheMan827/ios-app-signer)来一步达成。

![img](15_resign.assets/065fa9dd585e4746b0054c81181bfcf7~tplv-k3u1fbpfcp-zoom-1.image)

- 只要在Input File中输入.app文件路径，只针对.app包签名，.app内部动态库需要单独签名。
- [iReSign](https://github.com/maciekish/iReSign) 与该功能类似，但是操作比上面多。
- 签名完成之后会导出`ipa`包

### 4. xcode重签名

**必须是与MachO文件同名工程（工程名相同，BundId无所谓），否则进程不是微信进程**

#### 1. 运行app，装描述文件

新建`WeChat`工程，同上`Command+Run`，将描述文件生成一下

#### 2. 替换掉app包

复制一个新的越狱包，覆盖掉到原来的ipa包

![img](15_resign.assets/1-20210312163249103)

#### 3. 删除插件、Watch

#### 4. 重签framework

#### 5. run

由于是Xcode直接运行，可以通过Debug View查看界面

## 三、脚本重签名

### 1. Xcode新建任意工程，找到添加脚本位置

![img](15_resign.assets/1-20210312163503068)

### 2.准备资源文件夹

在项目目录下创建`APP`文件夹并放入`越狱ipa包`

![img](15_resign.assets/1-20210312163524542)

### 3.1 新建脚本文件 app.sh

```shell
# ${SRCROOT} 它是工程文件所在的目录
TEMP_PATH="${SRCROOT}/Temp"
#资源文件夹，我们提前在工程目录下新建一个APP文件夹，里面放ipa包
ASSETS_PATH="${SRCROOT}/APP"
#目标ipa包路径
TARGET_IPA_PATH="${ASSETS_PATH}/*.ipa"
#清空Temp文件夹
rm -rf "${SRCROOT}/Temp"
mkdir -p "${SRCROOT}/Temp"

#----------------------------------------
# 1. 解压IPA到Temp下
unzip -oqq "$TARGET_IPA_PATH" -d "$TEMP_PATH"
# 拿到解压的临时的APP的路径
TEMP_APP_PATH=$(set -- "$TEMP_PATH/Payload/"*.app;echo "$1")
# echo "路径是:$TEMP_APP_PATH"

#----------------------------------------
# 2. 将解压出来的.app拷贝进入工程下
# BUILT_PRODUCTS_DIR 工程生成的APP包的路径
# TARGET_NAME target名称
TARGET_APP_PATH="$BUILT_PRODUCTS_DIR/$TARGET_NAME.app"
echo "app路径:$TARGET_APP_PATH"

rm -rf "$TARGET_APP_PATH"
mkdir -p "$TARGET_APP_PATH"
cp -rf "$TEMP_APP_PATH/" "$TARGET_APP_PATH"

#----------------------------------------
# 3. 删除extension和WatchAPP.个人证书没法签名Extention
rm -rf "$TARGET_APP_PATH/PlugIns"
rm -rf "$TARGET_APP_PATH/Watch"

#----------------------------------------
# 4. 更新info.plist文件 CFBundleIdentifier
#  设置:"Set : KEY Value" "目标文件路径"
/usr/libexec/PlistBuddy -c "Set :CFBundleIdentifier $PRODUCT_BUNDLE_IDENTIFIER" "$TARGET_APP_PATH/Info.plist"

#----------------------------------------
# 5. 给MachO文件上执行权限
# 拿到MachO文件的路径WeChat
APP_BINARY=`plutil -convert xml1 -o - $TARGET_APP_PATH/Info.plist|grep -A1 Exec|tail -n1|cut -f2 -d\>|cut -f1 -d\<`
#上可执行权限
chmod +x "$TARGET_APP_PATH/$APP_BINARY"

#----------------------------------------
# 6. 重签名第三方 FrameWorks
TARGET_APP_FRAMEWORKS_PATH="$TARGET_APP_PATH/Frameworks"
if [ -d "$TARGET_APP_FRAMEWORKS_PATH" ];
then
for FRAMEWORK in "$TARGET_APP_FRAMEWORKS_PATH/"*
do

#签名
/usr/bin/codesign --force --sign "$EXPANDED_CODE_SIGN_IDENTITY" "$FRAMEWORK"
done
fi

#注入
#yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/XXXX.framework/XXXX"
```

放在项目根目录下，Xcode执行脚本处写入如下代码

```shell
chmod +x app.sh
./app.sh
```

![img](15_resign.assets/1-20210312163724137)

### 4.运行

连接手机，Command+Run运行项目就能将越狱包装到手机上了

当然这种方式也能像应用重签名一样进行附加进程

## 参考文献

[iOS 逆向（七）重签名](https://juejin.cn/post/6906998121973153799)

[iOS逆向 应用重签名+微信重签名实战](https://juejin.cn/post/6844903998110040077)

[iOS逆向 Shell脚本+脚本重签名](https://juejin.cn/post/6844903998554636301)

[About Code Signing](https://developer.apple.com/library/archive/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005929-CH1-SW1)

