#Git学习笔记
>廖雪峰课程 [链接](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)


##创建git版本库*repository*
* `git init`创建文件，进入文件目录下进行创建<font color=#EB3434 size=2 face="黑体">尽量不要取中文目录</font>

```
$ mkdir LearningGit
$ cd LearningGit
$ pwd
/Users/FeiChen/LearningGit
$ git init
```

* `git add`告诉git将文件添加到仓库。 *add操作实际上是将文件保存到暂存取，暂存区在.git中*

```
$ git add text.txt
```

* `git commit`将修改提交到仓库。*commit操作是将暂存区的所有内容保存到当前版本库中HEAD*

```
$ git commit -m "add text.txt"
[master (root-commit) 6e92b3f] add text.txt
 1 file changed, 1 insertion(+)
 create mode 100644 text.txt
```

##查看Git状态
* `git status`命令可以让我们时刻掌握仓库当前的状态

```
$ git status
On branch master
nothing to commit, working tree clean
```

* `git diff`查看不同

```
$ git diff readme.txt 
```

##版本管理
* `git log`查看历史记录

```
$ git log
$ git log --pretty=oneline		<!--查看一行信息-->
2093c65d2ae11c72b1e4ea80edc66f9cdb517925 add something to text.txt
17df3deb2b03cce0cc5d2ce3c864b9e3b98c5ca1 add a readme.md
6e92b3f4be4436ccf658a581d89249887b64959e add text.txt

```

* `HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`、`HEAD~100`
* `git reset --hard HEAD^`回滚方式

```
$ git reset --hard HEAD^
HEAD is now at ea34578 add distributed

$ git reset --hard 2093c6

```
*`git reflog`用来记录你的每一次命令

```
$ git reflog
17df3de HEAD@{0}: reset: moving to HEAD^
2093c65 HEAD@{1}: reset: moving to 2093c65d2ae11c72b1e4ea80edc66f9cdb517925
17df3de HEAD@{2}: reset: moving to HEAD^
2093c65 HEAD@{3}: commit: add something to text.txt
17df3de HEAD@{4}: commit: add a readme.md
6e92b3f HEAD@{5}: commit (initial): add text.txt
```
##撤销修改

* `git checkout -- file`可以撤销工作区的修改。相当于从版本库中重新checkout一份
<font color=#EB3434 size=2 face="黑体">注意‘--’不可少</font>

* `git reset HEAD file`可以撤销add后没commit的文件。

##删除文件

* `git rm`与`git add`相反，表示从暂存区删除文件

```
git rm text.txt
```

##远程仓库

* 先在GitHub中创建一个新的仓库,将本地仓库推至远程仓库

```
$ git remote add origin git@github.com:TabCen/LearningGit.git

$ git push -u origin master<!--第一次一般有‘-u’-->
$  git push origin master

Counting objects: 8, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (8/8), 769 bytes | 0 bytes/s, done.
Total 8 (delta 0), reused 0 (delta 0)
To github.com:TabCen/Learninggit.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.

```
* 从远程仓库clone到本地

```
$ git clone git@github.com:TabCen/LearningGit.git
Cloning into 'LearningGit'...
remote: Counting objects: 8, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 8 (delta 0), reused 8 (delta 0), pack-reused 0
Receiving objects: 100% (8/8), done.
Checking connectivity... done.

```
<font color=#EB3434 size=3 face="黑体">Git支持多种`协议`，包括`https`，但通过`ssh`支持的原生git协议速度最快。</font>

##创建分支&合并分支

* 首先创建分支，并且切换到分支

```
$ git checkout -b dev
M	readme.md
Switched to a new branch 'dev'
```
* `git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

 
``` 
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

* `git branch`查看分支 ,当前分支前面会标一个*号。

```
$ git branch
* dev
  master
```

* `git merge`合并指定分支到当前分支，所以一般要先将分支切换到需要合并的分支。

```
$ git merge dev
Updating 113dfee..aa4fbfc
Fast-forward
 test.txt | 2 ++
 1 file changed, 2 insertions(+)

```

* `$ git branch -d dev`删除分支

```
$ git branch -d dev
Deleted branch dev (was aa4fbfc).

```
>小结
>>	查看分支：git branch</br>
>>	创建分支：git branch name</br>
>>	切换分支：git checkout name</br>
>>	创建+切换分支：git checkout -b name</br>
>>	合并某分支到当前分支：git merge name</br>
>>	删除分支：git branch -d name </br>

##解决冲突

* 合并分支时出现冲突

```
$ git merge dev
Auto-merging test.txt
CONFLICT (content): Merge conflict in test.txt
Automatic merge failed; fix conflicts and then commit the result.

```
* 解决完冲突后需要将文件再次提交
* `$ git log --graph`查看分支合并图

```
$ git log --graph --pretty=oneline --abbrev-commit
*   ef741b1 解决了冲突
|\  
| * 4425bf8 addtest.txt
* | cb4b618 删除了一些东西
|/  
*   b53ce9a Merge branch 'dev'
```

* 如果需要删除分支最后删除分支，如果不需要，保留这个分支，但是发现分支的信息全部没了，原因是git采用了`Fast forward`模式

* 强制禁止`Fast forward`模式

```
$ git merge --no-ff -m "merge with no-ff" dev
```

##Bug分支

* 当正在dev分支中工作时，工作到一半，测试发现master中出现了一个bug需要紧急解决，这个可以对master分出一个bug分支，而dev需要`stash`

```
git stash			<!--将修改暂存-->
git stash pop		<!--还原保存-->
git stash list 	<!--查看stash-->
```

##标签管理
`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个commit id；

`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

`git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；

`git tag`可以查看所有标签。

------
#命令行指令
- `pwd`用于查看当前文件的目录

```
$ pwd
/Users/FeiChen

```

- `mkdir`用与创建文件夹

```
$ mkdir LearningGit

```

- `vim` 创建并且查看一个文件,编辑完先`esc`再输入`wq`

```
$ vim text.txt
```

- `cat`查看文件内容

- `rm`删除文件

```
rm text.txt
```




------
#MarkDown学习

```css

<font face="黑体">我是黑体字</font>

<font face="微软雅黑">我是微软雅黑</font>

<font face="STCAIYUN">我是华文彩云</font>

<font color=#EB3434 size=3 face="黑体">color</font>

<font color=#00ffff size=3>color</font>

<font color=gray size=3>color</font>

```
