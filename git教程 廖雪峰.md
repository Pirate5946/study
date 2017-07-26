### [git教程 廖雪峰](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374027586935cf69c53637d8458c9aec27dd546a6cd6000)
> 分布式版本控制系统
- 初始化git仓库  
在一个空目录下执行 git init， ls -ah 可以查看新生成.git文件
- 随时掌握工作区的状态  git status
- git diff 查看被修改的文件内容
- git log 查看git日志   
git log --pretty=oneline 格式化查看日志
> 本地版本回退  

- git reset --hard HEAD^  //回退到上一版本
git reset --hart HEAD~100 //回退到上100个版本     
git reset --hard 提交的ID  //回退到指定提交ID的版本  
- git log 查看当前版本的历史版本
- git reflog 查看当前版本的未来版本   
==如果错误回退==，用这条命令+git reset --hard 版本号回到指定版本
> 工作区（working directory ）与暂存区（stage）、分支

- git add 文件名/目录名 //将工作区修改提交到暂存区
- git commit -m ''  //将暂存区内容提交到当前分支    
==每次修改完记得 git add，所有的git add完成后一次性git commit==
- 撤销更改      
    - 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令==git checkout -- file==。

    - 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令==git reset HEAD file==，就回到了场景1，第二步按场景1操作。

    - 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库
- git checkout -- file      
用版本库里的文件版本更换工作区的文件版本
- 删除文件      
git rm 文件；git commit ；
> 添加远程仓库  

- 关联一个远程库，使用命令  
git remote add origin git@server-name:path/repo-name.git；

- git push -u origin master第一次推送master分支的所有内容；

- 每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

> 克隆远程仓库  
- 要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

- Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快
```
ssh 协议地址
git clone git@github.com:Pirate5946/study.git
https协议地址
git clone https://github.com/Pirate5946/study.git
```


> 分支管理  

HEAD指向 不同的分支 ；  
不同的分支指向提交后的版本；
- 创建并切换分支
```
git checkout -b 分支名
```
相当于以下两条命令
```
git branch 分支名  // 创建分支
git checkout 分支名 // 切换分支
```
- 查看当前分支
```
git branch
```

- 合并指定分支 到当前分支
```
git merge 指定分支
```
- 删除分支
```
git branch -d 分支名
```
> bug分支
- 修复bug时，首先在出现bug的分支上创建新的bug分支进行修复，然后合并，最后删除；

- 当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

> 解决冲突

```
1、git status
2、vi 查看 冲突文件与分支
3、手动解决冲突，重新提交
4、删除多余分支
```

- 强行删除没有合并的分支

```
git branch -D 分支名
```
> 多人协作  

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，==远程仓库的默认名称是origin==。

- 查看远程仓库的信息
```
git remote
git remote -v //查看详细信息
```
- 在本地创建和远程分支对应的分支

```
git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
```

- 建立本地分支和远程分支的关联

```
git branch --set-upstream branch-name origin/branch-name；
```

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
> 多人协作的工作模式通常是这样：

首先，可以试图用git push origin branch-name推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name
> 推送分支

在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定
- master分支是主分支，因此要时刻与远程同步；

- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

```
git push origin 分支名
```
> 标签管理  

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。 

- 新建一个标签，默认为HEAD，也可以指定一个commit id；
```
git tag <name>
git tag <tagname> commit_id
```
- 指定标签信息
```
git tag -a <tagname> -m "blablabla..."
```
- 用PGP签名标签
```
git tag -s <tagname> -m "blablabla..."
```
- 查看标签信息
```
git show tagname
```
- 推送一个本地标签到远程仓库
```
git push origin <tagname>
```
- 推送全部未推送过的本地标签
```
git push origin --tags
```
- 删除一个本地标签
```
git tag -d <tagname>
```
- 删除一个远程标签
```
git push origin :refs/tags/<tagname>
```

命令git tag可以查看所有标签
> 进阶应用

- 以单行显示提交内容时间线
```
git log --graph --pretty=oneline --abbrev-commit
```
- 禁用 Fast forward模式进行分支合并      
在Fast forward模式下删除分支后，会丢失分支信息
```
git merge --no-ff -m "merge with no-ff" dev
```
> 配置别名

```
git config --global alias.别名 原命令
```
- 每个仓库的Git配置文件都放在.git/config文件中：

#### 搭建Git服务器
搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian
>第一步，安装git：
```
sudo apt-get install git
```
>第二步，创建一个git用户，用来运行git服务：
```
 sudo adduser git
```
>第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
>第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
```
$ sudo git init --bare sample.git
```
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
```
$ sudo chown -R git:git sample.git
```
> 第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

git:x:1001:1001:,,,:/home/git:/bin/bash        
改为：      
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。
>第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

```
$ git clone git@server:/srv/sample.git

Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```

剩下的推送就简单了。












