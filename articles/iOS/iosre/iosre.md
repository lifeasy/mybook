# iOSRe

## 越狱

### 安装unc0ver（非完美越狱）

[unc0ver官网](https://unc0ver.dev/)

[unc0ver源码](https://github.com/pwn20wndstuff/Undecimus)

> 一些其他的越狱工具：[checkra1n](https://checkra.in/)

安装完成后进行越狱，会自行安装Cydia

### cydia源

https://zhuanlan.zhihu.com/p/137563228

### 使用Cydia安装常用应用

* openSSH
* [Cycript](http://www.cycript.org/)
* Filza
* Flex 3

## 在Mac上安装常用工具

### 砸壳工具dumpdecrypted

1. [下载源码](https://github.com/stefanesser/dumpdecrypted)，修改`makefile`

```sh
## 这里指定越狱设备的架构
GCC_UNIVERSAL=$(GCC_BASE) -arch armv7 -arch armv7s -arch arm64改成 GCC_UNIVERSAL=$(GCC_BASE) -arch arm64
## 这里指定越狱设备的版本号
## 网上也有其他操作，下载对应设备版本的Xcode
CFLAGS = 改成CFLAGS = -target arm64-apple-ios12.4
```

2. `make`，生成`dumpdecrypted.dylib`
3. 用`codesign`和个人调试证书给`dumpdecrypted.dylib`签名

```shell
# 查找可用的证书，然后用找到的证书签名
security find-identity -v -p codesigning 
codesign --force --verify --verbose --sign "找到的可用证书名称" dumpdecrypted.dylib
```

#### 遇到的错误

```shell
Killed: 9
# 切换mobile用户：su mobile 
# 或者
# dumpdecrypted.dylib未签名，参考上面步骤进行重签名
```

```shell
dyld: warning: could not load inserted library 'dumpdecrypted.dylib' into hardened process because no suitable image found.  Did find:
	dumpdecrypted.dylib: file system sandbox blocked mmap() of 'dumpdecrypted.dylib'
2021-02-23 10:54:50.669 brush_free[1563:120502] Checking for file at /var/mobile/Containers/Data/Application/D627C61F-4A0C-4DA3-978D-BD710C904571/Library/Caches/flex-extract.signal
2021-02-23 10:54:50.686 brush_free[1563:120502] FLXX Error: Couldn't get contents of file: /var/mobile/Library/Application Support/Flex3/patches.plist
Abort trap: 6
# 未将dylib放到/usr/lib/目录下
# 新版iOS不支持将dylib放在.app或Documents目录下来注入
```

```shell
dyld: Symbol not found: ___chkstk_darwin
# dumpdecrypted.dylib对应的SDK版本号不对，修改makefile重新编译或者下载对应版本的Xcode
```

### class-dump

可以将Objective-C编写的二进制文件反编出头文件，需要是已砸壳的二进制文件。

[官网](https://links.jianshu.com/go?to=http%3A%2F%2Fstevenygard.com%2Fprojects%2Fclass-dump%2F)

[源码](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fnygard%2Fclass-dump)

[MonkeyDev/class-dump](https://github.com/AloneMonkey/MonkeyDev/blob/master/bin/class-dump)：支持swift和oc混编

#### 遇到的错误

```shell
Error: Cannot find offset for address 0x40000000010053e2 in stringAtAddress:
# 项目中存在Swift，使用其他改版的class-dump
```

### Hopper

目标文件代码分析

[官网](https://www.hopperapp.com/)

## App逆向流程（系统版本iOS 12.4）

### 1. 砸壳

IOS的APP，若上传了App Store会被苹果进行一次加密，所以我们下载下来的安装包都是加密的，若要进行dump需要进行一次解密，即砸壳。

运行中的程序已经加载到了内存中，已经被系统解密

使用cydia安装应用：openSSH、Cycript、Filza

1）越狱手机上下载要破解的应用

2）在mac上ssh远程到越狱手机

```shell
# Mac
# 密码默认alpine
$ ssh root@10.94.51.82
root@10.94.51.82's password:
Cchukou:~ root#
```

3）运行app，`ps`查看进程信息，查找破解App的bundle地址

```shell
# iPhone
Cchukou:~ root# ps -e | grep brush
 1492 ??         1:18.02 /var/containers/Bundle/Application/EA717F2A-A0F7-4988-BA96-95B5830539B0/brush_free.app/brush_free
 1532 ttys000    0:00.01 grep brush
```

一些其他操作：

* 导出app包

```shell
# Mac
scp -r root@10.94.51.82:/var/containers/Bundle/Application/EA717F2A-A0F7-4988-BA96-95B5830539B0/brush_free.app .
```

* 代码查看手机上安装的所有应用的bundle id，使用了私有api，需要在越狱机上使用

```objective-c
#import <objc/runtime.h>
// 获取安装app 的bundle id
Class LSApplicationWorkspace_class = NSClassFromString(@"LSApplicationWorkspace");
NSObject *workspace = [LSApplicationWorkspace_class performSelector:@selector(defaultWorkspace)];
NSArray *arrAPP = [workspace performSelector:@selector(allApplications)];
NSLog(@"arrAPP: %@", arrAPP);
```

4）`cycript`寻找app的沙盒Documents具体路径

```shell
# iPhone
Cchukou:~ root# cycript -p brush_free
cy# NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES)[0]
@"/var/mobile/Containers/Data/Application/D627C61F-4A0C-4DA3-978D-BD710C904571/Documents"
```

5）使用`scp`指令把`dumpdecrypted.dylib`拷贝到`/usr/lib/`，老一点的系统版本可能需要放到`Documents`下

```shell
# Mac
$ scp -r dumpdecrypted.dylib root@10.94.51.82:/usr/lib
```

6）切换`mobile`用户(旧版本可能不需要)

```shell
# iPhone
$ su mobile
```

7）执行`dumpdecrypted`砸壳，这里是在`Documents`路径下操作

```shell
# iPhone
Cchukou:~/Containers/Data/Application/D627C61F-4A0C-4DA3-978D-BD710C904571/Documents mobile$ DYLD_INSERT_LIBRARIES=/usr/lib/dumpdecrypted.dylib /var/containers/Bundle/Application/EA717F2A-A0F7-4988-BA96-95B5830539B0/brush_free.app/brush_free
mach-o decryption dumper

DISCLAIMER: This tool is only meant for security research purposes, not for application crackers.

[+] detected 64bit ARM binary in memory.
[+] offset to cryptid found: @0x100a05028(from 0x100a04000) = 1028
[+] Found encrypted data at address 00004000 of length 6275072 bytes - type 1.
[+] Opening /private/var/containers/Bundle/Application/EA717F2A-A0F7-4988-BA96-95B5830539B0/brush_free.app/brush_free for reading.
[+] Reading header
[+] Detecting header type
[+] Executable is a plain MACH-O image
[+] Opening brush_free.decrypted for writing.
[+] Copying the not encrypted start of the file
[+] Dumping the decrypted data into the file
[+] Copying the not encrypted remainder of the file
[+] Setting the LC_ENCRYPTION_INFO->cryptid to 0 at offset 1028
[+] Closing original file
[+] Closing dump file
Cchukou:~/Containers/Data/Application/D627C61F-4A0C-4DA3-978D-BD710C904571/Documents mobile$ ls
brush_free.decrypted
```

8）将生成的.decrypted导出

```shell
# Mac
$ scp root@10.94.51.82:/var/mobile/Containers/Data/Application/D627C61F-4A0C-4DA3-978D-BD710C904571/Documents/brush_free.decrypted .
```

### 2. dump

导出的`.decrypted`实质上是一个解密过的`Mach-O`可执行文件

```shell
$ file brush_free.decrypted
brush_free.decrypted: Mach-O 64-bit executable arm64
```

1）使用`class-dump`导出头文件

```shell
class-dump -H brush_free.decrypted -o brush_header
```









### 静态库分析

https://blog.csdn.net/acwqzy/article/details/84147677

### Crash

https://github.com/Tencent/OOMDetector

https://github.com/didi/DoraemonKit

https://juejin.cn/post/6844903938618195981

https://blog.csdn.net/tugele/article/details/104004692

https://www.zhihu.com/question/426404169/answer/1535126072)/

## 操作

### 相关命令

```shell
# 查看架构
$ lipo -info BioAuthEngine.framework
# 分离出某一个架构
$ lipo BioAuthEngine.framework/BioAuthEngine -thin arm64 -o BioAuthEngine64
# 分离静态库
$ ar -x BioAuthEngine64
# 查看二进制文件中的strings
$ strings qihooloan_ios | grep --color=auto -ir "checkCachedLicense"
# 查看当前环境的SDK版本
xcodebuild -showsdks
```

