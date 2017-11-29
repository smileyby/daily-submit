# Git 提交项目代码记录
## 一、日常提交和冲突解决
### 1.1日常提交

**<font style="color:#ff6700;">温馨提示</font>：每次新增一个功能或者修改某一部分代码的时候都需要<font style="color:red;">新建一个分支</font>，以确保合并新功能的时候不会合并其他人或者自己之前改动的东西。**

```
// 基于正式分支建立自己的功能提交分支，表示基于正是库v1.0
git checkout -b boyang_dev origin/v1.0

git add 需要提交的文件

git status 查看状态信息

git commit -m '提交信息'

git status 查看状态信息

// 提交本地boyang_dev分支作为远程的boyang_dev分支
git push origin boyang_dev:boyang_dev

git checkout dev1.0  切换分支到测试1.0

git pull origin dev1.0 拉取远程分支最新代码

git merge boyang_dev  合并本地测试分支到本地测试环境

git push origin dev1.0 讲合并后的最新代码推到远程测试1.0环境中

// 推到远程测试环境后，需要登陆测试服务器进行git pull操作

// 经过测试没有问题后，合并到正式分支v1.0

git checkout v1.0  切换到v1.0正式环境本地分支

git pull origin v1.0 拉取远程正式库v1.0最新代码

git merge boyang_dev 将本地测试分支合并到本地正式分支上

git status 查看状态信息

git push origin v1.0  讲本地已经合并的正式分支代码推到远程正式分支v1.0中

// 更新到远程正式v1.0后需要登陆正式服务器进行git pull操作

// 到这里所有该提交和更新的地方都已经更新完成，不要忘记删除自己的这个分支。以免下一次自己又在这个分支提交代码造成混乱提交

git branch -D boyang_dev 强制删除分支

git push origin :boyang_dev 删除远程分支

// 到这里就完成了整个修改-提交-更新的全过程

```
> 说明：
> 1.每次添加新功能都以新建分支的形式提交
> 
> 2.每次`git merge`后就会弹出一个输入合并信息的vi界面，这个按`i`切换到insert模式，输入完成后按`Esc`再输入`:wq`回车就可以了
> 
> 3.基于另一个分支创建自己的功能分支[https://stackoverflow.com/questions/15026864/creating-git-branch-based-another-branch](https://stackoverflow.com/questions/15026864/creating-git-branch-based-another-branch)
>
> 4.`git push origin test:master` // 提交本地test分支作为远程的master分支  `git push origin test:test` // 提交本地test分支作为远程的test分支

#### 1.1.1撤销commit
1. 找到上次git commit的 id
     git log 
     找到你想撤销的commit_id
2.  git reset --hard commit_id
      完成撤销,同时将代码恢复到前一commit_id 对应的版本。
3. git reset commit_id 
     完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改。

#### 1.1.2删除本地和远程分支

```
// 删除本地分支 （不能再当前分支删除该分支，任意切换到其他分支删除即可）
git branch -D 分支名

// 删除远程分支
git push origin :分支名

```

### 1.2冲突解决
找到对应冲突的文件，里面会有断层提示。修改成你想要的代码后，进行add操作，然后再push到远程

### 1.3非冲突问题解决

错误信息如下：（根据英文提示，应该是有人提交代码到你要提交代码的分支了，此时需要你先运行`git pull`，再进行`git push`操作）
```
$ git push
To https://git.coding.net/feiwu_yl/mibo.git
 ! [rejected]        dev_v1.0 -> dev_v1.0 (fetch first)
error: failed to push some refs to 'https://git.coding.net/feiwu_yl/mibo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```


## 二、如何在测试环境更新代码

### 2.1登陆测试环境同步服务器代码

* 登陆测试服务器
* 进入到测试环境目录，`ls`查看目录结构
* 进入到`cd /opt/webroot/xxx/v1.0`,再次运行`git pull`
* 代码同步完成了，记得`exit;`退出服务器
