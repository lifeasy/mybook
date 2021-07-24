#  重签名

## codesign签名

codesign是Xcode提供的签名工具

签名是可以替换的

签名就是用证书对一份代码进行签名，现在使用另一份证书对同一份代码进行签名

列出钥匙串里可签名的证书：

```shell
$ security find-identity -v -p codesigning
```

查看Mach-O是否加密：

```shell
$ otool -l HelloWorldApp.app/HelloWorldApp | grep cryptid -A 5 -B 5
Load command 13
          cmd LC_ENCRYPTION_INFO_64
      cmdsize 24
     cryptoff 16384
    cryptsize 16384
      cryptid 0 	# 0表示为加密 1表示加密
          pad 0
```

加密并不是加密的全部Mach-O，只加密了代码段

## 重签名演练

1、删除插件和Watch，给可执行文件添加权限

2、使用`codesign`重签名`framework`

```shell
$ codesign -fs 36833CADAC1367617DA226BE8F5490253208DB68 mm_dart_cpp.framework
```

3、使用Xcode生成描述文件，需要使用上述证书，将生成的描述文件拷贝到目标App的包内

4、在目标App包内的`plist`文件修改`bundle id`为和上述描述文件相同

5、拷贝出目标App的描述文件中的权限到一个新的`plist`文件中，然后通过指定权限文件对App的Mach-O进行重签名

```shell
$ codesign -fs 36833CADAC1367617DA226BE8F5490253208DB68 --no-strict --entitlements=entitlements.plist WeChat.app
```

6、重签完成，可以使用Xcode进行安装了，也可以使用Xcode进行调试了

* 上述步骤6，可以提前用Xcode生成一次，然后用上面的包在目标路径下替换然后重新运行Xcode，就能安装上了

## 脚本重签

1、用户组

<img src="https://gitee.com/dexport/blog-image/raw/master/img/20210724173709.png" style="zoom: 25%;" />

2、Xcode脚本重签名

脚本的目录就是当前工程的根目录

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
# 拿到MachO文件的路径
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
#yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/HankHook.framework/HankHook"
```

* 最新的Wechat(8.0.2)包里面，info.plist有个Support Device的，删掉