# brew基本使用

## 安装卸载

[中文官网](https://brew.sh/index_zh-cn)

## Homebrew 的两个术语

- Formulae：软件包，包括了这个软件的依赖、源码位置及编译方法等；
- Casks：已经编译好的应用包，如图形界面程序等。

## Homebrw相关的几个文件夹用途

- bin：用于存放所安装程序的启动链接（相当于快捷方式）
- etc：brew安装程序的配置文件默认存放路径
- Library：Homebrew 系统自身文件夹
- Cellar：通过brew安装的程序将以 [程序名/版本号] 存放于本目录下

## 常用的 brew 命令

- 查看brew版本：brew -v
- 更新brew版本：brew update
- 本地软件库列表：brew list
- 查看软件库版本：brew list --versions
- 查找软件包：brew search xxx （xxx为要查找软件的关键词）
- **安装软件包：brew install xxx （xxx为软件包名称）**
- 卸载软件包：brew uninstall xxx
- **安装软件：brew cask install xxx（xxx为软件名称）**
- 卸载软件：brew cask uninstall xxx
- 查找软件安装位置：which xxx （xxx为软件名称）

## brew install 和 brew cask install

### brew install

brew 是下载源码解压，然后 ./configure && make install ，同时会包含相关依存库，并自动配置好各种环境变量。

### brew cask install

brew cask 是针对已经编译好了的应用包（.dmg/.pkg）下载解压，然后放在统一的目录中（Caskroom），省掉了自己下载、解压、安装等步骤。

> 简单来说
>
> - brew install 用来安装一些不带界面的命令行工具和第三方库。
> - brew cask install 用来安装一些带界面的应用软件。

