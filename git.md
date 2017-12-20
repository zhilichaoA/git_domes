# git命令及解释

### 初始化
- git config 配置命令
- git init 命令把这个目录变成Git可以管理的仓库
- git status 命令查看仓库当前的状态,查看那些文件修改，但还没有准备提交的修改。
- git diff 文件名字   查看对文件进行什么修改

### 提交文件
- git add ./文件名字/-A 添加文件 然后用git status查看 报红就是有修改
- git commit -m "提交信息备注" 提交修改 然后用git status查看 报红就是有修改

### 回退版本号
- git log 查看commit历史记录 显示从最近到最远的提交日志,如果嫌输出信息太多 加上 --pretty=oneline参数
	- 需要友情提示的是，你看到的一大串类似3628164...882e1e0的是commit id（版本号）

- git reset --hard HEAD^ 回退到上一个版本
	- 用HEAD表示当前版本,上一个版本就是HEAD^，上上一个版本就是HEAD^^
	- 当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
- git reset --hard 版本号的前几位
	- 版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。


### 回复版本号
- git reflog  用来记录你的每一次命令
	- 在Git中，总是有后悔药可以吃的。当你用$ git reset --hard HEAD^回退到以前的版本时，再想恢复到以前的，就必须找到它的commit id。Git提供了一个命令git reflog用来记录你的每一次命令
- git reset --hard [记录中某一个commit id] 回复到某一个版本


###工作区和暂缓区
- *前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：*
- 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
- 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
```
	因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
	你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
```

### 管理修改
- Git跟踪并管理的是修改，而非文件。
- git diff HEAD -- 文件名     查看工作区和版本库里面最新版本的区别


### 撤销修改
- git checkout -- 文件名   意思就是，把文件在工作区的修改全部撤销,总之，就是让这个文件回到最近一次
	- git checkout -- file命令中的'--'很重要，没有'--'，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。


### 删除文件
- 确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
	- git rm 文件名
	- git commit 文件名
- git checkout -- 文件名
	- 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本


### 添加远程仓库
- *把一个已有的本地仓库与之关联*
- git remote add origin https://github.com/zhilichaoA/git_domes.git  关联远程仓储
- git push -u origin master 第一次送到远程仓库master分支上
- git push origin master 第二次推送开始使用

### 克隆仓库到本地
- git clone 仓库地址

---------------------------------------------------------------------
### 创建与合并分支
- git checkout -b 分支名字    创建分支并且切换到创建的分支上

##### git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
- git branch [dev]   创建分支
- git checkout [dev]  切换到[dev]分支上

##### 查看当前分支
- git branch    查看分支,当前分支前面会标一个*号

##### 当前分支上提交文件
- git add 文件名/-A/.
- git commit -m "版本信息"
- git push origin [dev] 推送到远程仓库[dev]分支上(如果不用这个命令,就是本地分支)

##### 回到[某个]分支
- git checkout [master] 切换到[master]分支上

##### 合并[某一个分支]到master主分支上
- git merge [某一个分支]   合并到主分支(必须在主分支下合并);

##### 合并完成后删除某一个分支
- git branch -d [某一个分支]  删除某一个分支 (这能删除本地分支)
- git branch    查看分支,当前分支前面会标一个*号

### 多人协同开发
- git pull 出错后,原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接
- git branch --set-upstream dev origin/[远程的分支名字]


### 解决冲突
- 冲突是在分支上工作完成了,后没有合并,切换到主分支上继续工作,就就会有冲突了,解决办法删除<<<<<<< 具体自己看 >>>>>>>

### 分支合拼历史
- git merge --no-ff -m"第一次添加历史信息" [分支名字]     产生合并历史
- git log --graph --pretty=oneline --abbrev-commit 查看合并历史信息


### bug分支
- git stash隐藏工作(把当前工作现场“储藏”起来，等以后恢复现场后继续工作)
- git stash 查看工作区的状态
- git stash list  查看隐藏的工作区
- git stash pop 恢复隐藏的工作区(恢复的同时把stash内容也删了)

### 强行删除分支
- git branch -D [分支名字] 详情阅读强行删除分支.md