- [官网](https://git-scm.com/)
- 书籍: [Pro git](https://git-scm.com/book/zh/v2)
- [项目组仓库](https://code.aliyun.com/groups/xjl_xsh)

# Git 基础概念

## 三种状态

Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。 已提交表示数据已经安全的保存在本地数据库中。 已修改表示修改了文件，但还没保存到数据库中。 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。

![三个工作区域](./areas.png)

__工作目录、暂存区域以及 Git 仓库。__

Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作`‘索引’'，不过一般说法还是叫暂存区域。

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。

2. 暂存文件，将文件的快照放入暂存区域。

3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。


## 记录每次更新到仓库

请记住，你工作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。所以使用 Git 时文件的生命周期如下：

![三个工作区域](./lifecycle.png)


# 基本命令
## 前置操作

1. 安装好 Git 之后 使用 `git --version` 验证一下是否安装成功
1. `git config user.email $你的邮箱`
1. `git config user.name $你在项目中的昵称`
2. `mkdir $目录名` 创建项目文件夹 
3. `cd $目录名` 到项目的根目录
4. `git init` 初始化项目

## 检查当前文件状态
要查看哪些文件处于什么状态，可以用 `git status` 命令。 如果在克隆仓库后立即使用此命令，会看到类似这样的输出：

```bash
# 刚初始化的项目的输出
$ git status

On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)

# 给i他有过提交的项目的输出
$ git status

On branch master

nothing to commit, working directory clean
```

这说明你现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过。 此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来。 最后，该命令还显示了当前所在分支，并告诉你这个分支同远程服务器上对应的分支没有偏离。 现在，分支名是 “master”,这是默认的分支名。

在项目下创建一个新的 README 文件。 之前并不存在这个文件，使用 `git status` 命令，你将看到一个新的未跟踪文件：

```bash
$ echo 'My Project' > README
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
```

在状态报告中可以看到新建的 README 文件出现在 Untracked files 下面。 未跟踪的文件意味着 Git 在之前的快照（提交）中没有这些文件；Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文件”， 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。 不过现在的例子中，我们确实想要跟踪管理 README 这个文件。

## 跟踪新文件
使用命令 `git add` 开始跟踪一个文件。 所以，要跟踪 README 文件，运行：

```
$ git add README
```

此时再运行 `git status` 命令，会看到 README 文件已被跟踪，并处于暂存状态：
```bash
$ git status

On branch master

No commits yet

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
```

只要在 Changes to be committed 这行下面的，就说明是已暂存状态。 如果此时提交，那么该文件此时此刻的版本将被留存在历史记录中。 git add 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

## 暂存已修改文件
现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运行 git status 命令，会看到下面内容：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

文件 CONTRIBUTING.md 出现在 Changes not staged for commit 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。 要暂存这次更新，需要运行 git add 命令。 这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 __将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。__ 现在让我们运行 git add 将"CONTRIBUTING.md"放到暂存区，然后再看看 git status 的输出：

```
$ git add CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```

现在两个文件都已暂存，下次提交时就会一并记录到仓库。 假设此时，你想要在 CONTRIBUTING.md 里再加条注释， 重新编辑存盘后，准备好提交。 不过且慢，再运行 git status 看看：

```
$ vim CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

怎么回事？ 现在 CONTRIBUTING.md 文件同时出现在暂存区和非暂存区。 这怎么可能呢？ 好吧，实际上 Git 只不过暂存了你运行 `git add` 命令时的版本， 如果你现在提交，CONTRIBUTING.md 的版本是你最后一次运行 `git add` 命令时的那个版本，而不是你运行 `git commit` 时，在工作目录中的当前版本。 所以，运行了 `git add` 之后又作了修订的文件，需要重新运行 `git add` 把最新版本重新暂存起来：

```
$ git add CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```

## 提交更新
现在的暂存区域已经准备妥当可以提交了。 在此之前，请一定要确认还有什么修改过的或新建的文件还没有 `git add` 过，否则提交的时候不会记录这些还没暂存起来的变化。 这些修改过的文件只保留在本地磁盘。 所以，每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`：

$ git commit
这种方式会启动文本编辑器以便输入本次提交的说明。 (默认会启用 shell 的环境变量 $EDITOR 所指定的软件，一般都是 vim 或 emacs。当然也可以按照 起步 介绍的方式，使用 git config --global core.editor 命令设定你喜欢的编辑软件。）

编辑器会显示类似下面的文本信息（本例选用 Vim 的屏显方式展示）：

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#	new file:   README
#	modified:   CONTRIBUTING.md
#
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

可以看到，默认的提交消息包含最后一次运行 git status 的输出，放在注释行里，另外开头还有一空行，供你输入提交说明。 你完全可以去掉这些注释行，不过留着也没关系，多少能帮你回想起这次更新的内容有哪些。 (如果想要更详细的对修改了哪些内容的提示，可以用 -v 选项，这会将你所做的改变的 diff 输出放到编辑器中从而使你知道本次提交具体做了哪些修改。） 退出编辑器时，Git 会丢掉注释行，用你输入提交附带信息生成一次提交。

另外，你也可以在 `commit` 命令后添加 -m 选项，将提交信息与命令放在同一行，如下所示：

```
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

好，现在你已经创建了第一个提交！ 可以看到，提交后它会告诉你，当前是在哪个分支（master）提交的，本次提交的完整 SHA-1 校验和是什么（463dc4f），以及在本次提交中，有多少文件修订过，多少行添加和删改过。

请记住，提交时记录的是放在暂存区域的快照。 任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。 每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。

# 最佳实践

## 最小提交原则 
[参考](https://seesparkbox.com/foundry/atomic_commits_with_git)

每一次单独检出一个的提交，项目都可以运行并完整的通过测试

1. 类似于代码的职责单一，单独比较文件更改时方便查看
2. 方便利用 `git bisect` 来 debug

# 常用命令

## `git stash`
将当前工作区文件的快照保存到一个单独的存储区域

1. 假设我正在开发一个新的需求，修改 login 时的输入验证
2. 突然产品发现了一个bug，是用户修改头像时图片不能正常保存
3. 这个时候我的工作区里修改了一些有关 login 的代码，但我并不想做一个commit（不满足最小提交原则），但如果在这之上去修改bug的话，提交时会混乱而且如果修改 login 的代码影响了登录我就不方便进行bug修复
4. 这时使用`git stash`将已跟踪过的文件的快照保存到 stash 区域，如果有未跟踪的文件（就是从未提交过的文件）那么要加上`-u`参数
5. 这个时候工作区就完全回到了上一次提交的状态，这时再进行bug的修改，然后提交代码
6. bug修复了，要回到 stash 之前的状态 用`git stash pop`，把之前弄到一半的代码从 stash 区域里移动出来。这个时候修改的那些文件就完全回到了上一次 stash 时的状态

## alias
如果经常使用命令行的话，建议使用 alias 来快速键入命令（否则可以略过）

```
$ git config alias.co checkout --global
```
之后就可以直接使用 `git co ` 来快速使用 `git checkout`

```
$ git co dev
$ git checkout dev
```

参考配置
```
$ git config alias.co checkout --global
$ git config alias.ci commit --global
$ git config alias.st status --global
$ git config alias.sh stash --global
$ git config alias.br branch --global
```

## `git bisect` 
当确定了一个有bug和没有bug的提交区间后，会采用二分查找来定位出现bug的提交，从而快速定位错误代码

1. `git bisec start` 来开始定位
2. `git checkout $rev`检出一次提交，验证是否还存在bug($rev 代表一个快照，不论是hash值还是类似 HEAD 这种引用)
3. `git bisec good|bad` 来标记当前提交是否出现了bug
4. 当标记出了一个无bug的父提交和有bug的子提交后
5. git 就会自动二分查找中间的提交，并让你使用 `git bisec good|bad` 来标记给出的提交是否有问题
6. 当定位成功时，会返回出现bug的那个提交的相应信息