![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_15-58-33.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_15-57-48.png)

## 命令

<b>文件</b>

.git文件，本地git仓库的配置文件
.gitignore文件，配置不纳入git管理的文件
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_17-19-13.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_17-23-36.png)

<b> 工作区 -----》本地git仓库 </b>

git init   //将本地文件夹初始化为git仓库

git status  //查看所有文件状态

git add .  //跟踪所有文件，将文件添加到暂存区

git commit -m '备注信息'   //将暂存区文件提交到git仓库

git checkout a.txt    //撤销本地文件的修改，还原成旧版本文件，慎用

git reset HEAD a.txt   //从暂存区移除文件

git rm -f a.txt   //删除文件  git仓库和工作区

git rm --cache a.txt    //删除文件  git仓库

git log    //查看截至到该版本的日志

git reset HEAD <CommitID>   //回退到指定旧版本

git  reflog  //回退旧版后，使用该命令查看所有日志



## 远程仓库

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_19-18-48.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_19-36-24.png)

ssh -t git@github.com    //验证本机github是否配置成功

<b>本地git仓库 -----》 云端git仓库</b>

git remote add origin https://github.com/teach.git     //将本地仓库与远程仓库进行关联，并将远程仓库命别名为origin

git clone https:github.com/my/git    //通过从云端克隆创建本地仓库

git push -u orgin  master   //本地分支推到远程仓库

<b>远程仓库-----》 本地仓库</b>>

git checkout remoteBranch  //跟踪分支，从远程仓库下载分支到本地

git pull   //从远程仓库拉取当前分支最新代码，保持代码一致


## 功能分支

master作用是保存和记录整个项目已完成的代码

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_20-55-05.png)

git branch    //查看本地所有分支

git remote show origin   //查看远程仓库，所有分支信息

git branch newBranch    //基于当前分支，创建一个分支，此时新分支和master中的代码完全一样，不自动切换分支

git checkout newBranch   //切换到新分支newBranch   

git checkout -b newBranch     //创建并切换到新分支，类似于 git commit -a -m '备注'

git merge newBranch     //先切换到master分支，将新分支newBranch   合并到master分支

git branch -d newBranch   //删除本地分支newBranch   

git push origin -delete remoteBranch    //删除远程仓库分支remoteBranch    

git push -u  远程仓库的别名  本地分支名称：远程分支名称     //本地分支推到远程仓库     -u 表示将本地分支与远程分支相关联，第一次推的时候才用加 -u，之后不用
git push -u origin   loacalBranch  :    remoteBranch

git push -u orgin  master   //分支相同时，简化写法

git checkout remoteBranch  //跟踪分支，从远程仓库下载分支到本地
git checkout -b newName origin/remoteBranch    //下载分支，并重命名为newName




## 解决冲突

autoMarge  failure  自动合并错误

需要手动打开冲突文件，解决冲突

重新执行 git add .   那一套重复

## 一般流程

git init 

git remote -add origin https:github.com/a.git

git add .

git commit -m '备注'

git pull

git push  origin master    //第一次推的时候才用加 -u，之后不用