# Git

## 1.基本命令

- `git config --global user.name "username"`：配置用户名

- `git config --global user.password"xx@gmail.xx"`：配置邮箱

- `git init`：初始化一个git项目

- `git add 项目文件`：将指定文件添加到仓库 可以是`.`表示提交文件夹内的所有文件

- `git commit -m "提示"`：将添加到仓库的文件提交到仓库

- `git cherry-pick 提交记录id`：如果只想要合并某一次的分支提交，可以使用这个方法在对应分支使用。

- git commit --amend -m "New commit message"：修改上一次提交的信息。

- `git status`：显示当前仓库的状态,可以告诉我们哪些文件被修改过没有提交已经将要提交什么样的修改,如果都没有就会告诉我们git仓库很干净

- `git diff 指定文件`：查看指定文件与原来的状态有什么具体的不同,也可以使用`git diff HEAD -- 指定文件`来查看和最新版本的区别

- `git log`：显示最近到最远的提交日志和分支合并的情况,如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数`git log --pretty=oneline`,而使用`git log --pretty=oneline --abbrev-commit`可以显示历史的提交记录

  **注：**在git中用`HEAD`表示当前的版本,`HEAD^`表示上一个版本,`HEAD^^`表示上两个版本,依次类推

- `git reset`：用于回到git仓库以前的版本如：`git reset --hard HEAD^`回到上个版本

  **注意：**这时候使用git log就看不到最后一次提交的版本了,要重新回到那个版本需要不关闭这次窗口,然后找到前面提交的`commit id`,因为每次commit都会对应提交一个`commit id`,而且版本后不需要写全,写出前几位git会自动去寻找到对应的版本号`git reset --hard 5627c`,也就是所HEAD指向谁谁就是现在的版本号

  - `git reflog`：该命令能够记录每一次跟换版本号的命令,通过该命令能很快查找到想要直接退回的版本号。当我们需要回退到当前版本（已经回退的版本）的前一个版本时，只能使用 reflog

- `git checkout -- 指定文件(readme.txt)`：将指定文件在工作区的修改全部撤销(注意是工作区.不是暂存区),有两种情况

  + 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态
  + 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态

  **注意：**在这里一定要在中间加上`--`,不然就是切换到分支了,所以`git checkout`其实有两种用法

- `git reset HEAD 指定文件`：`git reset`的第二个用法,将保存在暂存区的文件撤销掉,重新放回工作区,然后就可以又使用`git checkout -- 指定文件`来丢弃工作区的更改

- `git rm 指定文件`：先在工作文件夹中使用`rm 指定文件`将指定文件删除,然后在使用`git rm 指定文件`在用`git commit`提交就可以删除git仓库中的指定文件

  **注：**

  - 先手动删除文件，然后使用`git rm <file>`和`git add<file>`效果是一样的
  - 如不小心删错了工作区的目录,可以使用` git checkout -- 指定文件`修改撤销,但是要注意这个文件是要在git仓库中存在才可以,没有添加到git仓库的文件是无法恢复的



## 2.工作区与暂存区

工作区是在git中进行操作的区域,暂存区是用git add提交到的地方,然后使用git commit将暂存区里的文件提交到分支,提交后工作区就是干净的了



## 3.添加到远程库

在github或其他git仓库创建一个git的远程仓库,使用`git remote add origin github上对应的地址 (git@github.com:michaelliao/learngit.git)`

**注：**添加后，远程库的名字就是`origin`，这是git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库

然后使用`git push -u origin master`把本地库的内容推送到远程

**注：**用`git push`命令，实际上是把当前分支`master`推送到远程,由于远程库是空的，第一次推送`master`分支时，加上了`-u`参数，git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令

以后每次本地提交了修改,就可以通过`git push origin master`把本地的`master`分支推送到github,之所以是master是因为创建的时候就叫master



## 4.从远程库克隆

使用`git clone github上对应的地址`就可以在文件夹中克隆一个远程仓库到本地



## 5.创建分支与合并

**git鼓励我们使用分支,因为更加具有安全性**

- 查看分支：`git branch`

- 创建分支：`git branch <name>`

- 切换分支：`git checkout <name>`

- 创建+切换分支：`git checkout -b <name>`

- 合并某分支到当前分支：`git merge <name>`

- 删除分支：`git branch -d <name>`

  **注意：**如果没有合并过就要删除,必须使用`git branch -D <name>`才能够强制删除

在本地仓库中,`HEAD`其实并不是指向提交,而是指向的当前分支,只是先前只有一个分支master,`HEAD`指向的就是当前分支

当我们新创建一个分支的时候,git就会新创建一个分支指针,该指针指向master相同的提交,如果再把`HEAD`指向该分支,当前分支就在该分支上,然后再切回master分支就可以将在该分支上的操作合并到master分支上,最后删除该分支,使得修改更加安全

- `git checkout -b dev`创建一个dev分支,并且切换到dev分支,这个加了`-b`的指令相当于先后执行了`git branch dev`创建分支和`git checkout dev`将当期分支切换到dev分支上
- 直接使用`git branch`命令可以查看当前分支。`git branch`命令会列出所有分支，当前分支前面会标一个`*`号
- 然后可以在这个dev分支上进行各种和前面一样的提交操作
- 但是使用`git checkout master`切换到master分支的时候会发现在dev分支上提交的所有修改和操作都没有,因为这其实是两个分支了,刚刚是在deb分支上做的修改,所有master分支上看文件是没有发生改变的
- 可以使用`git merge dev`用于合并指定分支到当前分支,注意这个时候是已经将当前分支切换到了master分支上了的,在查看文件的修改,会发现和在dev分支上的修改一样,说明合并是成功了的
- 然后就可以使用`git branch -d dev`将dev分支删除掉,再次使用`git branch`会发现已经没有dev分支了

**注：**通常，合并分支时，如果可能，git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息,也就是首不知道是否进行了分支的合并,如果要强制禁用`Fast forward`模式，git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。我们可以使用`git merge --no-ff -m "提交内容" 合并分支`,这样就可以使用`git log`查看分支历史



## 6.暂缓工作区

当我们遇到BUG时,需要修复BUG,但是手头又有工作,这是需要先暂缓手头工作,使用`git stash`储存手头工作,不管有没有提交,这时`git status`会发现工作区是干净的,然后确立哪个分支出BUG,在那个分支上创建一个新的分支,修改完成后合并到那个分支(不一定是master),删除新创建分支,然后使用回到原来工作的分支,恢复工作区：

- 可以先使用`git stash list`查看所有暂存工作区
- 然后恢复工作区,有两种方法：
  - 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，需要用`git stash drop`来删除
  - 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了
- 在使用`git stash list`就看不到暂存内容了,也可以多次暂存工作区,每一次暂存都会在列表中添加一个,使用`git stash apply 指定工作区(stash@{0})`就可以恢复到对应的工作区



## 7.多人协作

当从远程仓库克隆时，实际上git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`

- `git remote`：该命令可以查看远程仓库的信息,一般只有一个远程仓库的名字，使用`git remote -v`可以查看远程仓库的详细信息

  ```shell
  origin  git@github.com:michaelliao/learngit.git (fetch)
  origin  git@github.com:michaelliao/learngit.git (push)
  ```

  上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址

- `git push 对应仓库名 对应分支名`：把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上,如`git push origin master`和`git push origin dev`

  **但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？**

  - `master`分支是主分支，因此要时刻与远程同步
  - `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步
  - bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug
  - feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发
  
- `git push --delete 分支名`:删除远程上的某个分支

- `get fetch`：更新远程仓库的所有分支到本地（并不是在本地创建，而是在使用`git branch -r`查看远程分支的时候能找到对应的远程分支）

### 7.1 合作开发

在多人合作时，大家都会往`master`和`dev`分支上推送各自的修改,当一个成员克隆一个git项目时,	新的成员默认情况下只能看到`master`分支上的内容,用`git branch`看也只会有一个`master`分支显示,如果要在`dev`分支上进行开发,那么就需要创建远程库的`dev`分支都本地,使用`git checkout -b dev origin/dev`来创建,这样在本地的`dev`上做修改就能上传到远程的`dev`分支了



### 7.2 推送冲突

如果你的提交和远程仓库中别人最新的提交有冲突,那么就会推送失败,无法推送到远程仓库,这个时候先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送。但是这个`dev`分支会`pull`失败,原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示(pull错误里面会有)，设置`dev`和`origin/dev`的链接：`git branch --set-upstream-to=origin/dev dev`。再次`pull`就能成功了，然后就是手动解决冲突的问题,然后再次提交,再次`push`

强制让远程分支覆盖本地分支：

```shell
git fetch --all
git reset --hard origin/master
git pull origin master
```





### 7.3 rebase（最好使用这个来代替 merge）

[链接](http://gitbook.liuhui998.com/4_2.html)



## 8.标签管理

切换到需要打标签的分支上，然后敲命令`git tag <name>`就可以打上标签，注意标签其实是打在提交上的，如`git tag v1.0`，可以用`git tag`查询所有的标签，默认标签是打在最新提交的commit上的，可以通过`git tag v0.9 提交的id(f52c633)`来为指定的提交打上标签,使用`git show <tagname>`就可以显示该标签的具体信息,简化操作,如：`git show v0.9`

还可以创建带有说明的标签`git tag -a v0.1 -m "version 0.1 released" 1094adb`，用`-a`指定标签名，`-m`指定说明文字

**注意：**标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签

- `git tag -d <tagname>`：删除指定标签，因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除
- `git push origin <tagnam`：推送某个标签到远程仓库,也可以使用`git push origin --tags`一次性全部推送
- 如果要删除远程仓库的标签,使用`git tag -d v0.9`先将本地的删除,然后使用`git push origin :refs/tags/v0.9`把远程标签删除



## 9.忽略文件

使用windows时,如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了

如果有时添加不了一个文件,就是因为该文件被`.gitignore`忽略了,如果确实想添加,使用使用`git add -f 添加的文件`进行强制添加,或者也可以查看在`.gitignore`中哪个文件使用出错了.通过`git check-ignore`命令进行查看，如`git check-ignore -v App.class`



## 10.配置别名

[链接](https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424)



## 11.Git服务器

[链接](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)



## 12.Git规范

build：主要目的是修改项目构建系统(例如 glup，webpack，rollup 的配置等)的提交
ci：主要目的是修改项目继续集成流程(例如 Travis，Jenkins，GitLab CI，Circle等)的提交
docs：文档更新
feat：新增功能
merge：分支合并 Merge branch ? of ?
fix：bug 修复
perf：性能, 体验优化
refactor：重构代码(既没有新增功能，也没有修复 bug)
style：不影响程序逻辑的代码修改(修改空白字符，格式缩进，补全缺失的分号等，没有改变代码逻辑)
test：新增测试用例或是更新现有测试
revert：回滚某个更早之前的提交
chore：不属于以上类型的其他类型