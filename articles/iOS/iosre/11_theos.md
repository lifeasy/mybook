# Theos

## Theos环境配置

安装签名工具

```bash
$ brew install ldid
```

添加`Theos`环境变量

```shell
export THEOS=~/.theos
export PATH=$THEOS/bin:$PARH
```

下载`Theos`,`thoes`有其他依赖，使用参数`--recursive`进行clone

```shell
$ git clone --recursive https://github.com/theos/theos.git $THEOS
```

## 新建项目

新建`tweak`项目

```shell
$ nic.pl
NIC 2.0 - New Instance Creator
------------------------------
  [1.] iphone/activator_event
  [2.] iphone/activator_listener
  [3.] iphone/application_modern
  [4.] iphone/application_swift
  [5.] iphone/cydget
  [6.] iphone/flipswitch_switch
  [7.] iphone/framework
  [8.] iphone/library
  [9.] iphone/notification_center_widget
  [10.] iphone/notification_center_widget-7up
  [11.] iphone/preference_bundle_modern
  [12.] iphone/theme
  [13.] iphone/tool
  [14.] iphone/tool_swift
  [15.] iphone/tweak
  [16.] iphone/tweak_with_simple_prefere$ pwd
/Users/360jr/Desktop/iOSRe/tool/demo/ChangeColor/tweakwechatnces
  [17.] iphone/xpc_service
Choose a Template (required): 15 # 新建tweak项目
Project Name (required): TweakWeChat # 项目名称
Package Name [com.yourcompany.tweakwechat]: com.liang.tweakwechat # bundle id
Author/Maintainer Name [360JR]: Liang # author
[iphone/tweak] MobileSubstrate Bundle filter [com.apple.springboard]: com.tencent.xin # 需要动态修改的app的bundle id 【重要】
[iphone/tweak] List of applications to terminate upon installation (space-separated, '-' for none) [SpringBoard]: # 安装插件完成后需要关闭的app的bundle id，默认重启SpringBoard，直接回车即可
Instantiating iphone/tweak in tweakwechat/...
Done.

$ cd tweakwechat
$ ls
Makefile          Tweak.x           TweakWeChat.plist control
```

## 项目配置

* Makefile

```shell
# 添加设备ip和port，如果使用usb转发的话，这里ip为127.0.0.1，端口为转发端口
export THEOS_DEVICE_IP = 10.94.51.82
export THEOS_DEVICE_PORT = 22
# 打包架构 一般为安装设备的架构
ARCHS = arm64
# 导入的FRAMEWORK
<name>_FRAMEWORKS = UIKIT
```

* Tweak.x

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

## 安装

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

## 查看

在Cydia中的最近安装里可以看到自己开发的插件的详细信息和安装位置

