# 一、初识GIT 

## 1.1简介（来源百度百科） 

Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
Torvalds 开始着手开发 Git 是为了作为一种过渡方案来替代 BitKeeper，后者之前一直是 Linux 内核开发人员在全球使用的主要源代码工具。开放源码社区中的有些人觉得 BitKeeper 的许可证并不适合开放源码社区的工作，因此 Torvalds 决定着手研究许可证更为灵活的版本控制系统。尽管最初 Git 的开发是为了辅助 Linux 内核开发的过程，但是我们已经发现在很多其他自由软件项目中也使用了 Git。例如 最近就迁移到 Git 上来了，很多 Freedesktop 的项目也迁移到了 Git 上。

## 1.2 特点 ##
分布式相比于集中式的最大区别在于开发者可以提交到本地，每个开发者通过克隆（git clone），在本地机器上拷贝一个完整的Git仓库。

下图是经典的git开发过程。

![](http://i.imgur.com/u7vQGIC.jpg)

Git的功能特性：

从一般开发者的角度来看，git有以下功能：
    
    1、从服务器上克隆完整的Git仓库（包括代码和版本信息）到单机上。
    2、在自己的机器上根据不同的开发目的，创建分支，修改代码。
    3、在单机上自己创建的分支上提交代码。
    4、在单机上合并分支。
    5、把服务器上最新版的代码fetch下来，然后跟自己的主分支合并。
    6、生成补丁（patch），把补丁发送给主开发者。
    7、看主开发者的反馈，如果主开发者发现两个一般开发者之间有冲突（他们之间可以合作解决的冲突），就会要求他们先解决冲突，然后再由其中一个人提交。如果主开发者可以自己解决，或者没有冲突，就通过。
    8、一般开发者之间解决冲突的方法，开发者之间可以使用pull 命令解决冲突，解决完冲突之后再向主开发者提交补丁。
    
从主开发者的角度（假设主开发者不用开发代码）看，git有以下功能：
    
    1、查看邮件或者通过其它方式查看一般开发者的提交状态。
    2、打上补丁，解决冲突（可以自己解决，也可以要求开发者之间解决以后再重新提交，如果是开源项目，还要决定哪些补丁有用，哪些不用）。
    3、向公共服务器提交结果，然后通知所有开发人员。
    
优点：
	适合分布式开发，强调个体。
	公共服务器压力和数据量都不会太大。
	速度快、灵活。
	任意两个开发者之间可以很容易的解决冲突。
	离线工作。
缺点：
	资料少（起码中文资料很少）。
	学习周期相对而言比较长。
	不符合常规思维。
	代码保密性差，一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息。
	
# 二、安装GIT并连接github #

## 2.1 查看Linux是否已安装 ##

	[root@Siffre gitlearn]# rpm -qa  git
	git-1.7.1-3.el6_4.1.x86_64
	如果没有安装git，如下过程安装
	[root@Siffre gitlearn]# yum  install  git -y
	[root@Siffre gitlearn]# rpm -qa  git
	git-1.7.1-3.el6_4.1.x86_64
	
## 2.2 Linux连接github ##
### 2.2.1注册github账户 ###

如：账户名“Siffre”，邮箱“git@github.com”

![](http://i.imgur.com/1ihUufl.png)

### 2.2.2创建你的Repository代码库 ###

右上角点击加号选“New Repository”，输入“Repository Name”和其他相关信息，并点击“Create repository”。

![](http://i.imgur.com/0DWO6ZL.jpg)


### 2.2.3 在“.ssh”目录下生成RSA公私钥对 ###
	
	[root@Siffre gitlearn]# mkdir ~/.ssh   #创建.ssh目录
	[root@Siffre gitlearn]# ssh-keygen -t rsa -C  "952974923@qq.com"  #注册邮箱
	Generating public/private rsa key pair.
	Enter file in which to save the key (/root/.ssh/id_rsa): 
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again:    #一路enter即可
	Your identification has been saved in /root/.ssh/id_rsa.
	Your public key has been saved in /root/.ssh/id_rsa.pub.
	The key fingerprint is:
	c5:a2:fa:bd:13:dc:7c:ef:0f:70:12:84:75:81:75:96 952974923@qq.com
	The key's randomart image is:
	+--[ RSA 2048]----+
	|           ooo+.+|
	|         ..... E |
	|        . o .    |
	|       . o   .   |
	|      ..So  o .  |
	|     .  o o .+   |
	|    .    . . ..  |
	|     . ..     .. |
	|      . oo   ....|
	+-----------------+

### 2.2.4 在github上设置ssh公钥 ###

（1）打开github账户，打开“Setting-->SSH keys”页面，并点击“Add SSH key”

（2）在“Title”栏命名连接，在“key”栏输入刚刚在Linux上生成的公钥文件“ id_rsa.pub ”
	
	[root@Siffre .ssh]# ls
	id_rsa  id_rsa.pub
	[root@Siffre .ssh]# cat  id_rsa.pub 
	ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA2SY5rSSMysbuTu3D6wYPmBXfx6AzYqxAPYKIYoCIHywLoOLyr661BTiYuN9sSokJpat34/iQGv/r0opbWoZ3S3wXoVOJRohyvggS33/wjl2uf2YEr/8hl+Uvh6O2JzIlFKB1wZJalR/9WjWu3otAzGdkrYDpLuzKYk3MfYbzgmXo7NcuWiP529pUcZw1bmqWT6L9XJRD5+7VRRxqEDlBFV87G/k+3gEx7CIXH6LBvD2gVgXcCegEDpPI8oiYf+ZEa8ioaiXKUKqOXm71wyxYqvYYwwru2PtTcBn93lOAfHz7ch3WF0WTpMWA7FTQ4yKe+cuuNC2V2jIfcNhGrBR39Q== 952974923@qq.com
	
如图所示：

![](http://i.imgur.com/uz3dS6l.jpg)


### 2.2.5 连接github ###
	
	[root@Siffre gitlearn]# ssh -T git@github.com  #这里是固定地址
	The authenticity of host 'github.com (192.30.252.129)' can't be established.
	RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	Are you sure you want to continue connecting (yes/no)? yes  #输入yes
	Warning: Permanently added 'github.com,192.30.252.129' (RSA) to the list of known hosts.
	Hi Siffre! You've successfully authenticated, but GitHub does not provide shell access.   #出现这句话表示成功了！
	
# 三、GIT使用 #

## 3.1基础命令查询 ##
	
	[root@Siffre gitlearn]# git
	usage: git [--version] [--exec-path[=GIT_EXEC_PATH]] [--html-path]
	           [-p|--paginate|--no-pager] [--no-replace-objects]
	           [--bare] [--git-dir=GIT_DIR] [--work-tree=GIT_WORK_TREE]
	           [--help] COMMAND [ARGS]
	
	The most commonly used git commands are:
	   add        Add file contents to the index
	   bisect     Find by binary search the change that introduced a bug
	   branch     List, create, or delete branches
	   checkout   Checkout a branch or paths to the working tree
	   clone      Clone a repository into a new directory
	   commit     Record changes to the repository
	   diff       Show changes between commits, commit and working tree, etc
	   fetch      Download objects and refs from another repository
	   grep       Print lines matching a pattern
	   init       Create an empty git repository or reinitialize an existing one
	   log        Show commit logs
	   merge      Join two or more development histories together
	   mv         Move or rename a file, a directory, or a symlink
	   pull       Fetch from and merge with another repository or a local branch
	   push       Update remote refs along with associated objects
	   rebase     Forward-port local commits to the updated upstream head
	   reset      Reset current HEAD to the specified state
	   rm         Remove files from the working tree and from the index
	   show       Show various types of objects
	   status     Show the working tree status
	   tag        Create, list, delete or verify a tag object signed with GPG
	
	See 'git help COMMAND' for more information on a specific command.

## 3.2实战操作 ##

参考：http://lvwzhen.gitbooks.io/git-tutorial/content/chapter-1/1-2.html

### 3.2.1 创建版本库 ###

在创建版本库之前，我们先要做点事情：
由于git是分布式版本控制系统，所有每个机器必须自报家门：你的名字和Email地址。
    
    [root@Siffre gitlearn]# git config  --global user.name  "Siffre"
    [root@Siffre gitlearn]# git config  --global user.email  "952974923@qq.com"
    注：--global参数，表示本机器上所有的git仓库都会使用这个配置，我们也可以对某个仓库指定不同的用户名和Email地址。详情查看git config --help
    
创建版本库
    
    [root@Siffre /]# mkdir  gitlearn
    [root@Siffre /]# cd  gitlearn/
    [root@Siffre gitlearn]# pwd
    /gitlearn
    
    [root@Siffre gitlearn]# git  init  #将目录变成GIT可管理的仓库
    Initialized empty Git repository in /gitlearn/.git/
    提示：这里多了.git目录：GIT跟踪管理版本库的，没事不要改动即可！
    
### 3.2.2 添加并提交文件 ###

创建一个文件

    [root@Siffre gitlearn]# vim  readme.txt
    
添加文件

    [root@Siffre gitlearn]# git  add   readme.txt 
    
提交到github仓库

    [root@Siffre gitlearn]# git  commit  -m "read file"
    [master (root-commit) b7ebbeb] read file
     1 files changed, 2 insertions(+), 0 deletions(-)
     create mode 100644 readme.txt
     
注：也可以添加多个文件，一次性提交

    [root@Siffre gitlearn]# git add file1.txt 
    [root@Siffre gitlearn]# git add file2.txt 
    [root@Siffre gitlearn]# git add file3.txt 
    [root@Siffre gitlearn]# git commit  -m "3 files"
    [master aa13fa7] 3 files
     3 files changed, 3 insertions(+), 0 deletions(-)
     create mode 100644 file1.txt
     create mode 100644 file2.txt
     create mode 100644 file3.txt
以上过程完好的解释了，GIT提交为何需要add、commit两个步骤。
    
### 3.2.3 版本更新 ###
前面我们已经成功的提交一个readme.txt文件，现在，我们来改动文件内容：
    
    [root@Siffre gitlearn]# vim   readme.txt 
    Git is a distributed version control system.
    Git is free software.
    
更改后，我们来查看结果：
	
	[root@Siffre gitlearn]# git  status
	# On branch master
	# Changed but not updated:  #已经更改但未提交
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	modified:   readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")

有时，我们可能记不清更改的内容了，可以通过下列命令查看：
    
    [root@Siffre gitlearn]# git  diff  readme.txt
    diff --git a/readme.txt b/readme.txt
    index 46d49bf..9247db6 100644
    --- a/readme.txt
    +++ b/readme.txt   #更改内容如下：
    @@ -1,2 +1,2 @@
    -Git is a version control system.   #“-”表示删除了这句话
    +Git is a distributed version control system.  #“+”表示添加这句话
     Git is free software.   #未改动

然后，我们就可提交了：
    
    [root@Siffre gitlearn]# git add  readme.txt 
    [root@Siffre gitlearn]# git  commit  -m  "change" 
    [master 77c24bd] change
     1 files changed, 1 insertions(+), 1 deletions(-)
    [root@Siffre gitlearn]# git  status
    # On branch master
    nothing to commit (working directory clean)  #提示当前没有需要提交的修改，而且目录是干净的。
    
### 3.2.4 版本回退 ###

修改文件并提交到GIT版本库，我们已经学会了。那让我们来回顾一下提交的几个版本吧：

版本1：
	
	Git is a version control system.Git is free software.
	
版本2：
	
	Git is a distributed version control system.Git is free software.
	
当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干嘛。这时，我们可以通过如下命令查看历史记录：
    
    [root@Siffre gitlearn]# git log
    commit 77c24bd3413788f3cc69048c7852a5cf968c17a2  #第一次
    Author: Siffre <952974923@qq.com>
    Date:   Sun Aug 2 15:16:37 2015 +0800
    
    Change  #操作记录
    
    commit aa13fa7c4b2ec4a09cb164d40b551a29f13876af  #第二次
    Author: Siffre <952974923@qq.com>
    Date:   Sun Aug 2 15:05:14 2015 +0800
    
    3 files
    commit b7ebbeb9fc39472777013f6bfe70cd1d5bc36dd1  #第三次
    Author: Siffre <952974923@qq.com>
    Date:   Sun Aug 2 15:00:01 2015 +0800
    
    read file
    
    [root@Siffre gitlearn]# cat   readme.txt  #查看更改后的文件内容
    Git is a distributed version control system.
    Git is free software.
    单行显示方式：
    [root@Siffre gitlearn]# git  log  --pretty=oneline
    77c24bd3413788f3cc69048c7852a5cf968c17a2 change
    aa13fa7c4b2ec4a09cb164d40b551a29f13876af 3 files
    b7ebbeb9fc39472777013f6bfe70cd1d5bc36dd1 read fil
    (END) 
    按“q”退出，即可。

好了，现在我们准备把readme.txt会退到上一个版本：
	
	[root@Siffre gitlearn]# git reset --hard HEAD^ #这里先用，后面讲
	HEAD is now at aa13fa7 3 files
	
	[root@Siffre gitlearn]# git log
	commit aa13fa7c4b2ec4a09cb164d40b551a29f13876af
	Author: Siffre <952974923@qq.com>
	Date:   Sun Aug 2 15:05:14 2015 +0800
	
	    3 files
	
	commit b7ebbeb9fc39472777013f6bfe70cd1d5bc36dd1
	Author: Siffre <952974923@qq.com>
	Date:   Sun Aug 2 15:00:01 2015 +0800
	
	read file
	注：和刚刚的相比是不是少了一个：change
	
	[root@Siffre gitlearn]# cat  readme.txt   #内容已经回退了
	Git is a version control system.
	Git is free software.
	
那么问题来了，这只是各位数的回退，多位数的回退怎么搞？教父Linus早就想到了！
	
	[root@Siffre gitlearn]# git  reflog
	aa13fa7 HEAD@{0}: HEAD^: updating HEAD  #commit id 是aa13fa7
	77c24bd HEAD@{1}: commit: change
	aa13fa7 HEAD@{2}: commit: 3 files
	b7ebbeb HEAD@{3}: commit (initial): read file

知道了commit id，我们就可以随意回退、再回退啦！

我们再回到之前的change状态：
	
	[root@Siffre gitlearn]# git  reset --hard 77c24bd
	HEAD is now at 77c24bd change
	
	[root@Siffre gitlearn]# git  status  #查看状态
	# On branch master
	nothing to commit (working directory clean)
	
	[root@Siffre gitlearn]# cat  readme.txt   #查看内容
	Git is a distributed version control system.
	Git is free software.
	
	[root@Siffre gitlearn]# git  log
	commit 77c24bd3413788f3cc69048c7852a5cf968c17a2
	Author: Siffre <952974923@qq.com>
	Date:   Sun Aug 2 15:16:37 2015 +0800
	
	    Change    #回复到知道之前的状态了
	
	commit aa13fa7c4b2ec4a09cb164d40b551a29f13876af
	Author: Siffre <952974923@qq.com>
	Date:   Sun Aug 2 15:05:14 2015 +0800
	
	    3 files
	
	commit b7ebbeb9fc39472777013f6bfe70cd1d5bc36dd1
	Author: Siffre <952974923@qq.com>
	Date:   Sun Aug 2 15:00:01 2015 +0800
	
	    read file
	    
## 3.3工作区和暂缓区 ##

工作区：你本地电脑能看到的目录，如：之前建立的gitlearn就是一个工作区。

版本库：工作区有一个隐藏目录“.git”，这个不算工作区，而是GIT 的版本库。

GIT的版本库里存了很多东西，其中最重要的是称为stage（或index）的暂存区，还有GIT为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![](http://i.imgur.com/iCnBQsL.jpg)

**实战流程**：

（1）对之前的readme.txt做个修改，如下：
    
    [root@Siffre gitlearn]# cat   readme.txt 
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    
（2）添加LICENSE文本文件，内容随意：

    [root@Siffre gitlearn]# cat   LICENSE.txt 
    zhengshu
    
（3）查看状态（两个文件都未提交到暂存区）
    
    [root@Siffre gitlearn]# git  status
    # On branch master
    # Changed but not updated:   #已改，但未提交
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   readme.txt
    #
    # Untracked files:   #原状态的文件
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	LICENSE.txt
    no changes added to commit (use "git add" and/or "git commit -a")
    
（4）将readme.txt提交到暂存区，再查看状态
    [root@Siffre gitlearn]# git  add  readme.txt 
    [root@Siffre gitlearn]# git  status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   readme.txt
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	LICENSE.txt
    
（5）将readme.txt提交到仓库，查看状态
    [root@Siffre gitlearn]# git  commit -m "change 3" 
    [master 552162d] change 3
     1 files changed, 2 insertions(+), 1 deletions(-)
    
    [root@Siffre gitlearn]# git  status
    # On branch master
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	LICENSE.txt
    nothing added to commit but untracked files present (use "git add" to track)
    
（6）接下来对LICENSE.txt做以上操作

    [root@Siffre gitlearn]# git  add   LICENSE.txt 
    [root@Siffre gitlearn]# git   commit  -m "where"
    [master 2a9cebe] where
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 LICENSE.txt
    [root@Siffre gitlearn]# git  status
    # On branch master
    nothing to commit (working directory clean)   #版本库变成了这样，暂存区没有任何内容了
    
从上的操作流程我们知道了GIT的文件的三种状态及转换过程，如图所示：

![](http://i.imgur.com/lO1SqUN.jpg)

## 3.4管理修改 ##

为什么说GIT管理的是修改，而不是文件呢？实践来证明一切。

（1）更改文件：
	
	[root@Siffre gitlearn]# cat   readme.txt 
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	
（2）添加到暂存区

	[root@Siffre gitlearn]# git   add readme.txt 
	[root@Siffre gitlearn]# git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	modified:   readme.txt
	#
（3）然后在修改readme.txt
	
	[root@Siffre gitlearn]# cat   readme.txt 
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
	
	[root@Siffre gitlearn]# git commit  -m "git tracks changes"
	[master 62d88be] git tracks changes
	 1 files changed, 4 insertions(+), 2 deletions(-)
	
	[root@Siffre gitlearn]# git status
	# On branch master
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	modified:   readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")
	注：发现没有，commit只是把暂存区的内容提交到版本库了，第二次的修改并不会被提交。
	
（4）查看工作区和版本库最新版的区别
	
	[root@Siffre gitlearn]# git diff HEAD -- readme.txt 
	diff --git a/readme.txt b/readme.txt
	index 76d770f..a9c5755 100644
	--- a/readme.txt
	+++ b/readme.txt
	@@ -1,4 +1,4 @@
	 Git is a distributed version control system.
	 Git is free software distributed under the GPL.
	 Git has a mutable index called stage.
	-Git tracks changes.
	+Git tracks changes of files.
	注：可见，第二次的修改确实没有提交
	
	小结：每次修改，如果不add暂存区，那就不会加入到commit中。

## 3.5 撤销修改 ##

人是会犯错误的哦！
当你乱改工作区某个文件的内容，想直接丢弃工作区的修改

（1）更改内容如下：

    [root@Siffre gitlearn]# cat  readme.txt 
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.
    My stupid boss still prefers SVN.
    
（2）查看状态
    
    [root@Siffre gitlearn]# git  status
    # On branch master
    # Changed but not updated:  #改动未提交
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)   #这里已经提示了
    #
    #	modified:   readme.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

（3）丢弃工作区的修改

状态一：文件修改后，未放到工作区（续上述流程）
    
    [root@Siffre gitlearn]# git checkout -- readme.txt 
    [root@Siffre gitlearn]# git  status
    # On branch master
    nothing to commit (working directory clean)
状态二：文件修改后，放到暂存区

    [root@Siffre gitlearn]# git add  readme.txt 
    [root@Siffre gitlearn]# git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    # #这里同样提示了
    #	modified:   readme.txt
    #
    
    [root@Siffre gitlearn]# git  reset HEAD readme.txt 
    Unstaged changes after reset:
    M	readme.txt
    
    [root@Siffre gitlearn]# git  status
    # On branch master
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   readme.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")
    注：退回到了工作去状态
    
    [root@Siffre gitlearn]# git  checkout --  readme.txt 
    [root@Siffre gitlearn]# git status
    # On branch master
    nothing to commit (working directory clean)

状态三：提交到版本库

假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得版本回退一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把“stupid boss”提交推送到远程版本库，你就真的惨了……

## 3.6 删除文件 ##

（1）添加一个新文件test.txt到GIT并且提交：
    
    [root@Siffre gitlearn]# vim  test.txt
    
（2）提交并删除
	
	[root@Siffre gitlearn]# git  add   test.txt 
	[root@Siffre gitlearn]# git commit  -m "add  test"
	[master 872dcae] add  test
	 1 files changed, 1 insertions(+), 0 deletions(-)
	 create mode 100644 test.txt
	
	[root@Siffre gitlearn]# rm   test.txt   #删除
	rm: remove regular file `test.txt'? Yes
	
	[root@Siffre gitlearn]# git  status
	# On branch master
	# Changed but not updated:
	#   (use "git add/rm <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	deleted:    test.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")

根据上述操作流程，我们有两个选择：

A.确实要从版本库删除该文件
    
    [root@Siffre gitlearn]# git  rm  test.txt
    rm 'test.txt'
    [root@Siffre gitlearn]# git  commit -m "remove"
    [master f9c8119] remove
     1 files changed, 0 insertions(+), 1 deletions(-)
     delete mode 100644 test.txt
    [root@Siffre gitlearn]# git status
    # On branch master
    nothing to commit (working directory clean)

B.误删，因为版本库里面还有，所以很轻松的把误删文件恢复到最新版本

    [root@Siffre gitlearn]# git   checkout  --  test.txt
    [root@Siffre gitlearn]# ls
    file1.txt  file3.txt   test.txt
    file2.txt  readme.txt
    
# 四、远程仓库 #
## 4.1添加远程库 ##

现在我们使用的就是远程仓库状态，本地创建一个GIT仓库和github创建一git仓库，并且让这两个仓库同步，这样Github上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。当然我们并没有开始就实现两个仓库的同步，只是实现了简单的连接到github账户。

（1）首先，登陆Github，然后找到“Create a new repo”按钮，创建一个新的仓库：

![](http://i.imgur.com/HgM4IcN.jpg)

输入自己的仓库名：

![](http://i.imgur.com/OL28GaF.jpg)

已经成功创建了一空的仓库：

![](http://i.imgur.com/amC80tx.jpg)

在Github上的这个gitlearn仓库还是空的，Github告诉我们，乐意从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库内容推送到Github仓库：
	
	[root@Siffre gitlearn]# git remote  add origin
	  git@github.com:Siffre/gitlearn.git   #origin自己定义即可
	
	[root@Siffre gitlearn]# git push -u origin  master
	Warning: Permanently added the RSA host key for IP address '192.30.252.128' to the list of known hosts.
	Counting objects: 16, done.
	Delta compression using up to 2 threads.
	Compressing objects: 100% (11/11), done.
	Writing objects: 100% (16/16), 1.23 KiB, done.
	Total 16 (delta 4), reused 0 (delta 0)
	To git@github.com:Siffre/gitlearn.git
	 * [new branch]      master -> master
	Branch master set up to track remote branch master from origin.

到Github上查看仓库内容：

![](http://i.imgur.com/KEb9E0R.jpg)


把本地内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送到远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或拉取时就可以简化命令。
从现在起，只要本地作了提交，秩序通过如下命令：

    [root@Siffre gitlearn]# vi  hello1.txt 
    
    [root@Siffre gitlearn]# git  add  hello1.txt 
    
    [root@Siffre gitlearn]# git commit -m "hello"
    [master e78faee] hello
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 hello1.txt
    
    [root@Siffre gitlearn]# git  push  origin master
    Counting objects: 4, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 264 bytes, done.
    Total 3 (delta 1), reused 0 (delta 0)
    To git@github.com:Siffre/gitlearn.git
       be6e399..e78faee  master -> master
       
** 小结：**

1、关联一远程库，使用命令：
	
	git remote  add origin git@github.com:Siffre/gitlearn.git

2、关联后，使用命令推送


    git push -u origin  master
3、以后的操作简化命令：

    git  push  origin master
    
## 4.2 从远程库克隆 ##

首先要在远程库Github，创建一新的仓库名，名字叫gitskill。

![](http://i.imgur.com/8ojiols.jpg)

如果勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：

![](http://i.imgur.com/xv6Nq5B.jpg)


现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地仓库：
    
    [root@Siffre gitlearn]# git  clone  git@github.com:Siffre/gitskill.git
    Initialized empty Git repository in /gitlearn/gitskill/.git/
    warning: You appear to have cloned an empty repository.
    [root@Siffre gitlearn]# ls
    file1.txt  file3.txt  hello1.txt  test.txt
    file2.txt  gitskill   readme.txt
    注：勾选上述选项，会出生成相应的README.md文件
    
# 五、分支管理 #
## 5.1创建与合并分支 ##
### 5.1.1 介绍 ###

在版本回退里，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的。所以HEAD指向的就是当前分支。
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：

![](http://i.imgur.com/cv7TsyJ.jpg)

每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。当我们创建新的分支，例如：dev，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上

![](http://i.imgur.com/t7gx3R8.jpg)

Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件没有任何变化。不过从现在开始，对工作区的修改和提交就是针对dev分支了，比如：新提交一次后，dev指针往前移一步，而master指针不变：

![](http://i.imgur.com/F2dcdvW.jpg)


假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并？最简单的，就是直接把master指向dev的当前提交，就完成了合并:

![](http://i.imgur.com/qOtlI5N.jpg)

所以Git合并分支也很快，就是改改指针，工作区内容也不变。
合并玩分支后，甚至可以删除dev分支，删除dev分支就是把dev指针给删掉，删掉后，我们就剩下一条master分支：

![](http://i.imgur.com/CwikVd6.jpg)

### 5.1.2实战 ###

（1）创建dev分支，然后切换到dev分支

	[root@Siffre gitlearn]# git  checkout -b dev   #-b参数表示创建并切换到分支
	Switched to a new branch 'dev'
	
	注：相当于：
	[root@Siffre gitlearn]# git   branch  dev   #创建分支
	fatal: A branch named 'dev' already exists.
	[root@Siffre gitlearn]# git checkout  dev   #切换到分支
	Already on 'dev'
	
（2）查看当前分支
	
	[root@Siffre gitlearn]# git  branch
	* dev
	  master 
	注：git branch会列出所有分支，当前分支前面会表一个*号
	
	[root@Siffre gitlearn]# git  branch  dev1
	[root@Siffre gitlearn]# git  branch  
	* dev
	  dev1
	  master
（3）在分支上提交修改文件
	
	[root@Siffre gitlearn]# cat  readme.txt 
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	Creating a new branch is quick.  #添加此句
	
提交
	
	[root@Siffre gitlearn]# git  add  readme.txt 
	You have new mail in /var/spool/mail/root
	[root@Siffre gitlearn]# git  commit -m "branch"
	[dev 0f861bd] branch
	 1 files changed, 1 insertions(+), 0 deletions(-)
	 
（4）返回到master

	[root@Siffre gitlearn]# git  checkout  master
	Switched to branch 'master'
	
	[root@Siffre gitlearn]# cat readme.txt  #查看文件
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	注：刚刚添加的内容不见了！因为是在dev分支上提交的，而master分支此刻的提交点并没有改变。
	
（5）合并

	[root@Siffre gitlearn]# git  merge dev
	Updating e78faee..0f861bd
	Fast-forward   #快进模式，还有其他模式
	 readme.txt |    1 +
	 1 files changed, 1 insertions(+), 0 deletions(-)
	
	提示：git merge命令用于合并指定的分支到当前分支。合并后，在查看文件内容，和dev分支的内容完全一样：
	
	[root@Siffre gitlearn]# cat   readme.txt 
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	Creating a new branch is quick.
	
（6）合并完后，可以删除分支

	[root@Siffre gitlearn]# git  branch -d dev
	Deleted branch dev (was 0f861bd).
	
	[root@Siffre gitlearn]# git branch
	  dev1
	* master     #dev分支已经被删除

小结：

	查看分支：git branch
	
	创建分支：git branch name
	
	切换分支：git checkout name
	
	创建并切换分支：git checkout -b name
	
	合并某分支到当前分支：git merge name
	
	删除分支：git branch -d name
	
	
## 5.2 解决冲突 ##

解决合并分支的冲突问题！

（1）创建新的test1分支

	[root@Siffre gitlearn]# git  checkout -b  test1
	Switched to a new branch 'test1'
	
（2）修改文件并提交

	[root@Siffre gitlearn]# cat  readme.txt 
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	Creating a new branch is quick.
	Creating a new branch is quick AND simple.
	
	[root@Siffre gitlearn]# git  add  readme.txt 
	[root@Siffre gitlearn]# git commit  -m "test1"
	[test1 9f71313] test1
	 1 files changed, 1 insertions(+), 0 deletions(-)
	 
（3）切换到master

	[root@Siffre gitlearn]# git  checkout   master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 1 commit.
	[root@Siffre gitlearn]# 
	[root@Siffre gitlearn]# git  branch
	  dev1
	* master
	  test1
	Git还会自动提示我们当前master分支比远程的master分支要超前一个提交。
	
（4）在master分支上把readme.txt文件的最后一行改为：

	[root@Siffre gitlearn]# cat readme.txt 
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	Creating a new branch is quick & simple.
	
提交
	
	[root@Siffre gitlearn]# git  add  readme.txt 
	[root@Siffre gitlearn]# git  commit -m "master"
	[master a3feba3] master
	 1 files changed, 1 insertions(+), 1 deletions(-)
	 
（5）这时，我们合并master、test1分支
	
	[root@Siffre gitlearn]# git  merge  test1
	Auto-merging readme.txt
	CONFLICT (content): Merge conflict in readme.txt  #冲突了
	Automatic merge failed; fix conflicts and then commit the result.
	
	上述过程发生了冲突，必须手动解决冲突后在提交：
	[root@Siffre gitlearn]# git  status
	# On branch master
	# Your branch is ahead of 'origin/master' by 2 commits.
	#
	# Unmerged paths:
	#   (use "git add/rm <file>..." as appropriate to mark resolution)
	#
	#	both modified:      readme.txt
	#
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#	.good.txt.swp
	#	.hello.txt.swn
	#	.hello.txt.swo
	#	.readme.txt.swp
	#	.succes.txt.swp
	#	.testone.txt.swp
	no changes added to commit (use "git add" and/or "git commit -a")
	查看两个分支更改的内容
	[root@Siffre gitlearn]# git   diff  master  test1diff --git a/readme.txt b/readme.txt
	index 17356fe..4d61d8d 100644
	--- a/readme.txt
	+++ b/readme.txt
	@@ -2,4 +2,5 @@ Git is a distributed version cont
	 Git is free software distributed under the GPL.
	 Git has a mutable index called stage.
	 Git tracks changes.
	-Creating a new branch is quick & simple.
	+Creating a new branch is quick.
	+Creating a new branch is quick AND simple.
	直接查看文件
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
	<<<<<<< HEAD
	Creating a new branch is quick & simple.
	=======
	Creating a new branch is quick.
	Creating a new branch is quick AND simple.
	>>>>>>> test1
	修改master中的readme.txt内容，同test1中文件内容
	[root@Siffre gitlearn]# git  add  readme.txt 
	[root@Siffre gitlearn]# git  commit -m "conflict" 
	[master 0ed4cb4] conflict
	（6）查看合并情况
	[root@Siffre gitlearn]# git log --graph  --pretty=oneline  --abbrev-commit    #--graph 图形显示
	*   0ed4cb4 conflict
	|\  
	| * 9f71313 test1
	* | a3feba3 master     #合并
	|/  
	* 0f861bd branch
	* e78faee hello
	* be6e399 test file
	* f9c8119 remove
	* 872dcae add  test
	* 62d88be git tracks changes
	* aa13fa7 3 files
	* b7ebbeb read file
	* 
（7）删除分支

	[root@Siffre gitlearn]# git  branch  -d  test1
	Deleted branch test1 (was 9f71313).
	
## 5.3分支管理策略 ##
