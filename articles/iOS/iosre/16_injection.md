# 代码注入

> 前置条件 使用脚本重签名

## 一、FrameWork注入

### 1.1 新建FrameWork

在Xcode中`File->Target`新增一个Framework

![img](16_injection.assets/1-20210312180830497)

### 1.2 FrameWork中新建一个类

### 1.3 添加一个`load`方法

![img](16_injection.assets/1-20210312180908122)

> 目前DYLD会动态加载项目中的Frameworks，但不会加载当前FrameWork

### 1.4 运行编译一下

保证FrameWork放到`FrameWorks`目录下

![img](16_injection.assets/1-20210312181058121)

### 1.5 yololib注入动态库

建议将`yololib`复制粘贴到`/usr/local/bin`下，可以随时随地调用

将`app.sh`的最后一句代码启用（注意修改FrameWork名称）

```shell
yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/XXXX.framework/XXXX"
```

### 1.6 运行

控制台打印 ❎❎❎❎❎❎❎❎❎❎

![img](16_injection.assets/1-20210312181205906)



## 二、dylib注入

### 2.1 新建Library Target

![img](16_injection.assets/1-20210312182206965)

### 2.2 修改dylib的`BaseSDK`

![img](16_injection.assets/1-20210312182230191)

### 2.3 修改dylib的`签名`

修改成iPhone Developer或者通用的Apple Developer

![img](16_injection.assets/1-20210312182257825)

### 2.4 添加依赖

![img](16_injection.assets/1-20210312182350689)

### 2.5 运行编译

查看是否已经添加到App的Frameworks中

![img](16_injection.assets/1-20210312182438992)

### 2.6 修改脚本，使用yololib注入动态库

```shell
yololib "$TARGET_APP_PATH/$APP_BINARY" "Frameworks/libFXHook.dylib"
```

如果遇到`image not found`，`clean`一下再`run`

