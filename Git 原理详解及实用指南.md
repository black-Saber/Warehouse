Git的最基本的工作流程

```
1.从仓库拉代码到本地

git clone https://github.com/black-Saber/Warehouse.git  Warehouses(拉取到本地，项目别名)

从线上拉去最新的项目代码

git pull

2.新增,修改,删除,仓库文件（本地操作）

3.将所有本地修改的代码提交到暂存区：git add .（全部暂存）

4.将所有本地修改的代码提交到本地仓库：git commit -m 描述（add,update,delete+本次提交的描述）

5.将所有本地修改的代码提交中央远程仓库：git push

```

```

status 是用来查看工作目录当前状态的指令：

git status

每个文件有 "changed /unstaged"（已修改）,"staged"（已修改并暂存）,"commited"（已提交） 三种状态，
以及一种特殊状态 "untracked"（未跟踪）

log 是用来查看工作目录提交到远程仓库的记录的指令：(查看历史记录)

git log

PS D:\Desketop\Warehouse> git log
commit b9217e145f60dad51f97a01321eaff3b043dcafb (HEAD -> main, origin/main[主分支], origin/HEAD)
Author: black-saber <1203606379@qq.comm>
Date:   Tue Nov 24 10:37:53 2020 +0800

    .[本次提交的描述]

```

branch (分支)

```
创建branch

git branch test(创建一个test分支)

git checkout test(切换到test分支上面)

上述两步合并：git checkout -b test(创建一个test分支，并且切换到test分支上面)

git branch (查看分支列表)

HEAD 指向的  branch 不能删除。如果要删除  HEAD 指向的  branch ，需要先用checkout 把  HEAD 指向其他地方。

出于安全考虑，没有被合并到  master 过的  branch 在删除时会失败（因为怕你误删掉「未完成」的  branch 啊）：
这种情况如果你确认是要删除这个branch （例如某个未完成的功能被团队确认永久毙掉了，不再做了），可以把  -
d 改成  -D ，小写换成大写，就能删除了。

git branch -d test(删除test分支)

git branch -D test(删除没有合并到master的test分支)



A角色，分支上开发

1.创建test分支，并且在test分支中修改项目
2.git add .
3.git commit -m .
4.git push origin test（将test分支推送到线上主分支）

B角色，拉去test分支在主分支

git pull origin test(方法一)
(方法二)
git pull
git merge origin/test(合并分支)
git add .
git commit -m .
git push

A角色，删除test分支

git branch -D test(删除本地test分支)
git push  origin --delete test（删除线上test分支）

（如果是游离态的问题）--------------------------------------------------------------

A角色，删除detached的test分支

PS D:\Desketop\Warehouse> git branch -v
* (HEAD detached at refs/heads/demo) 7cdf2e9 .
  main                               f830eeb .
  test                               7adf2e9 .
PS D:\Desketop\Warehouse> git branch ln 7331feb (将游离的分支缓存再一个新的分支[demo]上面)

修改主分支内容
git add .
git commit -m .
git push 
git branch -d demo 删除分支

```

```

HEAD 是指向当前commit 的引用，它具有唯一性，每个仓库中只有一个  HEAD 。在每次提交时它都会自动向前移动到最新
的commit 。

branch 是一类引用。 HEAD 除了直接指向commit ，也可以通过指向某个  branch 来间接指向  commit 。当  HEAD 指向
一个branch 时， commit 发生时， HEAD 会带着它所指向的  branch 一起移动。


master 是 Git 中的默认  branch ，它和其它  branch 的区别在于：
i. 新建的仓库中的第一个  commit 会被  master 自动指向；
ii. 在  git clone 时，会自动checkout 出  master 。

```

push(本质)

```

push：把 branch 上传到远端仓库

实质上， push 做的事是：把当前  branch 的位置（即它指向哪个  commit ）上传到远端仓库，并把它的路径上的
commit s 一并上传。

例如，我现在的本地仓库有一个master ，它超前了远程仓库两个提交；另外还有一个新建的branch 叫feature1 ，
远程仓库还没有记载过它。具体大概像这样：

```

关于 add

```
1. add 后面加个点 "."：全部暂存

git add .

2. add 添加的是文件改动，而不是文件名

git add a.txt


```

查看修改历史

```

查看历史记录：git log

查看详细历史: git log -p 

查看简要统计: git log --stat

查看具体的commit：git show 5e68b0d8

	看任意一个 commit：git show 5e68b0d8
	
	看指定 commit 中的指定文件：git show 5e68b0d8 shoppinglist.txt
	
看未提交的内容:修改本地项目，并且git add .  然后输入：git diff

	比对暂存区和上一条提交:修改本地项目，并且git add .  然后输入：git diff --staged

	比对工作目录和上一条提交:git diff HEAD

```

高级一：使用rebase变基(主分支合并其他分支操作)
```

git checkout -b test(创建test分支，然后修改test分支内容)
git add .
git commit -m .
git rebase test
git checkout main(master主分支)
git merge test(主分支合并test分支)
git push (上传远程仓库)

```

修改之前版本代码

```

1. 修改刚刚提交的代码
	
	git add .

	git commit --amend

2. 修改到时第二个版本的代码

	git rebase -i HEAD^^

	说明：在 Git 中，有两个「偏移符号」：  ^和  ~ 。
	
	^ 的用法：在  commit 的后面加一个或多个  ^ 号，可以把  commit 往回偏移，偏移的数量是  ^ 的数量。例如： master^ 表示master 指向的  commit 之前的那个commit ；  HEAD^^ 表示  HEAD 所指向的commit 往前数两个  commit 。
	
	~ 的用法：在  commit 的后面加上  ~ 号和一个数，可以把  commit 往回偏移，偏移的数量是  ~ 号后面的数。例如： HEAD~5表示  HEAD 指向的  commit 往前数 5 个commit 。	

	但你的目标是修改倒数第二个  commit ，也就是上面的那个「增加常见笑声集合」，所以你需要把它的操作指令从  pick 改成  edit 。  edit 的意思是「应用这个 commit，然后停下来等待继续修正」。把  pick 修改成  edit 后，就可以退出编辑界面了：

	git add .
	git commit --amend
	git rebase --continue

1. 使用方式是  git rebase -i 目标commit 
2. 在编辑界面中指定需要操作的  commit s 以及操作类型；
3. 操作完成之后用  git rebase --continue 来继续  rebase 过程。

```

比错还错，想直接丢弃刚写的提交？

```
reset --hard 丢弃最新的提交

git reset --hard HEAD^  （回到上一个commit）   丢弃最新的提交

想丢弃的也不是最新的提交？

你想撤销倒数第二条  commit ，那么可以使用rebase -i 

git rebase -i HEAD^^

然后就会跳到  commit 序列的编辑界面，这个在之前的第 10 节里已经讲过了。和第 10 节一样，你需要修改这个序列来进行操作。不过不同的是，之前讲的修正  commit 的方法是把要修改的commit 左边的  pick 改成  edit ，而如果你要撤销某个  commit ，做法就更加简单粗暴一点：直接删掉这一行就好。 然后esc+大写的ZZ

用 rebase --onto 撤销提交

rebase 加上  --onto 选项之后，可以指定rebase 的「起点」。一般的  rebase ，告诉 Git的是「我要把当前  commit 以及它之前的
commit s 重新提交到目标  commit 上去，这其中， rebase 的「起点」是自动判定的：选取当前commit 和目标  commit 在历史上的交叉点作为起点。

git rebase 第3个commit

而  --onto 参数，就可以额外给 rebase 指定它的起点。例如同样以上图为例，如果我只想把  5 提交到  3 上，不想附带上  4 ，那么我可以执行

git rebase --onto 第3个commit（test分支） 第4个commit（master主分支） branch1（test分支）

同样的，你也可以用  rebase --onto 来撤销提交：

git rebase --onto HEAD^^ HEAD^ branch1

上面这行代码的意思是：以倒数第二个commit为起点（起点不包含在  rebase 序列里哟），branch1 为终点， rebase 到倒数第三个
commit 上。

撤销过往的提交：方法有两种，理念是一样的：在  rebase 的过程中去掉想撤销的  commit ，让他它消失在历史中
用  git rebase -i 在编辑界面中删除想撤销的  commit s
用  git rebase --onto 在 rebase 命令中直接剔除想撤销的  commit s

```


代码已经 push 上去了才发现写错？

```
有的时候，代码  push 到了中央仓库，才发现有个  commit 写错了。这种问题的处理分两种情况：

1. 出错的内容在你自己的 branch

本地的内容(branch1分支)强行覆盖掉中央仓库的内容(branch1分支)
git push origin branch1 -f

2. 出错的内容已经合并到 master

git revert HEAD^  （撤销上一个commit,想撤销哪个，后面添加哪个has1的记录）

```


reset 的本质：移动 HEAD 以及它所指向的 branch

```

reset --hard：重置工作目录

git reset --hard HEAD^

你的  HEAD 和当前  branch 切到上一条commit 的同时，你工作目录里的新改动也一起全都消失了，不管它们是否被放进暂存区

reset --soft：保留工作目录

git reset --soft HEAD^

reset --soft 会在重置  HEAD 和  branch时，保留工作目录和暂存区中的内容，并把重置HEAD 所带来的新的差异放进暂存区。

reset 不加参数：保留工作目录，并清空暂存区

git reset HEAD^

工作目录的内容和  --soft 一样会被保留，但和--soft 的区别在于，它会把暂存区清空

```

checkout 的本质
```

git checkout branch名 的本质，其实是把HEAD 指向指定的  branch ，然后签出这个branch 所对应的  commit 的工作目录。所以同
样的， checkout 的目标也可以不是  branch ，而直接指定某个  commit ：

git checkout HEAD^^
git checkout master~5
git checkout 78a4bc
git checkout 78a4bc^

这些都是可以的

在git status 的提示语中，Git 会告诉你可以用checkout -- 文件名 的格式，通过「签出」的方式来撤销指定文件的修改：

checkout有一个专门用来只让HEAD和branch 脱离而不移动HEAD 的用法：

git checkout --detach

执行这行代码，Git 就会把  HEAD 和  branch 脱离，直接指向当前  commit ：

stash：临时存放工作目录的改动

git stash （工作目录暂时清理干净）

当前的工作分支切到  master去给你的同事打包，打完包，切回你的分支

git stash pop （你之前存储的东西就都回来了）

注意：没有被 track 的文件（即从来没有被 add 过的文件不会被 stash 起来，因为 Git 会忽略它们。如果想把这些文件也一起 stash，可以加上 `-u` 参数，它是 `--include-untracked` 的简写。就像这样：

git stash -u （暂存没有add的文件）

```

branch 删过了才想起来有用？

```

reflog ：引用的 log

reflog 是 "reference log" 的缩写，使用它可以查看 Git 仓库中的引用的移动记录。如果不指定引用，它会显示  HEAD 的移动记录。假如你误删了branch1 这个  branch ，那么你可以查看一下HEAD 的移动历史：

git reflog

从图中可以看出， HEAD 的最后一次移动行为是「从  branch1 移动到  master 」。而在这之后， branch1 就被删除了。所以它之前的那个
commit 就是  branch1 被删除之前的位置了，也就是第二行的  c08de9a 。

所以现在就可以切换回  c08de9a ，然后重新创建branch1 ：

git checkout c08de9a

git checkout -b branch1

这样，你刚删除的  branch1 就找回来了。

注意：不再被引用直接或间接指向的commit s 会在一定时间后被 Git 回收，所以使用  reflog 来找回删除的  branch 的操作一定要及时，不然有可能会由于  commit 被回收而再也找不回来。



查看其他引用的 reflog

reflog 默认查看  HEAD 的移动历史，除此之外，也可以手动加上名称来查看其他引用的移动历史，例如某个  branch ：

git reflog master

```

不想被管理的文件和目录

```
在 Git 中有一个特殊的文本文件： .gitignore 。这个文本文件记录了所有你希望被 Git 忽略的目录和文件


# 打头的是注释文件

其他的都是对忽略文件的配置


```

tag：不可移动的 branch

```

tag 是一个和  branch 非常相似的概念，它和branch 最大的区别是： tag 不能移动。所以在很多团队中， tag 被用来在关键版本处打标记
用。

创建标签:git tag v1.0

查看已有标签:git tag

删除标签:git tag -d v1.0

查看此版本所修改的内容:git show v1.0

提交远程代码库：git push origin --tags

获取远程版本：git fetch origin tag V1.2

```
```
cherry-pick：把选中的 commits 一个个合并进来

cherry-pick 是一种特殊的合并操作，使用它可以点选一批  commit s，按序合并。更多关于  cherry-pick:

https://git-scm.com/docs/git-cherry-pick


git config： Git 的设置

git config 可以对 Git 做出基础设置，例如用户名、用户邮箱，以及界面的展示形式。内容虽然多，但都不难，整体看一遍，把 Git 设置成你最舒服的样子，从此就再也不用管它了。属于「一次付出，终身受用」的高性价比内容。

https://git-scm.com/docs/git-config

Git Flow：复杂又高效的工作流:https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
```
































