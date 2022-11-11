---
title: git常用命令
date: 2022-10-10 22:02:05
tags: 学习
category: 学习
cover: https://qiansen.oss-cn-hangzhou.aliyuncs.com/git常用命令.jpg
---

### git add

- 把指定的文件添加到暂存区中

  git	add	<文件路径>

- 添加所有修改、已删除的文件到暂存区中

  git	add	-u	<文件路径>

  git	add	--update	<文件路径>

- 添加所有修改、已删除、新增的文件到暂存区中，省略 <文件路径> 即为当前目录

  git	add	-A	<文件路径>

  git	add	--all	<文件路径>

- 查看所有修改、已删除但没有提交的文件，进入一个子命令系统

  git	add	-i	<文件路径>

  git	add	--interactive	<文件路径>

### git branch

操作 Git 的分支命令。

- 列出本地的所有分支，当前所在分支以 "*" 标出

  git	branch

- 列出本地的所有分支并显示最后一次提交，当前所在分支以 "*" 标出

  git	branch	-v

- 创建新分支，新的分支基于上一次提交建立

  git	branch	<分支名>

- 修改分支名称(如果不指定原分支名称则为当前所在分支)

  git	branch	-m	<原分支名称>	<新的分支名称>

- 强制修改分支名称

  git	branch	-M	<原分支名称>	<新的分支名称>

- 删除指定的本地分支

  git	branch	-d	<分支名称>

- 强制删除指定的本地分支

  git	branch	-D	<分支名称>

### git checkout

更新工作树中的文件以匹配索引或指定树中的版本。如果没有给出路径 - `git checkout` 还会更新 `HEAD` ，将指定的分支设置为当前分支。

- 切换到已存在的指定分支

  git	checkout	<分支名称>

- 创建并切换到指定的分支，保留所有的提交记录(等同于 "git branch" 和 "git checkout" 两个命令合并)

  git	checkout	-b	<分支名称>

- 创建并切换到指定的分支，删除所有的提交记录

  git	checkout	--orphan	<分支名称>

### git commit

将索引的当前内容与描述更改的用户和日志消息一起存储在新的提交中。

- 把暂存区中的文件提交到本地仓库，调用文本编辑器输入该次提交的描述信息

  ​	git	commit

- 把暂存区中的文件提交到本地仓库中并添加描述信息

  ​	git	commit	-m	'<提交的描述信息>'

- 修改上次提交的描述信息

  ​	git	commit	--amend

### git log

显示提交的记录。

- 打印所有的提交记录

  git	log

- 打印从第一次提交到指定的提交的记录

  git	log	<commit ID>

- 打印指定数量的最新提交的记录

  git	log	-<指定的数量>

### git pull

从远程仓库获取最新版本并合并到本地。 首先会执行 `git fetch`，然后执行 `git merge`，把获取的分支的 HEAD 合并到当前分支。

### git push

把本地仓库的提交推送到远程仓库。

- 把本地仓库的分支推送到远程仓库的指定分支

  git	push	<远程仓库的别名>	<本地分支名> ：<远程分支名>

### git remote

操作远程库。

- 列出已经存在的远程仓库

  git	remote

- 列出远程仓库的详细信息，在别名后面列出URL地址

  git	remote	-v

  git	remote	--verbose

- 添加远程仓库

  git	remote	add	<远程仓库的别名>	<远程仓库的URL地址>

- 修改远程仓库的别名

  git	remote	rename 	<原远程仓库的别名> 	<新的别名>

- 删除指定名称的远程仓库

  git	remote	remove	<远程仓库的别名>

- 修改远程仓库的 URL 地址

  git	remote	set-url	<远程仓库的别名>	<新的远程仓库URL地址>

  