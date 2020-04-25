---
title: Git学习笔记
toc: true
date: 2018-03-04 17:55:55
tags: git
categories:
---

# Git简介

Git是分布式版本控制系统，简单来说就是能记录每次文件的改动，如下图

![git](/images/git.png)

## 优点

- 分布式版本控制：每个人的电脑上都是一个完整的版本库，不用依赖于中央服务器仓库
- 分支管理：可以在脱机环境下多人同时协作开发
<!--more-->
# 版本控制

## 创建版本库

1. 新建一个空目录

   ```sh
   mkdir learngit
   cd learngit
   ```


2. 初始化

   ```sh
   git init
   ```

## 编辑版本库

### 添加文件到版本库

1. 文件添加到暂存区

   ```sh
   git add readme.txt
   ```

2. 提交到当前分支

   ```sh
   git commit -m "wrote a readme file"
   ```

### 撤销修改

1. 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时

   ```sh
   git checkout -- file
   ```

2. 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改

   ```sh
   git reset HEAD file
   回到情况1
   ```

3. 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库

### 删除文件

- 从版本库中删除该文件

  ```sh
  git rm file
  git commit -m "remove file"
  ```

- 如果工作区被删除，可从版本库恢复文件到最新版本，你会丢失最近一次提交后你修改的内容

  ```sh
  git checkout -- file
  ```

  ​

## 查看版本库

- 查看工作区的状态

  ```sh
  git status
  ```

- 查看工作区与暂存区之间区别

  ```sh
  git diff readme.txt
  ```

- 查看工作区和版本库里面最新版本的区别

  ```sh
  git diff HEAD -- readme.txt
  ```

- 查看从最近到最远的提交日志，以便确定要回退到哪个版本

  ```sh
  git log
  ```

- 不同版本之间穿梭

  ```sh
  git reset --hard HEAD
  ```

|    HEAD     |  当前版本    |
| :----------:| :-----  :   |
|    HEAD^    |  上个版本    |
|    HEAD^^   |  上上个版本  |
|   HEAD~100  | 上100个版本  |
| 版本号(只需输入前6位) |  相应版本   |

- 查看命令历史，以便确定要回到未来的哪个版本

  ```sh
  git reflog
  ```

## Git原理

Git跟踪并管理的是修改，而非文件

- 目录learngit = 工作区 + 版本库(.git目录)

![0](/images/0.jpg)

- HEAD指向当前分支

  ![master](/images/master.png)

- 新建了一个指针叫dev,HEAD指向当前分支在dev

  ![dev](/images/dev.png)

## 远程仓库<->本地仓库

### 本地仓库->远程仓库

- 关联一个远程库

  ```sh
  git remote add origin git@github.com:用户名/repo-name.git
  ```

- 第一次推送master分支的所有内容

  ```sh
  git push -u origin master
  ```

- 推送最新修改

  ```sh
  git push origin master
  ```

### 本地仓库<-远程仓库

- ```sh
  git clone 仓库地址
  ```

# 分支管理

## 创建及编辑分支

- 查看分支

  ```sh
  git branch
  ```

- 创建分支

  ```sh
  git branch <name>
  ```

- 切换分支

  ```sh
  git checkout <name>
  ```

- 创建+切换分支

  ```sh
  git checkout -b <name>
  ```

- 合并某分支到当前分支

  ```sh
  git merge <name>
  ```

- 删除分支

  ```sh
  git branch -d <name>
  ```

## 解决分支冲突

- 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再`git add`和`git commit`，合并完成

- 查看分支的合并情况

  ```sh
  git log --graph --pretty=oneline --abbrev-commit
  ```

## 分支管理策略

- 默认模式(fast forward)合并dev分支

  ```sh
  git merge dev
  ```

- 普通模式合并

  ```sh
  git merge --no-ff -m "merge with no-ff" dev
  ```

  注：合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward模式合并就看不出来曾经做过合并

### bug分支:临时修复bug

1. 储藏当前工作现场

   ```sh
   git stash
   ```

2. bug修复完成后，查看储藏列表

   ```sh
   git stash list
   ```

3. 恢复

   ```sh
   恢复但不删除stash内容
   git stash apply
   恢复并且删除stash内容
   git stash pop
   ```

### 新功能分支

- 开发一个新feature，最好新建一个分支

- 丢弃一个没有合并过的分支

  ```bash
  git branch -D <branchname>
  ```

## 多人协作

### 推送push

- 推送主分支

  ```sh
  git push origin master
  ```

- 推送其他分支

  ```sh
  git push origin dev
  ```

### 推送原则

- master分支是主分支，因此要时刻与远程同步
- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发

### 拉取分支到本地

- ```sh
  git pull
  ```


- 在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致

  ```sh
  git checkout -b dev origin/dev
  ```

- 建立本地分支和远程分支的关联(如果git pull提示“no tracking information”)

  ```sh
  git branch --set-upstream branch-name origin/branch-name
  ```

### 解决推送冲突

1. 首先，可以尝试推送自己的修改

   ```sh
   git push origin branch-name
   ```

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并

3. 如果合并有冲突，则解决冲突，并在本地提交

4. 没有冲突或者解决掉冲突后再次推送

   ```bash
   git push origin branch-name
   ```

# 标签管理
## 创建及查看标签
tag就是一个让人容易记住的有意义的名字，它跟某个commit id绑在一起

- 新建一个标签

  ```
  git tag <tagname>
  ```

- 查看所有标签

  ```
  git tag v1.0
  ```

- 给历史提交的commit id（6224937）打标签

  ```
  git tag v0.9 6224937
  ```

- 查看标签信息

  ```
  git show v0.9
  ```

- 创建带有说明的标签

  ```
  git tag -a v0.1 -m "version 0.1 released" 3628164
  ```

- 用私钥签名一个标签

  ```
  git tag -s v0.2 -m "signed version 0.2 released" fec145a
  ```

## 修改标签

- 删除标签

  ```
  git tag -d v0.1
  ```

- 推送标签到远程

  ```
  git push origin <tagname>
  ```

- 一次性推送全部尚未推送到远程的本地标签

  ```
  git push origin --tags
  ```

- 删除远程标签

  1. 先删除本地标签

     ```
     git tag -d v0.9
     ```

  2. 删除远程标签

     ```
     git push origin :refs/tags/v0.9
     ```

     ​

# 参考资料
> - [Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
> - [Git笔记](https://github.com/hongiii/gitNotes_from_Liao/blob/master/gitNotes_from_Liao.md)
> - [常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
> - [Git 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)
> - [Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
> - [Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
