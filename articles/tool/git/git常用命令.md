# Git常用命令

## git diff

* 看看你今天写了多少行代码

```shell
git diff --shortstat "@{0 day ago}"
# 4 files changed, 47 insertions(+), 3 deletions(-)
```

* 增量包

```shell
git diff b1dfbcc893dbb5d232235c3c8a313f570ff590ae b7ce5b8f0144e10b4ef1c4fcc30af067810ee657 | xargs tar -czvf update.tar.gz
```

```shell
git diff 78a12defeb251ecf218f41661b6b435 721699b0f9db5756ea84977b9de13 --name-only | xargs zip update.zip
```

```shell
git diff e1571c141c659 d3e15e29a934d
git diff e1571c141c659 d3e15e29a934d -name-only
```

