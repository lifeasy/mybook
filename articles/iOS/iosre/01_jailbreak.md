# iPhone越狱

> 以下为维基百科中的描述
>
> [越狱 (iOS)](https://zh.wikipedia.org/wiki/%E8%B6%8A%E7%8D%84_(iOS))
>
> **iOS越狱**（英语：**iOS Jailbreaking**）是获取iOS设备的[Root权限](https://zh.wikipedia.org/wiki/超级用户)的技术手段。iOS系统的Root用户对除Apple特定私有进程之外的其他进程不开放，使用Root用户运行的进程在进程树中的PID为0。程序员在iOS中挖掘出一些可以将进程提权至PID0的漏洞（例如Task For PID0）。利用Root用户运行的进程意味着可以任意读取设备其中的APFS分区表和内核缓存地址，拥有一个用户可以随意控制的PID0进程还不能称之为一个完整的越狱。之后还需要利用Bypass（旁路）手段绕过Apple在iOS系统中设置的其他安全防护措施，将APFS或HFS+文件系统中的ROOTFS分区重新挂载（Remount）为可读写（R/W），从而达到添加二进制文件和守护进程的目的。通常大众用户认为能够正常使用Cydia才能被称为越狱，但其实这种说法是不正确的。但通过此软件可以完成越狱前不可能进行的动作，例如安装App Store以外未经过签名的应用、修改SpringBoard、运行[Shell](https://zh.wikipedia.org/wiki/Shell)程序、使有运营商锁的设备利用卡贴解锁后通过替换配置文件形式实现本地化（例如“去除+86”，解锁FaceTime功能）。
>
> 越狱软件分发商店Cydia的创始人Jay Freeman在2010年10月估计，全球大概有10%的iPhone曾进行过越狱[[2\]](https://zh.wikipedia.org/wiki/越獄_(iOS)#cite_note-2)。当iOS设备越狱后，用户可对系统进行编辑或是运行不被苹果公司所验证的软件
>
> 目前对越狱的完美程度，业界开发人员给出了四个类别：
>
> - **不完美越狱**（Tethered Jailbreak），指的是，当处于此状态的iOS设备开机重启后，之前进行的越狱程序就会失效，用户将失去Root权限，需要将设备连接电脑来使用（如特定版本下的红雪（redsn0w））等越狱软件进行引导开机以后，才可再次使用越狱程序。否则设备将无法正常引导。
>
> - **完美越狱**（Untethered Jailbreak），指设备在进行重启后，越狱状态仍被完整保留。
>
> - **半不完美越狱**（Semi-tethered Jailbreak），指设备在重启后，将丢失越狱状态，并恢复成未越狱状态。如果想要恢复越狱环境，必须连接计算机并在越狱工具的引导下引导来恢复越狱状态。
>
> - **半完美越狱**（Semi-untethered Jailbreak），指设备在重启后，将丢失越狱状态；而若想要再恢复越狱环境，只需在设备上进行某些操作即可恢复越狱。

## 一、越狱

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
>
> uncOver企业签的安装方式：[iosninja](https://app.iosninja.io/)

## 二、Cydia源

添加Cydia源，下载常用软件

[Cydia常用源](https://zhuanlan.zhihu.com/p/137563228)

## 三、Cydia安装常用软件

* OpenSSH
  * 用于Mac远程ssh访问手机
* Cycript
  * Cycript是由Cydia创始人Saurik推出的一款脚本语言，Cycript混合了OC、JavaScript语法的解释器，这意味着我们能够在一个命令中使用OC或者JavaScript，甚至两者并用。它能够hook正在运行的进程，在运行时获取、修改很多东西。
* Filza
  * 查看手机文件系统
* RevealLoader
  * 配合Mac端的Reveal，可查看App的UI架构
* Vi IMproved
  * 文本编辑器
* Apple File Conduit
  * 可通过Mac软件（类似iFunBox）查看手机文件系统