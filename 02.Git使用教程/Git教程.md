[TOC]

# 1.Git教程

## 1.1Git版本管理工具简介

Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理、版本管理。也是[Linus Torvalds](https://baike.baidu.com/item/Linus Torvalds/9336769?fromModule=lemma_inlink)为了帮助管理Linux内核开发而开发的一个开放源码的版本控制软件。Torvalds 着手开发 Git 是为了作为一种过渡方案来替代 BitKeeper 。

Git和SVN是我们最常用的版本控制系（Version Control System，VCS），当然，除了这二者之外还有许多其他的VCS，例如早期的CVS等。顾名思义，版本控制系统主要就是控制、协调各个版本的文档内容的一致性，这些文档包括但不限于代码文件、图片文件等等。早期SVN占据了绝大部分市场，而后来随着Git的出现，越来越多的人选择将它作为版本控制工具，社区也越来越强大。相较于SVN，最核心的区别是Git是分布式的VCS，简而言之，每一个你pull下来的Git仓库都是主仓库的一个分布式版本，仓库的内容完全一样，而SVN则不然，它需要一个中央版本库来进行集中控制。采用分布式模式的好处便是你不再依赖于网络，当有更改需要提交的时候而你又无法连接网络时，你只需要把更改提交到本地的Git仓库，最后有网络的时候再把本地仓库和远程的主仓库进行同步即可。

 ![a71ea8d3fd1f4134ca7667d8251f95cad0c85ed6](https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicturea71ea8d3fd1f4134ca7667d8251f95cad0c85ed6.png)

## 1.2Git的工作原理

### 1.2.1Git四个区域功能简介

Git本地有4个工作区域，分别是工作目录、暂存区、本地仓库、远程仓库。

1. Workspace：工作区，平时存放项目代码的地方。工作区就是你克隆项目到本地后，项目所在的文件夹目录
2. Index/Stage：用于存储工作区中添加上来的变更（新增、修改、删除）的文件的地方。操作时，使用git add .会将本地所有新增、变更、删除过的文件的情况存入暂存区中。
3. Repository：用于存储本地工作区和暂存区提交上来的变更（新增、修改、删除）过的文件的地方。操作时，使用git commit –m “本次操作描述” 可以将添加到暂存区的修改的文件提交到本地仓库中。
4. Remote：远程仓库，当某一个人的开发工作完毕时，需要将自己开发的功能合并到主项目中去，但因为功能是多人开发，如果不能妥善保管好主项目中存储的代码及文件的话，将会存在丢失等情况出现，所以不能将主项目放到某一个人的本地电脑上，这时就需要有一个地方存储主项目，这个地方就是我们搭建在服务器上的git远程仓库（GitHub、Gitee），也就是在功能开始开发前，每个人要下载项目到本地的地方。操作时，使用git push origin 分支名称，将本次仓库存储的当前分支的修改推送至远程仓库中的对应分支中。

### 1.2.2Git四个区域之间的关系  

git的工作流程一般是这样的：

１、在工作目录中添加、修改文件；

２、将需要进行版本管理的文件放入暂存区域；

３、将暂存区域的文件提交到git仓库。

因此，git管理的文件有三种状态：已修改（modified）,已暂存（staged）,已提交(committed)

<img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicturemypictureSnipaste_2023-08-10_00-18-10.png" alt="Snipaste_2023-08-10_00-18-10" style="zoom:67%;" /> 

<img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mac_image/git_status-5729938.png" alt="image" style="zoom: 33%;" /> 

从上图可以看到，我们如果想将在本地工作区中修改，推送到远程仓库的话，需要将工作区的修改的内容，添加到暂存区，再将暂存区的内容提交到本地仓库，最终将本地仓库的内容推送至远程仓库，才能达到最终想要将本地修改推送到远程仓库的目的。 

 

## 1.3Git中分支的简介

### 1.3.1分支概念简介

简单来说，就是我们工作过程中，要开发一个系统，这个系统会由若干个功能组成，我们将若干个功能交由多个人进行开发。每个人在开发之前，都会将项目从远程仓库下载到本地，然后才能在本地进行对应功能的代码编写。此时，每个人就可以看作是一个分支。每个人在其分支中进行着功能开发，最终开发完毕后，需要将开发的功能代码推送到远程仓库进行代码合并，远程仓库中才能有我们开发的功能。

### 1.3.2分支如何使用

在本地仓库中，可以创建多个分支，在多个分支中进行不同的功能开发，来满足业务需求。

在开发完功能后，为了保证本地仓库推送到远程仓库的功能代码，不会出现将其他人开发的功能代码覆盖的情况，需要在每次使用git push origin 分支名称 命令将当前分支中，在本地仓库改动推送到远程仓库之前，需要先将远程仓库的主干分支master的最新代码拉取到本地当前分支的本地仓库中，再进行推送操作，从而保证最终推送本地仓库代码到远程仓库时，推送的代码是完整的（即包含其他人提交的功能的）。

> 注意：开发过程中，必须创建自己分支进行功能开发，不允许直接在master分支中进行功能开发、修改、删除等操作。以免误操作或操作出错等情况出现，污染了远程仓库的主干分支master，导致功能代码无法继续使用，也会影响到其他人的使用。

## 1.4Git操作相关命令

### 1.4.1Git在Ubuntu上的安装

安装命令：`sudo apt-get install git`

### 1.4.2Git初始化

`git config --global user.name "名字"`    #设置名字

`git config --global user.email "xxx@xxx.com"`        #设置设置邮箱

`git config --list`          #查看设置的信息

`git init`     #初始化一个git仓库，在当前目录下会产生一个.git目录，目录下就是版本管理关键信息

### 1.4.3Git本地操作

`git add 文件/目录`  

将文件或者目录从工作区添加到暂存区

`git add .`     

#将工作区的所有内容添加到暂存区（有种通配符的意味）

`git rm [-r] 文件/目录`

(等价rm file+git add file)#删除工作区的文件，并将本次删除放入暂存区

`git rm --cached file`

#放弃文件的跟踪状态，将文件从暂存区，本地仓库和远程仓库全部删除，但是本地依然存留此文件

`git mv 源文件的名字 新文件的名字`

#将文件的名字修改，并存入暂存区

`git status`

 #查看变更文件的状态

`git commit`

 #将暂存区中的改动提交到本地仓库，在提交的时候会生成一个40位的哈希值，这个哈希

#值在版本回退时非常有作用，它相当于一个快照，在未来的某个时间点可以通过git reset

#命令回到这里

`git commit -m "message"`
#将暂存区的改动提交到本地仓库，这里通过-m "message"给本次提交一个描述字段

`git commit -a`

#将以追踪的文件直接从工作区提交到本地仓库，即使没有经过git add .。没有被跟踪的

#文件依然不会被提交，这个命令一般不使用。

`git commit -v`

#在向本地仓库提交的时候，显示本次提交和上一次提交时候的差别，相当于执行了diff

`git commit --amend -m [message]`

#上一次修改代码后，提交不满意，不满意的这次提交的哈希值和信息都不想保存

#就可以使用--amend完成，相当于覆盖上一次的提交。

`git log （当前分支）`

#可以查看提交的日志信息，其实就是哈希值和自己的提交时候的信息

### 1.4.4Git撤销和版本回退

1. git的撤销

    `git checkout -- [file]或.` #恢复暂存区的文件到工作区

    `git checkout HEAD [file]或.` #将本地仓库恢复到暂存区和工作区

2. git版本回退

    `git reset --soft HEAD^`   #将上一次的commit恢复到本地仓库，暂存区和工作区不改变

    `git reset --mixed HEAD^` #将上一次的commit恢复到本地仓库和暂存区，工作区不改变

    `git reset --hard HEAD^`   #将上一次的commit恢复到本地仓库、暂存区和工作区

    > 注：
    >
    > `HEAD^` :上一次commit
    >
    > `HEAD^^`:上上一次commit
    >
    > `HEAD~3`:上上上一次commit
    >
    > 上述的`git reset --xxx commitID`后也可以写成提交的哈希值

    

### 1.4.5Git分支管理

`git branch`

#列出所有本地分支，仓库默认有一个master分支

`git branch -r`

#列出远程仓库的分支

`git branch -a`

#列出本地及完成的所有分支

`git branch 新分支名`

#新建一个分支，但并不会去切换分支

`git checkout -b  新分支`

#新建一个分支，并切换到新分支

`git checkout 分支名`

#分支切换

`git checkout -`

#切换到上一个分支

`git merge 分支名`

#合并指定分支到当前分支

`git branch -d 分支名`

#删除分支



例如：在master分支的基础上创建ori和dzs分支，在ori和dzs分支中分别创建git2.c的文件，

将ori和master分支合并无误，但是将dzs向master合并，就会出现冲突，可以使用git status

查看，然后在git2.c中也会记录冲突的位置如下：

```c
#include <stdio.h>

<<<<<<< HEAD
int sub(int a,int b)
{
    return (a-b);
=======
int add(int a,int b)
{
    return (a+b);
>>>>>>> dzs 
}

int main(int argc, const char *argv[])                                          
{
<<<<<<< HEAD
    printf("sub = %d\n",sub(100,200));
=======
    printf("sum = %d\n",add(100,200));  
>>>>>>> dzs 
    return 0;
}
```

修改完上述代码之后

`git add .`

`git commit -m "message"`

`git log --graph --pretty=oneline --abbrev-commit`

#查看分支合并图，如下：

`–graph` 图形

`–pretty=oneline` 减少数据
`–abbrev-commit` 头部数据减少

<img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20200726135748303.png" alt="image" style="zoom:67%;" /> 

### 1.4.6Git远程管理

#### 1.4.6.1Github和Gitee的简介

Git的远程操作我们可以借助Github Gitee等远程仓库，github和Gitee的对比如下

1. ==地理位置==：Gitee是国内的代码托管平台，GitHub是国外的代码托管平台。
2. ==公开项目数量==：GitHub的公开项目数量更多，因此它更适合用于开源项目。
3. ==限制==：Gitee在国内没有任何限制，而GitHub受到国外网络的限制。
4. ==用户数量==：GitHub的用户数量更多，因此它在开发者社区中更受欢迎。
5. ==功能==：Gitee和GitHub的功能大致相同，但GitHub更加强大。
6. ==社区==：GitHub的开发者社区更加活跃，因此如果需要寻求帮助或提供帮助，可以考虑使用GitHub

#### 1.4.6.2Gitee的远程仓库创建

1. 先去Gitee官网注册账号

    https://gitee.com/

2. 创建仓库

    <img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypicturemypictureimage-20230809130430825.png" alt="image-20230809130430825" style="zoom:50%;" /> 

    <img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20230810004958778.png" alt="image-20230810004958778" style="zoom:67%;" />  

3. 创建成功之后，显示如下

    <img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20230810005035489.png" alt="image-20230810005035489" style="zoom:67%;" />  

4. 将Ubuntu的ssh公钥插入账户

    <img src="https://hqyj-note-picture.oss-cn-beijing.aliyuncs.com/picture_bak/mypictureimage-20230810005413122.png" alt="image-20230810005413122" style="zoom:67%;" />  

#### 1.4.6.3Git远程仓库操作命令

`git clone [url]` #将远程仓库克隆到本地

`git pull [远程仓库] [远程分支]:[本地分支]`#将远程仓库的分支同步到本地并和本地分支==合并==

`git fetch [远程仓库] [分支]` #下载远程仓库所有变动，但不会合并(git pull = git fetch + git merge)

`git push [远程仓库] [本地分支]:[远程分支]`   eg:`git push origin master`

#将本地master分支上传到远程仓库的远程origin默认的master分支

`git push [远程仓库] [本地分支]:[远程分支] -f` #本地版本低于远程仓库版本，强制推送

`git push 仓库名 --delete 分支名`  #删除远程分支

`git remote -v` #显示远程仓库

`git remote show [远程仓库]` #显示远程仓库的信息	

`git remote add [远程仓库] [url]` #添加一个远程仓库地址，可以git push向上推送代码

`git remote rm [远程仓库]` #删除添加的远程仓库及地址

1. 

### 1.4.7Git给提交打标签

`git tag` #查看系统中的tag

`git tag 标签名` #给当前的commit id打一个tag

`git tag 标签名 <commitID>` #给指定的的commit id打一个tag

`git tag -a 标签名 -m "附加信息"`#打标签的同时写附加信息

`git show 标签名` #查看某个标签细节

`git tag -d 标签名` #删除一个标签

`git push 仓库名 --tags` #推送所有本地tag到远程

`git push 仓库名 标签名`#推送指定本地tag到远程

`git push 仓库名 :refs/tags/标签名`#删除远程指定tag

### 1.4.8Git文件比较

`git diff` #比较工作区和暂存区的差别(不能是新文件，同一个文件的比较)

`git diff --cached .`#显示暂存区和上一次commit的差异

`git diff HEAD`#显示工作区和上一次commit的差异

`git diff 分支1 分支2` #比较两个分支的差异

`git diff 标签/ID 标签/ID`#比较两个版本的差异

## 1.5git服务器搭建

GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。

搭建Git服务器需要准备一台运行Linux的机器，通过几条简单的`apt`命令就可以完成安装。

假设你已经有`sudo`权限的用户账号，下面，正式开始安装。

1. 安装`git`

    ```bash
    $ sudo apt-get install git
    ```

2. 创建一个`git`用户，用来运行`git`服务

    ```bash
    $ sudo adduser git
    ```

3. 创建证书登录

    收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

4. 初始化Git仓库

    先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

    ```bash
    $ sudo git init --bare sample.git
    ```

    Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：

    ```bash
    $ sudo chown -R git:git sample.git
    ```

5. 禁用shell登录

    出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

    ```bash
    git:x:1001:1001:,,,:/home/git:/bin/bash
    ```

    改为：

    ```bash
    git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
    ```

    这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。

6. 克隆远程仓库

    现在，可以通过`git clone`命令克隆远程仓库了，在各自的电脑上运行：

    ```bash
    $ git clone git@server:/srv/sample.git
    Cloning into 'sample'...
    warning: You appear to have cloned an empty repository.
    ```

    剩下的推送就简单了。

    

