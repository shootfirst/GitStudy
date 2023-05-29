# GitStudy

我的git学习笔记

https://blog.csdn.net/qq_32452623/category_6675038.html
https://www.jiyik.com/w/git/git-merge
https://www.yiibai.com/git/git_merge.html
https://www.cnblogs.com/qdhxhz/p/9757390.html
## git区域与状态

### 区域

+ 工作目录

+ 暂存区

+ 本地仓库

+ 远程仓库

### 文件状态

+ 未跟踪

+ 已修改

+ 已暂存

+ 已提交

+ 未修改

![git区域和文件状态流转](./git_file_status.png)



## git 命令


### Start a working area

#### git clone 

git clone主要用于指向现有的仓库，并在另一个新目录中创建该仓库的克隆副本。
克隆会自动创建一个名为“origin”的远程连接，指向原始仓库。

#### git init 

git init命令会创建一个新的 Git 仓库库。它可用于将现有的、未进行版本控制的项目转换为 Git 仓库或初始化一个新的空的仓库。
会在当前工作目录中创建一个子目录.git，其中包含新仓库所需的所有 Git 元数据。此元数据包括对象、引用和模板文件等子目录。
还会创建一个HEAD文件，指向当前签出的提交。



### Work on the current change

#### git add

将文件添加到暂存区

#### git restore

还原工作区

git restore --staged 文件名： 将暂存区的修改恢复到工作区（包括文件删除，新增）

git restore 文件名：丢弃工作区的修改（不包括文件删除，新增）

#### git stash

暂时隐藏我们工作区和暂存区的修改，让我们切换其他分支处理完事情后回来继续工作

git stash list

git stash pop

git stash save "此次stash的message"

git stash drop $stashid



### Show Status

#### git diff

作用是比较更改

下面是git diff的输出格式

比较输入：
diff --git a/README.md b/README.md

元数据：忽略
index ae53de2..d0d4980 100644

变化标记：
--- a/README.md
+++ b/README.md

差异块：从62行开始提取了六行，从34行开始添加了25行
@@ -62,6 +62,25 @@ 

-删除的内容
+新增的内容

#### git status

展示当前工作目录、暂存区和当前分支的状态

#### git log

查看当前分支的历史提交情况

#### git reflog

能够记录几乎所有本地仓库的改变，包括所有分支的commit提交，以及已经被删除的commit






### Commit and branch

#### git commit 

提交文件

+ 提交附加commit信息 git commit -m "the commit message" 
+ 先把所有已经track的文件的改动`git add`进来，然后提交  git commit -a
+ 增补提交，会使用与当前提交节点相同的父节点进行一次新的提交，旧的提交将会被取消  git commit --amend 

#### git branch

分支相关操作

+ 查看本地
+ 新建本地分支
+ 查看本地和远程分支
+ 修改分支的名字
+ 删除本地分支
+ 手动建立追踪关系

#### git checkout

HEAD指针：既可以指向快照节点，也可以指向branch
当指向branch的时候，提交后会随着branch指针一起向后移动
当不指向branch的时候，提交的时候会在一个detached状态

git checkout 操控三个不同对实体 文件，提交和分支（分支属于特殊的提交）

git checkout 节点哈希值：将分离HEAD指向此commit

git checkout 分支名：将HEAD指向指定分支

git checkout 分支名/节点哈希值 文件路径名：签出文件 ，将当前工作区文件修改(省略分支名则恢复上一次commit)

#### git merge

将指定分支合并到当前分支
Fast Forward 和 三路合并
对于三路合并，会自动提交一次

#### git rebase

将指定分支rebase到当前分支
不能在协作分支上进行rebase操作！！！
当rebase完，本地分支与远程分支不一样时，可以直接push-f
git pull –rebase 会使commit看起来很自然,因为代码都有一个前后依赖，看起来更加的直观

[rebase vs merge](https://joyohub.com/2020/04/06/git-rebase/)

#### git reset

修改git三棵树

git三棵树：

+ HEAD：上一次的提交，下一次commit的父节点

+ index：暂存区，下一次被提交的内容

+ working directory：沙盒

git reset会以特定顺序修改这三棵树，若

git reset --soft $HASH #移动HEAD到指向节点

git reset --mixed $HASH #修改HEAD和index到指向节点

git reset --hard $HASH #修改三者内容，前面的commit将会成为孤儿节点，会被git 定期垃圾回收

#### git revert

git revert命令可以被认为是“撤消”命令。但是它不是传统的撤消操作。
不是从项目历史中删除提交，而是计算出如何反转要撤销的提交所引入的更改，
并附加一个新的提交及生成的反向内容。这种方式可以防止 Git 丢失历史记录

#### git cherry-pick

它允许我们通过引用选择任意 Git 提交并将其附加到当前工作分支的 HEAD

```
a - b - c - d   Main
         \
           e - f - g Feature

$ (Main) git cherry-pick f-hash

a - b - c - d - f   Main
         \
           e - f - g Feature
```



### Remote collaborate

#### git push

对远程分支进行修改操作

格式：git push 主机名 本地分支名 远程分支名

如果省略远程分支名，则表示将本地分支推送与之存在”追踪关系”的远程分支(通常两者同名)，如果该远程分支不存在，则会被新建

如果省略本地分支名而不省略远程，则等同于删除远程分支

-u 参数：选项指定一个默认主机，这样后面就可以不加任何参数使用git push

--all 参数：不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机

--tags 参数：推送标签

-f 参数：强行覆盖远程分支，远程分支相关修改会被丢弃

git push 主机名 --delete 分支名：删除对应主机远程分支

#### git pull

取回远程的分支，和本地对应的分支合并，本地若不存在则新建本地分支（git fetch + git merge）

格式：git pull 主机名 远程分支名 本地分支名

git pull --rebase <remote> #使用rebase而不是merge

#### git fetch

获取远程分支，但不合并

git fetch <remote>

git fetch <remote> <branch>

#### git remote

管理远程仓库

git remote 列出所有远程仓库

git remote add <name> <url>添加远程仓库




## git底层

### Object

#### Blob

#### Tree

#### Commit

#### Tag

#### .git目录

.git
├── COMMIT_EDITMSG
├── FETCH_HEAD
├── HEAD
├── ORIG_HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       ├── heads
│       │   └── main
│       └── remotes
│           └── origin
│               ├── HEAD
│               └── main
├── objects
│   ├── 02
│   │   └── 0db89ffa220224b8f59279a9600c59341e424e
│   ├── 0d
│   │   └── 30dbb9e66cc93293a22e406811b28d1fd3a859
│   ├── 0f
│   │   └── 0d692c857e6fa749c8d9e5a80162b86a3f4d73
│   ├── 14
│   │   └── 1453dedc8e73e02754b841fd3fc1bb55e0bbaa
│   ├── 1c
│   │   └── 8b28981dc3ae2db0620f3758e508bd09062a32
│   ├── 33
│   │   └── b7434f452589747f5cbe9e18e3bf88cb354a22
│   ├── 50
│   │   └── 9c14065f26804b03d64843578df7d20ba72a94
│   ├── 51
│   │   └── 78eb31c5b4c68c2b32650ba94274736e1066a1
│   ├── 54
│   │   └── 8ad7eedc05dc4e064afa45f7cd0ac2f7f542e8
│   ├── 63
│   │   └── bab07b7f0a9f11d947e7dd45398d2d48b83425
│   ├── 65
│   │   └── 356e56e861bba6a18eddc9695d3f387e738cd8
│   ├── 67
│   │   └── 069b1b3b2e57f0e2ee9b57838ecf14eebe7240
│   ├── 6b
│   │   └── 19a6ec61399811c4753943242f7c5780131fca
│   ├── 6d
│   │   └── afa3cf821f7f96c973586107bec55699232066
│   ├── 6e
│   │   └── 229ec31970c14c82e910fb71e0ac9ab7fc0b26
│   ├── 74
│   │   └── dee86bc06643f98e06cee26dacaa776cbb1106
│   ├── 78
│   │   └── 52a576597d71d500e2947636507b5eaa53d9a6
│   ├── 7d
│   │   └── 715bde4301adf5899bfb4e9dda33c9eb444675
│   ├── 80
│   │   └── 5a39062ff8eb27e879bd005105430ee7be91e0
│   ├── 83
│   │   └── 36dee04e5ae577ff327b69da5808126a430192
│   ├── 85
│   │   └── a8fc7bb56cfbf9cf55fa592e5659cb6b48dba9
│   ├── 8c
│   │   └── 3d6cc1064ac7551ac992490c653aa54f0a7621
│   ├── 99
│   │   └── 8c993fe8141a431341226539d7c551481e3aa5
│   ├── a3
│   │   └── 948873e7e144b351781093026ff458ad2b4b52
│   ├── aa
│   │   └── 6313117423472fb0c417592884c79bf2ce3df7
│   ├── b6
│   │   ├── 3676af53b9fda5d66046ee6fd6d09cd1b8832a
│   │   └── 7af098fffd978bc97562c7e9e2a2f8a428d6ba
│   ├── cc
│   │   └── ce4a97640d7ecdd35ddbc19e41d4b8236197f9
│   ├── cd
│   │   └── 7cd76e5d73ce96c4219465effa11a103a07154
│   ├── d1
│   │   └── d4c13434e3a50608de7a82dd30e4edbe7d45bd
│   ├── d2
│   │   └── 40b8ede1f88cf7dc7f98989e9c3f821404b490
│   ├── d7
│   │   ├── 929563ffcb6a0412248b27aa41aba53d2bda43
│   │   └── f006965fed169945ba982dfc53e1893af89ad7
│   ├── df
│   │   └── 9ec6b1fed82cb818e850d51f756438a346f66b
│   ├── e2
│   │   └── 75eeb2d18c8965e88e245aaf1cb8e4a394f158
│   ├── e9
│   │   └── 0b9755a365950a0dd63bbab1336f03e9b65578
│   ├── f6
│   │   └── 9e169465fc32c245eaa556ccd745fd7de2d849
│   ├── info
│   └── pack
│       ├── pack-b812e0455316747fc0bb9f6976369675d0dd4907.idx
│       └── pack-b812e0455316747fc0bb9f6976369675d0dd4907.pack
├── packed-refs
└── refs
    ├── heads
    │   └── main
    ├── remotes
    │   └── origin
    │       ├── HEAD
    │       └── main
    └── tags

## git修改

main：12