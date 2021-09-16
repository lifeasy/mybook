# 前言

> 士不可以不弘毅 任重而道远

## 运行

* 安装gitbook
* 运行`gitbook install`
* build `gitbook build`
* 本地运行 `gitbook serve`

## 部署到服务器

* 目前直接通过git上传到服务端

1、在服务端创建git仓库

```shell
$ cd /var/repo
$ git init --bare gitbookBlog.git
$ ls
gitbookBlog.git  hexoBlog.git
```

2、设置post-receive钩子

```shell
$ cd gitbookBlog.git
$ cd hooks
$ touch post-receive
$ vi post-receive
# 根据服务路径自行配置
# git --work-tree=/var/www/html/myGitbook --git-dir=/var/repo/gitbookBlog.git checkout -f
$ chmod +x post-receive
```

3、配置本地的gitbook的git config，增加remote

```shell
# 测试加在第一个，source tree不受影响，source tree读取的是最后一个url，但是同时都会上传
[remote "origin"]
	url = root@1.2.3.4:/var/repo/gitbookBlog
	url = git@github.com:lifeasy/mybook.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

参考：[gitbook部署到服务器](https://blog.shuifengche.top/2019/11/22/gitbook%E9%83%A8%E7%BD%B2%E5%88%B0%E6%9C%8D%E5%8A%A1%E5%99%A8/#%E7%AC%AC%E4%BA%8C%E6%AD%A5%EF%BC%8C%E8%AE%BE%E7%BD%AEpost-receive%E9%92%A9%E5%AD%90)

