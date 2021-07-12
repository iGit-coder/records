#### Git

> Git：是一个免费的开源分布式版本控制系统，旨在快速高效地处理从小型到大型项目的所有内容。
>
> 在Git中，我们将需要进行版本控制的文件目录叫做一个**仓库（repository）**，每个仓库可以简单理解成一个目录，这个目录里面的所有文件都通过Git来实现版本管理，Git都能跟踪并记录在该目录中发生的所有更新。
>
>  现在我们已经知道什么是repository（缩写repo）了，假如我们现在建立一个仓库（repo），那么在建立仓库的这个目录中有一个“.git”的文件夹。这个文件夹非常重要，所有的版本信息，更新记录，以及Git进行仓库管理的相关信息全部保存在这个文件夹里面。所以，不要修改/删除其中的文件，以免造成数据的丢失。
>

>
>  根据上面的图片，下面给出了每个部分的简要说明：
>
> - Directory：使用Git管理的一个目录，也就是一个仓库，包含我们的工作空间和Git的管理空间。
> - WorkSpace：需要通过Git进行版本控制的目录和文件，这些目录和文件组成了工作空间，除了.git之外的都属于工作区。
> - .git：存放Git管理信息的目录，初始化仓库的时候自动创建。
> - Index/Stage：暂存区，或者叫待提交更新区，在提交进入repo之前，我们可以把所有的更新放在暂存区。
> - Local Repo：本地仓库，一个存放在本地的版本库；HEAD会只是当前的开发分支（branch）。
> - Stash：是一个工作状态保存栈，用于保存/恢复WorkSpace中的临时状态。

1. 下载地址：`https://git-scm.com/download/win`
2. 下载github代码：`git clone https://github.com/cxinping/PyQt5.git`
3. 上传本地代码：
## 本地已存在
```shell
cd records #records目录下是需要上传的项目
git init #初始化git管理
git add . #把records目录下的所有文件都添加进暂存区
git commit -m 'records初次提交' #将暂存区的内容存到本地仓库
git remote -v #查看本地远程库，初次查询应该是没有
git remote add git@github.com:iGit-coder/jvscode.git #地址是github上ssh的地址
git push origin master # 将本地版本库推送到远程服务器，origin是远程主机名(默认)，master表示远程服务器上的main分支，分支名可以修改，只要githb上有就行
```
## 本地不存在
```shell
1.可以先在github上创建一个对应仓库
2.将仓库克隆到本地，克隆下来的文件夹就是被git管理的
3.将需要上传的文件复制到克隆的文件夹里
4.远程提交即可
```

4. 上传本地更新的文件

```shell
git status #显示当前目录下已经更新的文件(与暂存区的区别)
git add -u #添加修改过的文件到索引库
git status #再次检查
git commit -m "修改" #将修改从暂存区提交到本地版本库
git push #将本地版库的分支推送到远程服务器上对应的分支
git pull #从远程获取最新版本并merge到本地
```

