这里总结一些Git的常用命令和使用场景
主要参考了廖雪峰的git教程
https://www.liaoxuefeng.com/wiki/896043488029600

1.分布式版本控制系统
2.安装Git Windows上安装客户端
设置
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
3.仓库的创建
版本库/仓库 英文名字 repository 
类似于一个目录，概率目录下的文件可以被Git管理文件的更改受到跟踪，任何时刻都可以追踪还原。
首先创建一个版本库 ，创建一个空目录
$ mkdir learngit  //创建   默认创建到 /C/user/DELL/User 下面
$ cd learngit
$ pwd  //显示目录
/C/user/DELL/Users/michael/learngit


//我建议手动创建吧 
//cd 到你仓库的位置 
$ cd D:/learngit
$ pwd
/d/learngit
$ git init
Initialized empty Git repository in D:/learnGit/.git/

通过git init命令把这个目录变成Git可以管理的仓库：
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/

PS:
如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。
或者在windows 查看，显示隐藏的项目

就能看到.git文件夹了

4.  添加文件到仓库 
添加文件到Git仓库，分两步：
使用命令git add <file>，注意，可反复多次使用，添加多个文件；
使用命令git commit -m <message>，完成
5.查看修改的状态 
要随时掌握工作区的状态，使用git status命令。
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

6.版本机制--版本回退 
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
git reset --hard HEAD^   回退到上一个版本 
git reset --hard 版本号 前几位 
7. 工作区 版本库 
工作区，就是你本地的文件 
版本库里面有个暂存区（stage） ，存放你add的 文件（你可以add多个文件），然后 commit 才会将暂存区的文件一次性提交到master 



问题：是不是版本库的文件 log版本是针对版本库的所有文件来说的？

每次修改，如果不用git add到暂存区，那就不会加入到commit中。 就是说，你要是更改了两次，但是只提交了第一次，就仅仅会把第一册的内容提交到版本库


8.撤销修改 
（1）撤销工作区的修改 
这个修改指的是本地的修改（工作区），的并且还没添加到暂存区，更没有提交
git checkout -- readme.txt
撤销修改为add到暂存区的，或者版本库的最新版本
checkout 是校验的意思
（2）撤销 暂存区的修改 
就是你add到暂存区但是没有提交 
reset指令
git reset HEAD readme.txt


1.建立仓库 关联 推送
在github上新建一个仓库 
关联一个远程库，使用命令git remote add origin git@github.com:UserName/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

2.克隆一个仓库 
（1）首先云端存在一个仓库 
（2）然后你再本机选择一个目录，也就是当前命令操作的目录

（3）克隆 
git clone git@github.com:UserName/gitskills.git
克隆会在

3.分支管理 
分支，就是 你可以创建，然后在分值上修改，最后在合并。而且需要注意的是，你的操作都在当前分支上，也就是互不干扰么，而且当你切换分支的时候对于本地来说会自动更新。
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

4.合并分支 
git merge feature1
往主线合并 
5.冲突以及解决 
冲突时因为主线和支线的文件的同一段代码都被更改了，所以合并后会出问题。要手动修改
git status 显示冲突的位置，手动修改。add ，提交commit

6.分支管理策略 
没明白啊 ，后来我渐渐懂了。原来，第一次不懂，第二次看到后面的操作，也就懂了前面是干嘛的
  合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
 git checkout master  //选中合并的支线 
git merge --no-ff -m "merge with no-ff" dev   //普通模式合并 ，能看见分支的历史 也就是 dev分支还在
结果对比 

7.bug分支 （重要，我认为）

（1）选中当前的分支，比如master （或者就不用改分支也行，就当前的）
（2）缓存当前进度 
git stash
比如说，你当前的代码改了一半，然后突然让你提交一个bug。
这时候你就可以缓存工作现场  git stash    ，代码会恢复到 上一次提交的代码 
然后恢复是 git stash pop ，但是 恢复的同时stash缓存的内容也删除了   
（3）从  master 支 创建 ，一个关于bug的分支     命名为issue -101（问题）
     git checkout -b issue-101
（4）add commit 
（5）恢复现场。。。。git stash pop 
（6）切换回 主支 合并 ，删除bug分支 ，（当然你删除可以选择分支管理策略，也可以不使用 ）

bug分支，就是正常的分支加上 stash缓存 pop恢复 而已 

8.强制删除分支 
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

