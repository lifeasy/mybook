# Cycript

`Cycript`是由 `Cydia` ( 熟悉越狱的同学应该都很清楚 )  创始人 `Saurik` 推出的一款脚本语言，`Cycript` 混合了 `OC`、 `JavaScript` 语法的解释器，这意味着我们能够在一个命令中使用 `OC` 或者 `JavaScript`，甚至两者并用。

它能够hook**正在运行的进程**，在运行时修改很多东西。

[Cycript官网](http://cycript.org) 

## 常用语法

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

## 参考资料

[mjcript](https://github.com/CoderMJLee/mjcript)

[iOS 逆向 - lldb高级篇 Chisel 与 Cycript](https://juejin.cn/post/6844904006758694925)