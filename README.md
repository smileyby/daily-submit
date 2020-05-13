# Git 提交项目代码记录
## 一、日常提交和冲突解决
### 1.1日常提交

**<font style="color:#ff6700;">温馨提示</font>：每次新增一个功能或者修改某一部分代码的时候都需要<font style="color:red;">新建一个分支</font>，以确保合并新功能的时候不会合并其他人或者自己之前改动的东西。**

```

// 基于正式分支建立自己的功能提交分支，表示基于正式库v1.0

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

git branch -D [分支名] 强制删除分支

git push origin --delete [分支名] 删除远程分支

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
4. git reset --hard  撤销当前分支的所有修改，回到上一次提交的内容

#### 1.1.2删除本地和远程分支

```
// 删除本地分支 （不能再当前分支删除该分支，任意切换到其他分支删除即可）
git branch -D 分支名

// 删除远程分支
git push origin :分支名

```

#### 1.1.3 分支加标签

### 1.2冲突解决
找到对应冲突的文件，里面会有断层提示。修改成你想要的代码后，进行add操作，然后再push到远程

### 1.3非冲突问题解决

错误信息如下：（根据英文提示，应该是有人提交代码到你要提交代码的分支了，此时需要你先运行`git pull`，再进行`git push`操作）
```
$ git push
To https://git.coding.net/feiwu_yl/mibo.git
 ! [rejected]        dev_v1.0 -> dev_v1.0 (fetch first)
error: failed to push some refs to 'xxxxxxxxxxxxxxxxxxxx.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

在没有`add` 修改文件的时候，新建分支去修改其他功能，这时会把所有修改的文件都带过去。需要将在前一个分支修改过的文件在其分支下进行**add**操作，类似于添加分支控制到修改文件。这样再去新建分支

merge时显示信息：

```
Please enter a message to explain why this merge is necessary .....
```

解决办法：
```
//1.按键盘字母 i 进入insert模式

//2.修改最上面那行黄色合并信息,可以不修改

//3.按键盘左上角"Esc"

//4.输入":wq",注意是冒号+wq,按回车键即可
```

### 1.4 如何忽略已经提交过的文件

> 最近在项目中遇到，需要需略已经提交过的文件
> 发现如果文件已经进行了`git add/git commit/git push`其中的任何一个操作，再去修改 `.gitignore`文件是无效的，需要清除cached然后重新添加就可以了

```
// 如果觉得此方法比较暴力，可以对要想忽略的某个文件进行单独删除
// git rm --cached <fileName>
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push
```

## 二、如何在测试环境更新代码

### 2.1登陆测试环境同步服务器代码

```
ssh 用户名@主机地址
输入：登录密码
```

* 进入到测试环境目录，`ls`查看目录结构
* cd 需要更新的目录
* 运行`git pull`
* 测试环境的代码就都已经同步完成了，记得`exit;`退出服务器

### 2.2登陆正式环境同步服务器代码

同上操作

## 处理冲突的方法（这是个不成熟的建议，不建议使用）

```
git stash 将工作区的东西放在暂存区
git pull 拉取线上代码
git stash pop 退出暂存区返回工作空间
```
此时再把自己的代码提交上去就行啦。

## 回滚单个文件到指定版本
* 这里的文件路径可以是相对也可以是绝对，相对路径开始不能友斜杠
```
git log '文件路径/文件名'  // 拿到commit id
git checkout commit_id '文件路径/文件名'
git commit 添加回滚信息
git push
```

## 查看单个文件的修改记录
```
git log filename // 可以看到对应文件的提交记录（commit_id）
git log -p filename // 可以查看每次提交具体修改了哪里（具体到哪一行有变动）
git log --pretty=oneline [文件名] 显示完成的commit_id和提交信息并在一行显示
git show [commit_id] [filename] // 可以查看某个commit_id下修改的内容
// 退出英文状态下按Q
```

## git fetch --prune
当本地分支与远程分支不同步的时候，运行上面这条命令即可

## 常用命令

```

git init : 创建仓库
git add : 把修改提交到暂存区
git commit -m "" :  把暂存区的所有修改提交到分支
git checkout -- file : 丢弃工作区的修改
git reset HEAD file : 把暂存区的修改撤销掉（unstage），重新放回工作区
git log (--pretty=oneline): 查看提交的历史记录
git reset --hard HEAD^ : 退回到上一个版本
git reset --hard 版本号 ： 退回到某个版本
git reflog : 记录每一次命令
git rm : 从版本库中删除文件

ssh-keygen -t rsa -C "新注释"
git push origin master : 把本地库推送到远程库
git clone : 克隆一个本地库
git checkout -b 分支名 ： 创建并切换分支
git branch 分支名 ： 创建分支
git checkout 分支名 ：　切换分支
git branch ： 查看当前分支
git merge 分支名 ： 合并分支
git branch -d 分支名 ：删除分支
git log --graph 查看分支合并图
git merge --no-ff -m "备注" 分支名 ： 禁用fast forward 方式合并分支，并创建一个新的commit
git stash :　把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash list : 查看工作现场
git stash apply : 恢复后，stash内容并不删除，你需要用git stash drop来删除
git stash pop : 恢复的同时把stash内容也删了
git branch -D 分支名 ： 强制删除分支
git remote -v : 查看远程仓库的信息
git push origin 分支名 ： 推送分支到远程分支
git checkout -b dev origin/dev : 创建远程origin的dev分支到本地
git branch --set-upstream dev origin/dev : 指定本地dev分支与远程origin/dev分支的链接 
git pull : 拉取最新分支
git tag 版本号 ： 添加标签
git tag : 查看所有标签
git tag -d 版本号 ： 删除标签
git push origin 版本号 : 推送某个标签到远程
git push origin --tags : 一次性推送全部尚未推送到远程的本地标签
删除远程标签方法：
先删除本地标签：git tag -d 标签名
再删除远程标签：git push origin :refs/tags/标签名

忽略某些文件时，需要编写.gitignore；
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

```

更多关于git的操作和原理，请移步[Pro Git（中文版）](http://git.oschina.net/progit/)
Git 实用指南：https://juejin.im/post/5c9c6e4ee51d454e3a3903a8
