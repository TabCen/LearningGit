#Git初阶

##常见名词
- VCS （Version Control System）版本控制系统
- repository 仓库 
- track 跟踪
- stage 暂存
- commit 提交
- push 推送
- pull 拉取



##三种状态
- committed（已提交）
	>已提交表示数据已经安全得保存到本地数据库中。
	
- modified （已修改）
	>已修改表示修改了文件，但是还没有保存到数据库中。
		
- staged   （已暂存）
	>已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

##常见操作
- 设置git初始化信息(设置姓名和邮箱)

使用--global命令后，那么该命令只需要运行一次。

```
$ git config --global user.name "chenfei"
$ git config --global user.email chenfeigogo@gmail.com
```

- 查看配置信息

```
$ git config --list
```

- 查看git文档

```
git help <verb>
如：
查看init的文档
git help init
```
##基本操作

- 创建

```
$git init
或者
$git clone https://........
```

- 添加到暂存

```
$git add <file>
```

- 提交到版本库

```
$git commit -m "备注"
```

- 查看状态

```
查看详细
$git status

查看简略
$git status -s
或者
$git status --short

```

- 忽略文件

