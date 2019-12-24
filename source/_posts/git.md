---
title: 细数平时工作中用到的git命令
date: 2018-12-06 13:34:00
top: true
author: damon
cover: true
categories: git
tags:
  - git
---

## **git基本操作命令**

### 添加git仓库

```html
git init
git remote add origin <origin地址>
```
或者
```html
git clone <origin地址>
```

### git分支管理
> 查看分支列表
 ```html
git branch -a
 ```

> 创建新分支
 ```html
git branch <branch名称>
 ```

> 切换分支
```html
git check <branch名称>
```

然后，上面两个命令也可以合成为一个命令
```html
git checkout -b <branch名称> ,
```
这里经常有使用到git checkout -b origin <branch名称>，创建本地分支并拉取远程到本地

### git使用
> 查看仓库有变化文件
```html
git status
```
> 添加文件到索引库
```html
git add <file带路径的名称>
```
这里使用git add .,提交当前库中有变化的文件到索引库中
> 提交added状态的文件，即添加到索引库中的文件
```html
git commit -m '<注释>'
```
> 提交远程仓库
```html
git push origin <branch名称>
```
这里如果当前分支只有一个追踪分支，git push即可。远程主机名可以忽略
> 拉取分支更新文件
```html
git pull origin <branch名称>
```
如果当前分支与主机存在追踪关系，git pull origin即可。如果只有一个追踪分支，远程主机名忽略 git pull即可
> 删除分支
```html
git branch -d <branch名称>
```
如若要删除远程分支，git push --delete origin <branch名称>
### git合并
> 合并分支
示例一:将分支master合并到当前分支中，但不要自动进行新的提交
```html
git merge --no-commit master
```
示例二:合并到当前分支中，保留分支提交记录不自动提交
```html
git merge --no-ff master
```
示例三:合并到当期分支,自动进行提交
```html
git merge master
```
> 合并冲突解决
```html
setp1:更新并拉取远程到本地
git fetch origin
git checkout -b origin/master

step2:切换拉取远程需被合并的分支，如已切换作pull操作
git checkout -b origin <branch名称>

step3:示例二操作，合并master到当前分支 不提交
git merge --no-ff master

step4:推送到远程主机
git push origin <branch名称>
```

### git小知识
> 查看日志提交记录
```html
git log
```
> 还在工作区，并未进入暂存区可以执行撤销操作
```html
>git checkout .

>git reset --hard
```
> 查看尚未暂存的文件更新了哪些部分
```html
git diff <带路径的file>
```
> 查看暂存区和本地仓库之间的差异
```html
git diff --cached
```
> 删除文件
```html
git rm -r <带路径的file>
```
> 查看当前本地分支与远程分支的关联
```html
git branch -v
```
> 标签
```html
git tag 
```
创建标签则带参数-a，git tag -a <tag名称>

> git pull 或者git push有时候会出现此提示
```bash
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master
```
done:
```git
git branch --set-upstream-to=origin/<远程分支名字>
```
>git 回滚到之前某提交commit-id
```
git reset --hard commit-id :回滚到commit-id，讲commit-id之后提交的commit都去除
git reset --hard HEAD~3：将最近3次的提交回滚
```

## git进阶篇
[git子模块](https://github.com/DamonNie/git-Submodule)