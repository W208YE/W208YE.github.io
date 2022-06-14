# readme.md

本笔记为`Acwing付费课程Linux基础课中git部分的学习笔记`。

> 注意：学习git一定要系统的学一遍.

课程采用画图的方式来讲，因为git本身是一个树状结构，但很多博客都是按照命令来讲的（很乱），所以用树状结构讲解会更好理解。

此部分知识为学习axios预备知识,预备知识链:ajax --> promise --> axios --> react/vue

> 2022/05/04 11:25 二刷完毕

**git简介:一个便于版本管理,支持历史回滚,多人合作开发,代码比较,代码同步,可持久化的树结构的代码管理工具.**

# Git 基本概念与命令

## 1.1. git基本概念

- 工作区：**仓库的目录**。工作区是独立于各个分支的。
- 暂存区：数据暂时存放的区域，类似于工作区写入版本库前的缓存区。暂存区是独立于各个分支的。
- 版本库：存放所有已经提交到本地仓库的代码版本
- 版本结构：树结构，树中每个节点代表一个代码版本。

## 1.2 git常用命令

1. `git config --global user.name xxx`:设置全局用户名,信息记录在`~/.gitconfig`文件中
2. `git config --global user.email xxx@xxx.com`:设置全局邮箱地址,信息记录在`~/.gitconfig`文件中
3. `git init`:将当前目录配置成git仓库,信息记录在隐藏的.git文件夹中
4. `git add XX`:将XX文件添加到暂存区.
   - `git add .`:将所有待加入暂存区的文件加入暂存区
5. `git commit -m "备注信息"`:将暂存区的内容提交到当前分支
6. `git status`:查看仓库状态
7. `git rm --cached XX`:将文件从仓库索引目录中删除 **注意和git restore --stage XX的区别**
8. `git restore --staged XX`:将文件从暂存区中拿出来
9. `git checkout - XX` 或 `git restore XX`:将XX文件尚未加入暂存区的修改全部撤销,并恢复到HEAD所指的版本.      `git restore XX`**将工作区的自行修改恢复到暂存区存的内容，如果暂存区没有存任何内容，就恢复到HEAD所指向的版本**
10. `git diff`:
    1. `git diff XX`当工作区有改动，暂存区为空，diff对比是**工作区**和**最后一次commit提交的仓库**的共同文件。当工作区有改动，暂存区不为空，diff对比是**工作区**与**暂存区**的共同文件。
    2. `git diff --cached`或`git diff --staged`:显示**暂存区（已add但未commit文件）**和**最后一个commit（HEAD）**之间的所有不相同文件的增删改（两个命令作用相同）。
    3. `git diff HEAD`:显示**工作目录（已track但未add文件）和暂存区（已add但未commit文件）**与**最后一次commit**之间的所有不同文件的增删改。
    4. `git diff HEAD~X 或 git diff HEAD^^...`:可以查看最近一次提交的版本与过去时间线前X个版本之间的 **工作目录（已track但未add文件）和暂存区（已add但未commit文件）**与**最后一次commit**之间的所有不同文件的增删改。
11. `git log`:查看当前分支的所有版本
12. `git reflog`:查看HEAD指针的移动历史(包括被回滚的版本)
13. `git reset --hard HEAD^` 或 `git reset --hard HEAD~`:将代码库回滚到上一个版本
    - `git reset --hard HEAD^^`:往上回滚两次,以此类推
    - `git reset --hard HEAD~100`:往上回滚100个版本
    - `git reset --hard 版本号`:回滚到某一特定版本
14. `git remote add origin git@git.acwing.comxxx/XXX.git`:将本地仓库关联到远程仓库
15. `git push -u(第一次需要-u以后不需要)`:将当前分支推送到远程仓库
    - `git push origin branch_name`:将本地的某个分支推送到远程仓库
16. `git clone git@git.acwing.com:xxx/XXX.git`:将远程仓库XXX下载到当前目录下
17. `git branch`:查看所有分支和当前所处分支
18. `git checkout branch_name`:切换到`branch_name`:这个分支
19. `git checkout -b branch_name`:创建并切换到`branch_name`这个分支
20. `git merge branch_name`:将分支`branch_name`合并到当前分支上
21. `git branch -d branch_name`:删除本地仓库的`branch_name`分支，如果没有跟主分支合并就想删除的话，运行`git branch -D branch_name`命令即可。
22. `git branch branch_name`:创建新分支
23. `git push --set-upstream origin branch_name`:设置本地的`branch_name`分支对应远程仓库的`branch_name`分支（如果云端没有这个分支的话，就在云端创建一个分支并push上去）
24. `git push -d origin branch_name`:删除远程仓库的`branch_name`分支
25. `git pull`:将远程仓库的当前分支与本地仓库的当前分支合并（可简单理解为从云端拿下来，再merge一遍）
    - `git pull origin branch_name`:将远程仓库的`branch_name`分支与本地仓库的当前分支合并
26. `git branch --set-upstream-to=origin/branch_name1 branch_name2`:将远程的`branch_name1`分支与本地的`branch_name2`分支对应
27. `git checkout -t origin/branch_name`:将远程的`branch_name`分支拉取到本地
28. `git stash`:将工作区和暂存区中尚未提交的修改存入栈中
29. `git stash apply`:将栈顶存储的修改恢复到当前分支,但不删除栈顶元素
30. `git stash drop`:删除栈顶存储的修改
31. `git stash pop`:将栈顶存储的修改恢复到当前分支,同时删除栈顶元素
32. `git stash list`:查看栈中所有元素

## 3. git pull 和 git fetch 的区别

* `git fetch`只是将远程仓库的变化下载下来，并没有和本地分支合并。
* `git pull`会将远程仓库的变化下载下俩，并和当前分支合并

## 4. git rebase 和 git merge 的区别

`git merge`和`git rebase`都是用于分支合并，关键 **在commit 记录的处理上不同**

* `git merge`会新建一个新的 commit 对象，然后两个分支以前的 commit 记录都指向这个新 commit 记录。这种方法会保留之前每个分支的 commit 历史。
* `git rebase`会先找到两个分支的第一个共同的 commit 祖先记录，然后提取当前分支这之后的所有 commit 记录，然后将这个 commit 记录添加到目标分支的最新提交后面。经过这个合并后，两个分支合并后的 commit 记录就变成线性的记录了。

**总结：**

如果你想要一个干净的，没有merge commit的线性历史树，那么你应该选择git rebase
如果你想保留完整的历史记录，并且想要避免重写commit history的风险，你应该选择使用git merge

## 5. git 和 svn 的区别

* 最大的区别在于 git 是分布式的，而 svn 是集中式的。因此我们不能在离线的情况下使用svn。如果服务器出现问题，就没有办法使用 svn 来提交代码。
* svn 中的分支是整个版本库复制的一份完整目录，而 git 的分支是指针指向某次提交，因此 git 的分支创建开销更小。git 分支上的变化不会影响到其他人，而svn 的分支变化会影响到所有人。
* **git 把内容按元数据的方式存储，而svn是按文件：**因为 git 目录是处于个人机器上的一个克隆版的版本库，它拥有中心版本库上的所有信息，如标签，分支，版本记录等。
* **git 分支和 svn 的分支不同：**svn 会发生分支遗漏的情况，而 git 可以在同一个工作目录下快速的在几个分支间切换，很容易发现未被合并的分支，简单而快捷的合并这些文件。
* **git 没有全局的版本号，而 svn 有**
* **git 的内容完整性要优于 svn：**git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时，降低对版本库的破坏。