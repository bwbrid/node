### git的初期配置
+ 设置用户名: `git config --global user.name "username"`
+ 设置用户邮箱: `git config --global user.email "eamil.example.com"`
+ 输出显示颜色: `git config --global color.ui true`

### 初始化一个git仓库
+ 在一个目录中执行`git init`命令，可以初始化该目录成为git的工作目录

### 添加文件到git仓库
+ 第一步：告诉git, 把文件添加到仓库
	* 一次添加一个文件: `git add filename`
	* 一次同时添加多个文件: `git add file1 file2`

+ 第二步：告诉git, 把文件提交到仓库
	* `git commit -m "comment"`

### 查看工作区的状态与修改的内容
+ 要随时掌握工作区的状态，使用`git status`命令
+ 如果`git status` 告诉你有文件被修改过得话，使用`git diff`来查看修改的内容

### git中的版本控制
+ `HEAD`指向的版本就是当前的版本，我们可以使用命令`git reset --hard commit_id`来进行版本之间的切换
+ 在版本切花之前，我们可以使用`git log`来查看历史，以便确定要退回哪个版本
+ 要回到未来的版本时，可以使用`git reflog`查看历史命令，以便确定要回到未来的哪个版本

### git中的暂存区的概念
+ 当我们把修改好的文件用`git add`命令添加到git仓库时，其实此时添加的文件会被加入到暂存区中
+ 然后，当我们执行`git commit`命令时，就可以一次性把暂存区的所有文件提交到分支
+ 每次修改后，如果不把修改的文件`add`到暂存区中，那就不会加到`commit`中

### git中的文件撤销相关操作
+ 想把工作区的文件恢复（丢弃工作区的修改）的话，用命令`git checkout -- file`
+ 想撤销已添加到缓存区中的文件是，使用命令`git reset HEAD file`, 此命令可以把用`add`添加到缓存区中的文件恢复到工作区中
+ 当提交了不合适的修改到版本库中的时候，想要撤销的话，可以使用版本回退功能回到上一个版本，命令`git reset --hard HEAD^`

### git中的文件删除
+ 可以使用`git rm file`命令来删除文件
+ `git checkout`命令可以进行"一键还原"，其实是用版本库中的版本来替换工作区的版本，无论工作区的是修改了还是删除了

### git中的远程仓库
+ 要关联一个远程仓库，是用命令`git remote add origin git@server-name:path/repo-name.git`
+ 关联远程仓库后，使用命令`git push -u origin master`第一次推送master分支的内容
+ 此后，每次本地提交后，只要有必要就可以使用`git push origin master`推送最新的修改
+ 要克隆一个远程仓库，首先必须知道远程仓库的地址，然后使用`git clone`命令克隆
+ git支持多种协议，包括`http`，但是通过`ssh`支持的原生`git协议`速度最快
+ 查看远程库信息，使用`git remote -v`
+ 本地的分支如果不推送到远程的话，对其他是不可见的
+ 从本地分支推送，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓去远程的然后在提提交
+ 在本地创建和远程对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称只好一致
+ 建立本地分支和远程分支的关联，使用`git branch --set-upstream origin/branch-name`
+ 从远程抓取分支，使用`git pull`，如果有冲突，需要先解决冲突在抓取

### git中分支的创建和使用
+ 查看分支: `git branch`
+ 创建分支: `git branch <name>`
+ 切换分支: `git checkout <name>`
+ 创建并切换分支: `git checkout -b <name>`
+ 合并某分支到当前分支: `git merge <name>`
+ 删除分支: `git branch -d <name>`
+ 当修改的内容无法自动合并时，只能手动修改内容以解决冲突。解决冲突后再提交，完成合并
+ 使用`git log --graph`可以查看分支合并图
+ 使用`git log --graph --pretty=oneline --abbrev-commit`使用精简模式查看并图
+ 在合并分支时，加上`--no-ff`参数就可以使用普通的合并模式，合并后有历史分支。可以看出曾经做过合并，而`fast forward`合并的话，是看不出曾经做过的合并
+ 开发一个新feature，最好新建一个分支
+ 如果要丢弃一个没有合并过的分支，可以通过`git bransh -D <name>`来强行删除

### 工作目录的存储和恢复
+ 当我们在`dev`分支中进行开发时，此时要修复BUG，要创建一个新的分支来`issue-01`来修复BUG.
+ 于是我们需要先把现在的工作目录保存起来，再从`master`中取出最新的内容进行BUG修复
+ 使用命令`git stash`来储存现在的工作目录状态
+ 等到BUG修复完成之后，我们可以使用命令`git stash pop`来恢复存储的状态
+ 查看当前的所有的存储内容: `git stash list` 
+ 指定要恢复的stash: `git stash apply stash@{0}`

### 基于git的多人协作开发
1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改
2. 如果推送失败，则是因为远程分支比本地的更新，需要先用`git pull`来试图合并
3. 如果合并有冲突，则解决冲突，并在本地提交
4. 没有冲突或者解决冲突后，再用`git push origin <baanch-name>`推送
5. 如果`git pull`提示 **no tracking information** ,则说明本地的分支和远程的分支的链接关系没有创建，要用命令`git branch --set-upstream <branch-name> origin/<branch-name>`

### git中的标签的使用
+ 命令`git tag <name>`用于创建一个标签，默认为HEAD, 也可以指定一个commit id
+ 创建标签时可以指定说明: `git tag -a <tagname> -m "comment ..."` 
+ 可以使用PGP签名标签: `git tag -s <tagname> -m "comment ..."`
+ 查看所有的标签: `git tag`
+ 删除一个本地的标签: `git tag -d <tagname>`
+ 推送一个本地的标签到远程: `git push origin <tagname>`
+ 推送全部未推送的本地标签到远程: `git push origin --tags`
+ 要删除一个远程的标签: 
	* 先删除本地的标签: `git tag -d <tagname>`
	* 在删除远程的标签: `git push origin :refs/tags/<tagname>`

### GitHub的"pull request"
+ 在GitHub上，可以任意的Fork开源仓库
+ 自己拥有Fork后的仓库的读写权限
+ 可以推送`pull resuest`给官方仓库来贡献代码

### 忽略特殊的文件
+ 要忽略某些文件时，需要编写`.gitignore`
+ `.gitignore`文件本身要放到版本库中，并且可以对`.gitignore`最版本管理
+ 所有配置文件可以直接在线浏览：[https://github.com/github/gitignore]()

