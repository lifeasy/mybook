# Gitbook创建

## 安装

```shell
$ npm install gitbook-cli -g
```

## 查看安装后的版本

```shell
# 注意大写的V
$ gitbook -V 
```

```shell
# 首次会安装
$ gitbook --version
CLI version: 2.3.2
Installing GitBook 3.2.3
  SOLINK_MODULE(target) Release/.node
  CXX(target) Release/obj.target/fse/fsevents.o
  SOLINK_MODULE(target) Release/fse.node
  SOLINK_MODULE(target) Release/.node
  CXX(target) Release/obj.target/fse/fsevents.o
  SOLINK_MODULE(target) Release/fse.node
/...

# 安装完成
$ gitbook --version
CLI version: 2.3.2
GitBook version: 3.2.3
```

## 写作

### 初始化

```shell
$ gitbook init
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md
info: initialization is finished
```

新建了两个文件，其中**SUMMARY.md**为目录

### 预览

```shell
i$ gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 1 pages
info: found 0 asset files
info: >> generation finished with success in 0.5s !

Starting server ...
Serving book on http://localhost:4000
```

### 配置文件

```json
{
    "title": "lifeasy 的文档库",
    "author": "Liang_QL",
    "description": "随便写写",
    "language": "zh-hans",
    "styles": {
      "website": "./public-repertory/css/gitbook-configure.css"
    },
    "links": {
      "sidebar": {
          "我的小站": "https://www.liangqili.com"
      }
    },
    "plugins": [
      "theme-comscore",
      "prism",
      "-highlight",
      "copy-code-button",
      "search-pro",
      "-search",
      "-lunr",
      "expandable-chapters",
      "splitter",
      "-sharing",
      "github",
      "github-buttons",
      "donate",
      "tbfed-pagefooter",
      "anchor-navigation-ex"
    ],
    "pluginsConfig": {
        "github": {
            "url": "https://github.com/lifeasy"
        },
      "github-buttons": {
        "buttons": [
          {
            "user": "lifeasy",
            "repo": "mybook", 
            "type": "star",
            "count": true,
            "size": "small"
          }, 
          {
            "user": "lifeasy",
            "width": "160", 
            "type": "follow", 
            "count": true,
            "size": "small"
          }
        ]
      },
      "donate": {
        "button": "打赏",
        "alipayText": "支付宝打赏",
        "wechatText": "微信打赏",
        "alipay": "./source/images/alipay.jpeg",
        "wechat": "./source/images/wxpay.jpeg"
      },
      "prism": {
        "css": [
          "prismjs/themes/prism-solarizedlight.css"
        ]
      },
      "tbfed-pagefooter": {
        "copyright":"Copyright &copy lifeasy.top 2020",
        "modify_label": "该文件修订时间：",
        "modify_format": "YYYY-MM-DD HH:mm:ss"
      },
      "anchor-navigation-ex": {
        "showLevel": false
      }
    }
  }
```

## 转换

### 安装ebook-convert

生成电子书 (epub, mobi, pdf) 时需要ebook-convert

[App下载地址](https://calibre-ebook.com/download)

下载完成后可创建符号连接到一个环节变量地址中，方便命令行调用

```shell
$ sudo ln -s /Applications/calibre.app/Contents/MacOS/ebook-convert /usr/local/bin
```

### 转pdf、epub、mobi

```shell
$ gitbook epub
$ gitbook pdf
$ gitbook mobi
```

* 目前存在排版和乱码问题，后续解决

## 存在问题

 * ```shell
   TypeError: cb.apply is not a function
       at /usr/local/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js:287:18
       at FSReqCallback.oncomplete (fs.js:184:5)
   ```

   和node版本有关

   解决：

    1. node降级

    2. 注释`/usr/local/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js`62-64行

       ```js
       fs.stat = statFix(fs.stat)
       fs.fstat = statFix(fs.fstat)
       fs.lstat = statFix(fs.lstat)
       ```

## 参考文献

[官方文档](https://github.com/GitbookIO/gitbook/blob/master/docs/setup.md)

[GitBook - 快速打造可留言的博客 | 插件介绍详细](https://juejin.cn/post/6844903848914452488)

[GitBook 使用教程 | 入门教程](https://www.jianshu.com/p/421cc442f06c)

[安装ebook-convert](http://caibaojian.com/gitbook/build/ebookconvert.html)