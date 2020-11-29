# npm修改配置文件

## 查看npm配置

```shell
$ npm config ls
; cli configs
metrics-registry = "https://registry.npm.taobao.org/"
scope = ""
user-agent = "npm/6.14.9 node/v14.15.1 darwin x64"

; userconfig /Users/360jr/.npmrc
prefix = "/usr/local"
registry = "https://registry.npm.taobao.org/"

; node bin location = /usr/local/bin/node
; cwd = /usr/local
; HOME = /Users/360jr
; "npm config ls -l" to show all defaults.
```

## 恢复默认配置

删除上述 userconfig的值

## npm 换源

```shell
$ npm config set registry http://mirrors.cloud.tencent.com/npm/
```

## 修改npm -g 默认安装位置

```shell
$ npm config set prefix '/usr/local'
```

默认安装位置为 /usr/local/lib/node_modules

查看默认安装位置

```shell
$ npm root -g
```

