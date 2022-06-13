## Git简介
---
Git的官方网站：https://git-scm.com/

+ Linux内核开源项目有着为数众广的参与者。一开始整个项目组使用Bitkeeper来管理和维护代码。2005年, BitKeeper不再能免费使用,这就迫使Linux开源社区开发一套属于自己的版本控制系统。
+ 自诞生于2005年以来, Git日臻成熟完善,它的速度飞快,极其适合管理大型项目,它还有着令人难以置信的非线性分支管理系统,可以应付各种复杂的项目开发需求。

### CVS、SVN与Git
+ 集中式版本控制管理系统（CVCS）
+ 分布式管理控制系统（DVCS）
+ Git是一个版本控制软件
+ GitHub与GitLab都是用于管理版本的服务端软件
+ GitHub提供免费服务（代码需公开）及付费服务（代码为私有）
+ GitLab用于在企业内部管理Git版本库，功能上类似于GitHub
### Git设计目标
+ 快速
+ 高效存储
+ 简单
+ 完全分布
+ 满足大规模项目需要
### Git工作模式
+ 版本库初始化
	+ 个人计算机从版本服务器同步
+ 操作
	+ 90%以上的操作在个人计算机上
	+ 添加文件
	+ 修改文件
	+ 提交变更
	+ 查看版本历史等
+ 版本库同步
	+ 将本地修改推送到版本服务器

![[Pasted image 20210911090657.png]]

### Git文件存储
![[Pasted image 20210911091242.png]]
+ 上图：SVN维护的是增量的变化（增量值得就是增加的数据，在维护版本的时候需要和之前版本做差异性的比较）
+ 下图：Git维护的是全量的变化(全量值得就是所有数据，好处就是维护版本的时候不需要做差异性的比较，速度快)，图中虚线指的就是没有做更改的文件
### Git基础概念
+ 直接记录快照，而非差异比较（也就是记录更改后的全部的数据）
+ 近乎所有操作都在本地执行
+ 时刻保持数据完整性
+ 多数操作仅添加数据（对文件多次操作，只添加变化的数据）
+ 文件的三种状态
	+ 已修改（Modified）
	+ 已暂存（Staged）
	+ 已提交（Committed）
### Git文件状态
+ Git文件
	+ 已被版本库管理的文件
+ 已修改
	+ 在工作目录修改Git文件
+ 已暂存
	+ 对已修改的文件执行Git暂存操作，将文件存入暂存区
+ 已提交
	+ 将已暂存的文件执行Git提交操作，将文件存入版本库

![[Pasted image 20210911093017.png]]
![[Pasted image 20210911093650.png]]
** 本地版本库与服务器版本库**
![[Pasted image 20210911093740.png]]
### Git安装
+ Linux (Ubuntu)
	+ sudo apt-get install git
+ Mac
	+ 安装命令行工具(如已安装Xcode,命令行工具会在首次启动code时提示安装)
	+ homebrew
	+ macports
+ Windows
	+ 通过官网下载安装https://git-scm.com/download/win
	+ 完成安装之后,就可以使用命令行的git工具(已经自带了ssh客户端)了,另外还有一个图形界面的Git项目管理工具
	+ 建议使用Git命令行,方便又快捷, GUI反而繁琐
	+ 如果需要使用GUI,推荐使用SourceTree,拥有Mac与Windows版本;此外, Windows下还可以使用TortoiseGit

### Git常用命令
+ 获得版本库
	+ git init
	+ git clone
+ 查看信息
	+ git help
	+ git log
	+ git diff
+ 版本管理
	+ git add
	+ git commit
	+ git rm
+ 远程协作
	+ git pull
	+ git push
## Git重要命令操练
tips: 在初始化前设置你的git的user.name和user.email
**设置的作用域如下：**
+ /etc/gitconfig（几乎不会使用），git config --system
+ ~/.gitconfig（很常用），git config --global
+ 针对于特定项目的，.git/config文件中 git config --local

**示例：**
git config --local user.name '202252197'
git config --local user.email '202252197@qq.com'

### 基本流程
1.在一个空的文件夹中使用git init 初始化本地版本库
```
在git中当我们调用获取版本库的命令的时候，git会帮我们自动创建一个默认的分支，这个分支就是主分支（master
```
2.使用touch lvshihao.txt创建一个文件，并添加hello lvshihao
3.使用git status查看当前git的状态
```
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        lvshihao.txt

nothing added to commit but untracked files present (use "git add" to track)
```
4.使用git add lvshihao.txt将文件添加到暂存区
5.再次查看状态git status
```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   lvshihao.txt

use "git restore --staged <file>..." to unstage 这句话的意思是，你可以使用git restore --staged xxx将文件恢复到工作区
```
6.将文件提交到版本库使用git commit lvshihao.txt，此时git会让你输入提交描述内容，不输则不会提交。
7.再次查看状态git status
```
$ git status
On branch master
nothing to commit, working tree clean
```
8.使用git log查看提交的日志
```
$ git log
commit 79a24e21f90e386d1bde362d16b5b01c5c6ce708 (HEAD -> master)
Author: 202252197 <202252197@qq.com>
Date:   Sat Sep 11 21:35:52 2021 +0800

    commit lvshihao.txt
```
可以看到commit后面是一个id，它就是一个摘要值，这个摘要值实际上是个sha1计算出来的。
### Git还原命令
**Git的工作区更改的内容撤回**
1.使用git restore xxx将工作区修改的内容删除还原
2.使用git checkout -- xxx同上操作一样
```
两者的区别：
1.git checkout –- <file_name>是将暂存区的修改重新放回工作区，但只能操作文件内容，不能添加、删除文件；
2.git restore --staged <file_name>相当于撤销git add 命令，git restore <file_name> 是放弃对工作区的修改，对文件的操作（添加、删除）和文件内容的操作都能使用此命令；
```
**Git的暂存区回退到工作区**
+ 可以使用git restore --staged xxx将暂存区的文件回退到工作区，不会删除修改的文件内容
+ 也可以使用git reset HEAD <file_name>将你暂存区的文件恢复到工作区
### Git删除命令
```
git rm <file_name>可以删除不想要的文件

```