# Ruby & Cocoapods

$ ruby --version

rvm & rbenv 用于安装管理使用多个ruby环境。两个工具本质都是在path上做手脚，一个在执行前，一个在执行中

rvm管理版本。新手建议不用

$ rvm list 查看本机ruby list

$ rvm list known   查看当前rvm可管理的ruby版本，如果rvm版本过低，不一定能管理到当前的ruby版本

$ rvm install ruby-2.x.x 安装rvm可管理的指定版本的ruby

$ rvm install ruby --head 能够安装的最新版本

$ rvm get stable     rvm升级



ruby的第三方库，Gem形式发布

$ gem search -r/-f <gem>

$ gem install <gem> --version <num>

$ gem list



Bundler

Gemfile  -> bundle install  -> Gemfile.lock

Gemfile

source 'https://rubygems.org'    # 源

gem 'xxx' , path: '../..'

gem 'xxx', '1.1.1



ruby调试

ruby-debug-ide   这是一个gem。这是一个中间件，用于ruby调试信息在调试器和ide中间传递

debase  这是ruby调试器，断点、调用栈等等



```shell
$ gem info cocoapods

*** LOCAL GEMS ***

cocoapods (1.10.1, 1.10.0, 1.9.1)
    Authors: Eloy Duran, Fabio Pelosin, Kyle Fuller, Samuel Giddins
    Homepage: https://github.com/CocoaPods/CocoaPods
    License: MIT
    Installed at (1.10.1): /Library/Ruby/Gems/2.6.0
                 (1.10.0): /Library/Ruby/Gems/2.6.0
                 (1.9.1): /Library/Ruby/Gems/2.6.0

    The Cocoa library package manager.
```

gem源

```shell
gem source
gem source -l
```

