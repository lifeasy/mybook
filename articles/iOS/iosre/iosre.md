# iOS逆向全流程

## 一、越狱

> 是获取iOS设备的[Root权限](https://zh.wikipedia.org/wiki/超级用户)的技术手段。
>
> - **不完美越狱**（Tethered Jailbreak），指的是，当处于此状态的iOS设备开机重启后，之前进行的越狱程序就会失效，用户将失去Root权限，需要将设备连接电脑来使用（如特定版本下的红雪（redsn0w））等越狱软件进行引导开机以后，才可再次使用越狱程序。否则设备将无法正常引导。
> - **完美越狱**（Untethered Jailbreak），指设备在进行重启后，越狱状态仍被完整保留。
> - **半不完美越狱**（Semi-tethered Jailbreak），指设备在重启后，将丢失越狱状态，并恢复成未越狱状态。如果想要恢复越狱环境，必须连接计算机并在越狱工具的引导下引导来恢复越狱状态。
> - **半完美越狱**（Semi-untethered Jailbreak），指设备在重启后，将丢失越狱状态；而若想要再恢复越狱环境，只需在设备上进行某些操作即可恢复越狱。

### 1.1、uncOver

> 使用工具**unc0ver** （半完美越狱）（网上也存在一些其他越狱手段，可自行查找）
>
> [unc0ver官网](https://unc0ver.dev/)
>
> [unc0ver源码](https://github.com/pwn20wndstuff/Undecimus)
>
> 目前已支持iOS 11.0 - 14.3

官网有详细的操作步骤，直接参照操作即可。

`uncOver`越狱过程中需要观看广告，可提前翻墙观看或者多试几次。

`uncOver`是半完美越狱，重启手机之后需要重新越狱。

> 一些其他的越狱工具：[checkra1n](https://checkra.in/)

### 1.2、添加Cydia源

添加Cydia源，下载常用软件

[Cydia常用源](https://zhuanlan.zhihu.com/p/137563228)

Cydia安装常用软件

- OpenSSH
  - 用于Mac远程ssh访问手机
- Cycript
  - Cycript是由Cydia创始人Saurik推出的一款脚本语言，Cycript混合了OC、JavaScript语法的解释器，这意味着我们能够在一个命令中使用OC或者JavaScript，甚至两者并用。它能够hook正在运行的进程，在运行时获取、修改很多东西。
- Filza
  - 查看手机文件系统
- RevealLoader
  - 配合Mac端的Reveal，可查看App的UI架构
- Vi IMproved
  - 文本编辑器
- Apple File Conduit
  - 可通过Mac软件（类似iFunBox）查看手机文件系统

## 二、ssh远程访问越狱设备

### 2.1、ssh通过网络连接

越狱手机安装完 OpenSSH后，即可通过ssh远程访问。

ssh默认端口为22。

默认ssh账号密码：`account: root`	`password:alpine`

```shell
$ ssh root@10.1.1.193
```

### 2.2、基于usbmuxd服务，通过usb实现MAC与iPhone的通信

```shell
$ brew install usbmuxd
# 安装usbmuxd库之后，就顺带安装了一个小工具iproxy
# iproxy 本地端口 映射端口 
$ iproxy 10010 22 
```

即可通过本地10010端口映射到远端22端口

* ssh指定端口

```shell
$ ssh root@localhost -p 10010
Last login: Mon Mar  8 15:16:23 2021 from 127.0.0.1
Cchukou:~ root#
```

* scp也可通过usb进行拷贝

```shell
# -P 进行端口指定（大写P）
scp -P dumpdecrypted.dylib root@localhost:/usr/lib
```

## 三、Cycript 代码调试

> `Cycript`是由 `Cydia` 创始人 `Saurik` 推出的一款脚本语言，`Cycript` 混合了 `OC`、 `JavaScript` 语法的解释器，这意味着我们能够在一个命令中使用 `OC` 或者 `JavaScript`，甚至两者并用。
>
> 它能够hook**正在运行的进程**，在运行时执行代码，获取修改很多东西。

[Cycript官网](http://cycript.org/)

### 3.1、常用语法

`#` + 对象地址 可以直接调用对象的方法

```shell
cy# [#0x10ec66aa0 subviews]
@[#"<UILayoutContainerView: 0x10ec682e0; frame = (0 0; 320 568); autoresize = W+H; gestureRecognizers = <NSArray: 0x28197be70>; layer = <CALayer: 0x2817f28c0>>"]
```

查看视图层级`recursiveDescription()`

```shell
UIWindow.keyWindow().recursiveDescription().toString()
```

UIApp等价于[UIApplication sharedApplication]

```shell
cy# UIApp
#"<UIApplication: 0x10ec35870>"
```

定义变量

```shell
cy# var a = 3
3
cy# var b = 4
4
cy# a + b
7
```

显示已加载的所有OC类

```shell
ObjectiveC.classes
```

显示对象的所有成员变量

```shell
cy# *#0x111970800
{isa:NSKVONotifying_WCAccountLoginFirstViewController,_hasOverrideClient:0,_hasOverrideHost:0,_hasInputAssistantItem:0,_overrideTransitioningDelegate:null,_view:#"<UIView: 0x11149a930; frame = (0 0; 320 568); autoresize = W+H; layer = <CALayer: 0x2817e59a0>>",_tabBarItem:null,_navigationItem:#"<UINavigationItem: 0x1114d6930> titleView=0x1114d6500",
...
```

筛选出某种类型的对象

```shell
cy# choose(UIViewController)
[#"<MMUINavigationController: 0x112855600> ChildViewControllers:(\n    \"<WCAccountLoginFirstViewController: 0x111970800>\"\n)",#"<MMUINavigationController: 0x111822a00> ChildViewControllers:(\n    \"<WCAccountBaseViewController: 0x1119a1000>\"\n)"]
```

退出cy 调试模式

`control`+`d`

实际开发中，可将常用的方法封装到一个`.cy`文件中，然后放到`/usr/lib/cycript0.9`中，可以在此路径下自定义路径，并且在cycript环境中导入，即可使用

```shell
## com.mj.mjcript 意思是cycript根路径下的com/mj/mjcript.cy文件
cy# @import com.mj.mjcript
{}
```

## 四、Reveal 界面调试

[Reveal](https://revealapp.com/) 是 `Mac OS X` 平台上的一款方便开发者调试 `iOS` 应用的开发软件，[Reveal](https://revealapp.com/) 能够在运行时调试和修改 `iOS` 应用程序。 `Reveal` 能连接到应用程序，并允许开发者编辑各种用户界面参数，而且会立即反应在程序的 `UI` 上。

参考：[Reveal 详细安装（以及安装问题解决）](https://juejin.cn/post/6909716520490762248)

## 五、脱壳

上传至Apple Store的应用，会被加密，无法正常查看Mach-O信息，需要脱壳

- 如何检查Mach-O是否已经脱壳
  - 查看LC中的`LC_ENCRyPTION_INFO`中的`Crypt ID`，如果等于`0`则未加密

### 5.1、frida-ios-dump一键脱壳

[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)

1）Mac安装frida

```shell
$ sudo pip install frida
```

2） 下载[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)

3）进入`frida-ios-dump`，安装依赖，可能会出现错误，可先不用管，部分库未下载下来可在后面根据提示在进行安装

```shell
sudo pip install -r requirements.txt --upgrade
```

4）使用iproxy映射端口

```shell
$ iproxy 10010 22
```

5）修改`frida-ios-dump`中的`dump.py`文件的端口配置

```shell
Port = 10010
```

6）手机`Cydia`中安装`frida`

```shell
# 打开Cydia->软件源->编辑->添加，输入build.frida.re，添加软件源后，搜索安装Frida即可，注意Frida对应的Cpu型号
```

7）`frida-ios-dump`工程中执行dump

```shell
$ ./dump.py com.tencent.xin
Start the target app com.tencent.xin
.....
Generating "微信.ipa"

# 如果报错 ImportError: No module named 'xxx'
# sudo pip install 'xxx'
```

### 5.2、class-dump

可以将Objective-C编写的二进制文件反编出头文件，需要是已砸壳的二进制文件。

[官网](https://links.jianshu.com/go?to=http%3A%2F%2Fstevenygard.com%2Fprojects%2Fclass-dump%2F)

[源码](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fnygard%2Fclass-dump)

[MonkeyDev/class-dump](https://github.com/AloneMonkey/MonkeyDev/blob/master/bin/class-dump)：支持swift和oc混编

1）使用`class-dump`导出头文件

```shell
# class-dump -H Mach-O文件 -o 输出路径
$ class-dump -H brush_free.decrypted -o brush_header
```

## 六、Theos

编写tweak项目，动态Hook第三方程序

### 6.1、环境配置

```shell
# 安装签名工具
$ brew install ldid
# 配置Theos环境变量
export THEOS=~/.theos
export PATH=$THEOS/bin:$PARH
# 下载Theos,及其依赖
$ git clone --recursive https://github.com/theos/theos.git $THEOS
```

### 6.2、编写tweak

新建`tweak`项目

```shell
$ nic.pl
NIC 2.0 - New Instance Creator
------------------------------
  [1.] iphone/activator_event
  [2.] iphone/activator_listener
  ...
  [15.] iphone/tweak
  ...
Choose a Template (required): 15 # 新建tweak项目
Project Name (required): TweakWeChat # 项目名称
Package Name [com.yourcompany.tweakwechat]: com.liang.tweakwechat # bundle id
Author/Maintainer Name [360JR]: Liang # author
[iphone/tweak] MobileSubstrate Bundle filter [com.apple.springboard]: com.tencent.xin # 需要动态修改的app的bundle id 【重要】
[iphone/tweak] List of applications to terminate upon installation (space-separated, '-' for none) [SpringBoard]: # 安装插件完成后需要关闭的app的bundle id，默认重启SpringBoard，直接回车即可
Instantiating iphone/tweak in tweakwechat/...
Done.
```

`tweak`项目配置

- Makefile

```shell
# 添加设备ip和port，如果使用usb转发的话，这里ip为127.0.0.1，端口为转发端口
export THEOS_DEVICE_IP = 10.94.51.82
export THEOS_DEVICE_PORT = 22
# 打包架构 一般为安装设备的架构
ARCHS = arm64
# 导入的FRAMEWORK
<name>_FRAMEWORKS = UIKIT
```

- Tweak.x

```objective-c
#import <Foundation/Foundation.h> // 需要导入的框架
#import <UIKit/UIKit.h>

// 需要hook的类和方法、属性等，需要提前声明
@interface ViewController
- (UIView *)view;
@end

// hook的类  
%hook ViewController

// hook的方法  
- (void)changeBlue {
    [[self view] setBackgroundColor:[UIColor yellowColor]];
}

%end
```

安装项目

```shell
$ make clean &&  make && make package && make install
# 这样安装的版本为debug版本，在Cydia中可以看到，如果想安装release版本，在make package的时候加参数
# make package debug=0

# make 打包成动态库.dylib
# make package 生成.deb
# make install 根据配置的手机地址和端口信息，远程连接手机，安装deb，调用cydia安装动态库
# 安装的动态库统一由 Cydia Substrate进行管理
# 启动微信后，Cydia Substrate会去/Library/MobileSubstrate/DynamicLibraries中检查是否安装了和微信bundle id相同的动态库（通过plist检测，也在这个路径中），有的话，就加载
```

在Cydia中的最近安装里可以看到自己开发的插件的详细信息和安装位置

## 七、动态调试

通过lldb attach 已有进程，调试应用

### 7.1、debugserver重签名

默认情况下，`/Developer/usr/bin/debugserver`缺少一定的权限，只能调试通过Xcode安装的APP，无法调试其他的APP（比如通过App Store下载的APP）

如果希望调试其他APP，需要对debugserver重新签名

1）将手机上的`/Developer/usr/bin/debugserver`复制到mac

2）通过`file`命令查看文件信息

```shell
$ file debugserver
debugserver: Mach-O universal binary with 2 architectures: [arm64:Mach-O 64-bit executable arm64] [arm64e]
debugserver (for architecture arm64):    Mach-O 64-bit executable arm64
debugserver (for architecture arm64e):    Mach-O 64-bit executable arm64e2.1）
```

如果`Fat Mach-O`的话，使用`lipo`命令剥离出和自己手机匹配的架构

```shell
lipo -thin arm64 debugserver -output debugserver
```

3）导出原来权限

```shell
$ ldid -e debugserver > debugserver.entitlements
```

4）修改权限文件，添加删除响应的权限，这里删除了`com.apple.security.network.server`，`com.apple.security.network.client`，`seatbelt-profiles`，添加了`get-task-allow`，`task_for_pid-allow`，`platform-application`，`run-unsigned-code`

```shell
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.backboardd.debugapplications</key>
    <true/>
    <key>com.apple.backboardd.launchapplications</key>
    <true/>
    <key>com.apple.frontboard.debugapplications</key>
    <true/>
    <key>com.apple.frontboard.launchapplications</key>
    <true/>
    <key>com.apple.springboard.debugapplications</key>
    <true/>
    <key>com.apple.system-task-ports</key>
    <true/>
    <key>get-task-allow</key>
    <true/>
    <key>platform-application</key>
    <true/>
    <key>run-unsigned-code</key>
    <true/>
    <key>task_for_pid-allow</key>
    <true/>
</dict>
</plist>
```

5）重签名

```shell
$ ldid -Sdebugserver.entitlements debugserver
```

将已经签好权限的`debugserver`放到手机的`/usr/bin`目录

6）ssh登录手机，为debugserver添加权限

```shell
$:~ root# chmod +x /usr/bin/debugserver
```

### 7.2、开启调试

1）mac上开始usb端口映射

```shell
$ iproxy 10086 10086
```

2）debugserver附加到进程，需要提前打开app。或者可以通过debugserver直接启动app并附加到进程

```shell
Cchukou:~ root# debugserver 127.0.0.1:10086 -a WeChat
debugserver-@(#)PROGRAM:LLDB  PROJECT:lldb-900.3.87
 for arm64.
Attaching to process WeChat...
Listening to port 10086 for a connection from localhost...
```

3）lldb连接到debugserver

```shell
$ lldb
(lldb) process connect connect://127.0.0.1:10086
Process 14359 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
    frame #0: 0x00000001a6dff0f4 libsystem_kernel.dylib`mach_msg_trap + 8
libsystem_kernel.dylib`mach_msg_trap:
->  0x1a6dff0f4 <+8>: ret

# 链接成功

# 尝试打印image信息
(lldb) image list -o -f
[  0] 0x0000000001164000 /var/containers/Bundle/Application/C4BD67F6-5D1E-4FB8-AAAC-CD9EE05248E9/ChangeColor.app/ChangeColor(0x0000000101164000)
[  1] 0x0000000101490000 /Users/360jr/Library/Developer/Xcode/iOS DeviceSupport/12.4.8 (16G201)/Symbols/usr/lib/dyld
[  2] 0x000000010117c000 /usr/lib/substitute-inserter.dylib(0x000000010117c000)
[  3] 0x00000000263a8000 /Users/360jr/Library/Developer/Xcode/iOS DeviceSupport/12.4.8 (16G201)/Symbols/System/Library/Frameworks/Foundation.framework/Foundation
[  4] 0x00000000263a8000 /Users/360jr/Library/Developer/Xcode/iOS DeviceSupport/12.4.8 (16G201)/Symbols/usr/lib/libobjc.A.dylib
```

## 八、应用重签名

如果需要将破坏了签名的安装包，安装到非越狱的手机上，需要对安装包进行重签名的操作。

**进行重签名的包，必须是未加壳的。**

### 8.1、 重签名前期准备

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

### 8.2、 重签名步骤

1）删除插件和带有插件的.app包（比如Watch，PlugIns）

因为普通账号不能对插件进行签名，所以需要删除

在`包内容`路径中找到`Watch`和`PlugIns`文件夹直接删除

2）重签framework

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

3）给MachO上可执行权限

在`包内容`路径下执行`$ Chmod +x WeChat`

4）添加描述文件

新建工程，真机运行即可得到描述文件

将ipa包中的`描述文件`放到微信的`包内容`中去

5）修改应用包的bundid

修改微信`包内容`中的info.plist->BundleId，改为与`描述文件`的BundleId一致

6）授权文件重签app包

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

7）安装.app包

Xcode界面下`shift+cmd+2`调出手机设备

选择`+`->`Replace`

如果出现安装失败，删除之前app在重新安装

app运行后利用Xcode附加进程：Debug->Attach to Process

### 8.3、 Xcode重签名

**必须是与MachO文件同名工程（工程名相同，BundId无所谓），否则进程不是微信进程**

1）运行app，装描述文件

新建`WeChat`工程，同上`Command+Run`，将描述文件生成一下

2）替换掉app包

复制一个新的越狱包，覆盖掉到原来的ipa包

3） 删除插件、Watch

4） 重签framework

5） run

由于是Xcode直接运行，可以通过Debug View查看界面

以上流程可以通过Xcode添加脚本直接完成

## 九、动态库注入

### 9.1、FrameWork注入

1）Xcode新建一个FrameWork 动态库项目，测试代码新建一个类，重写`+(void)load`方法，打印一些东西。运行编译生成`.framework`库文件。

2）将`.framework`库文件放到`IPA`包的`FrameWorks`目录下

3） yololib注入动态库

[yololib](https://github.com/KJCracks/yololib)

```shell
# yololib binary dylib file
yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/XXXX.framework/XXXX"
```

即可注入动态库到Mach-O中，查看Mach-O LC中链接的动态库，可以发现已经链接了自己生成的动态库

4）运行程序

可以发现控制台打印了`+(void)load`中测试的输入信息。

### 9.2、dylib注入

1）新建Library Target

macOS->Library

2）修改dylib的`BaseSDK`为`iOS`

3）修改dylib的`签名`为iPhone Developer或者通用的Apple Developer，编译生成.dylib

4）主Target 添加依赖.dylib

5）运行编译主Target，查看是否已经添加到App的Frameworks中

6）yololib注入动态库

```shell
yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/libFXHook.dylib"
```

如果遇到`image not found`，`clean`一下再`run`