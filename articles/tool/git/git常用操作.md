# Git常用操作

## 刷新.gitignore

当文件已经被git跟踪时，.gitignore不生效，需要清除缓存重新跟踪

```shell
git rm -r --cached .

git add .

git commit -m 'update .gitignore'
```

## 修改全局配置

```shell
git config  --global user.name xxx
git config  --global user.email xxx
```

## oh-my-zsh git 慢/卡顿问题

[oh-my-zsh slow, but only for certain Git repo](https://stackoverflow.com/questions/12765344/oh-my-zsh-slow-but-only-for-certain-git-repo)

```shell
$ git config --add oh-my-zsh.hide-dirty 1
$ git config --add oh-my-zsh.hide-status 1
```

