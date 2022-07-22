---
layout: post
title: 常用 Git 命令集
tags: [git]
date: 2022-07-21 17:50:00 +800
---


> 本文收集了一些常用的 Git 命令，方便查找

<!--more-->

## 本地分支关联远程分支
1. 查看远程分支和其最近提交
```bash
git branch -vv
```
2. 查看远程分支
```bash
git branch -r
```
3. 存在关联错误的情况先删除
```bash
git remote remove origin/master
```
4. 关联正确分支
```bash
git branch --set-upstream-to=origin/dev dev
```

## 创建远端分支
1. 创建一个本地分支
```bash
git checkout -b [branch_name]
```
2. 创建远端分支
```bash
git push origin [remote_branch_name]
```
3. 将本地分支与远程同名分支关联
```bash
git push --set-upstream origin [local_branch_name]
```

## 本地暂存
1. 添加要保存文件
```bash
git add .
```
2. 保存
```bash
git stash save "a message"
```
3. 恢复  

先查看：如果有多个stash内容，这里可以选择恢复哪一个
```bash
git stash list
stash@{0}: On dev: saved_changes
```
方法1：这种情况保存的内容仍然保留
```bash
git stash apply stash@{0}
```
方法2：这种情况不会保留已保存内容
```bash
git pop
```

## 提交另外一个分支上的某个提交
对于多分支的代码库，将代码从一个分支转移到另一个分支是常见需求。
这时分两种情况。一种情况是，你需要另一个分支的所有代码变动，那么就采用合并（git merge）。另一种情况是，你只需要部分代码变动（某几个提交），这时可以采用 Cherry pick。
http://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html

## 回退到某次提交
- 未提交到远程
```bash
git reset --soft HEAD^
```
说明：HEAD^ 表示上一个版本，即上一次的 commit，也可以写成 HEAD~1。
如果进行两次的 commit，想要都撤回，可以使用 HEAD~2

- 已提交到远程，回滚
```bash
git reset --hard commit_id
```
回退到commit_id这次的提交，包含这次
```bash
git push --force
```
强制推到远端

## 更改远程仓库指向
1. 查看远程当前远程库地址：
```bash
git remote -v
```
2. 重新设置远程库地址
```bash
git remote set-url origin http://120.221.160.107:3000/medtech_web/lrhealth_ms.git
git remote set-url origin http://120.221.161.88:9001/medtech_web/cloudImage-webH5.git
```

## 删除分支
- 删除本地
```bash
git branch -D [分支名]
```
- 删除远程分支
```bash
git push origin --delete [分支名]
```

## 切换远端 git 仓库地址
```bash
git remote set-url origin http://mingzhanghui@192.168.10.57/r/OnlineEdu/bgms-web.git
```

## 打 tag
- 创建 tag
```bash
git tag [tagName]
```

- 推到远程
```bash
git push origin [tagName]
```

- 删除本地 tag
```bash
git tag -d [tagName]
```

- 删除远程 tag
```bash
git push orign :refs/tags/[tagName]
```

