# homebrew 换源

## 使用腾讯源

测试了下，腾讯源比较快，这里使用腾讯源

```shell
替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.cloud.tencent.com/homebrew/brew.git

替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.cloud.tencent.com/homebrew/homebrew-core.git

注意：这步未验证
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"
git remote set-url origin https://mirrors.cloud.tencent.com/homebrew/homebrew-cask.git
```

更新 bottles源

对于bash用户：

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.cloud.tencent.com/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```

对于zsh用户:

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.cloud.tencent.com/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

这里记录下，我安装到了zsh的配置文件中

## 更新

```shell
brew update
之前brew有问题，这里使用update-reset
brew update-reset
```

## 问题解决

* brew 执行出现如下问题

```shell
Traceback (most recent call last):
 11: from /usr/local/Homebrew/Library/Homebrew/brew.rb:23:in `<main>'
 10: from /usr/local/Homebrew/Library/Homebrew/brew.rb:23:in `require_relative'
  9: from /usr/local/Homebrew/Library/Homebrew/global.rb:37:in `<top (required)>'

```

解决步骤：

1. 替换腾讯源
2. brew update-reset

## 参考文献

[mac brew更换国内源](https://www.jianshu.com/p/bea984d27cd2)

[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

[腾讯源](https://mirrors.cloud.tencent.com/)

