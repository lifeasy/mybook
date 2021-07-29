# 符号

## 间接符号查找过程

fishhook的查找过程图

TODO

## 去符号&恢复符号

xcode脱符号，strip，两个地方

断点不能来，可以符号断点间接符号，bt显示看不出来方法名

如何恢复？因为虽然去掉了符号，但是因为runtime的原因，仍然包含了类型和方法名，签名等信息

工具：restore-symbol

https://github.com/tobefuturer/restore-symbol

## 使用fishhook防护

自己使用fishhook，hook runtime的一些方法，让别人不能进行方法交换,mehod_exchangeImplementations

## MonkeyDev

issue 266

重签名

Hook->logos

monkey内部用的method_setImplementation和method_getImplementation

monkey会注入很多动态库，其中就有libsubstrate.dylib，这个库就是用来解析logos，将xm文件解析称为.mm文件



