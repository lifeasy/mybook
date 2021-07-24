# 代码注入

## 注入原理

程序的动态库会加载

动态库的加载时在App Mach-O中的LC中`LC_LOAD_DYLIB`标记

所以如果需要程序加载我们自己添加的动态库，需要修改Mach-O，添加对应的LC段

使用`yololib`

## 注入流程 

1. Framwork注入

* 通过Xcode新建Framwork，将库安装进入APP包
* 通过yololib注入Framwork库路径。命令：$yololib（空格）MachO文件路径（空格）库路径
* 所有的Framwork加载都是由DYLD加载进入内存被执行的
* 注入成功的库路径会写入到MachO文件的LC_LOAD_DYLIB字段中

2. Dylib注入

* 通过Xcode新建Dylib库（注意：Dylib属于MacOS所以需要修改属性）
* 添加Target依赖(新建Copy到Framework)，让Xcode将自定义Dylib文件打包进入APP包。
* 利用yololib进行注入。

### 脚本注入

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
yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/HankHook.framework/HankHook"
```

## Method Swizzling注意事项

* 如果交换后，回调原来类的方法，SEL不存在的话，也会报错，因为img是根据sel去查找的
* 所以一般交换的话，就是给原始类添加一个方法，然后在原始类中进行交换