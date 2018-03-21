# git 命令

## git clone 克隆一个仓库到一个新目录

### 概要
```
  git clone [--template=<template_directory>]
    [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
    [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
    [--dissociate] [--separate-git-dir <git dir>]
    [--depth <depth>] [--[no-]single-branch] [--no-tags]
    [--recurse-submodules] [--[no-]shallow-submodules]
    [--jobs <n>] [--] <repository> [<directory>]
```

### 描述

1. 最简单直接的命令
git clone xxx.git
2. 如果想clone到指定目录
git clone xxx.git "指定目录"
3. clone时创建新的分支替代默认Origin HEAD（master）
git clone -b [new_branch_name]  xxx.git
4. clone 远程分支
git clone 命令默认的只会建立master分支，如果你想clone指定的某一远程分支(如：dev)的话，可以如下：
　A. 查看所有分支(包括隐藏的)  git branch -a 显示所有分支，如：　　　　
    * master
      remotes/origin/HEAD -> origin/master
      remotes/origin/dev
      remotes/origin/master
  B.  在本地新建同名的("dev")分支，并切换到该分支
    git checkout -t origin/dev 该命令等同于：
    git checkout -b dev origin/dev
