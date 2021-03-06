# git文档

![545446-20171223173946084-1909688870](C:\Users\zsq\Desktop\git文档\images\545446-20171223173946084-1909688870.gif)

### git基础及命令行使用

> touch Readme

在目录下生成一个readme文件，不然之后执行git相关指令会报错。

打开git bash进入自己的目录，执行

	> git init

会在当前目录下生成一个名为**.git**的隐藏文件夹，存放相应的版本，仓库信息。

> git remote add origin http://gitlab.example.com/zhaoshenqiao/test01.git

将当前目录与gitlab对应的仓库连接

#### git add

git add --ignore-removal . ：他会监控工作区的状态树，使用它会把工作时的**所有变化**提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

git add -u ：他仅监控**已经被add的文件**（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）

git add .   git add -A : 上面两个功能的集合。

|                          | New Files | Modified Files | Deleted Files |
| ------------------------ | --------- | -------------- | ------------- |
| git add -A               | T         | T              | T             |
| git add .                | T         | T              | T             |
| git add --ignore-removal | T         | T              | F             |
| git add -u               | F         | T              | T             |

##### 过滤不需要提交的文件

编辑工作目录下的.gitignore文件，如果不存在就新建一个

```shell
#               表示此为注释,将被Git忽略
*.a             表示忽略所有 .a 结尾的文件
!lib.a          表示但lib.a除外
/TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt
 
bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
/bin:           表示忽略根目录下的bin文件
/*.c:           表示忽略cat.c，不忽略 build/cat.c
debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
**/foo:         表示忽略/foo,a/foo,a/b/foo等
a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
!/bin/run.sh    表示不忽略bin目录下的run.sh文件
*.log:          表示忽略所有 .log 文件
config.php:     表示忽略当前路径的 config.php 文件
 
/mtk/           表示过滤整个文件夹
*.zip           表示过滤所有.zip文件
/mtk/do.c       表示过滤某个具体文件
 
被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。
 
需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
!*.zip
!/mtk/one.txt
 
唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
/mtk/*
!/mtk/one.txt
 
假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！
 
----------------------------------------------------------------------------------
还有一些规则如下：
fd1/*
说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；
 
/fd1/*
说明：忽略根目录下的 /fd1/ 目录的全部内容；
 
/*
!.gitignore
!/fw/ 
/fw/*
!/fw/bin/
!/fw/sf/
说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。
```

过滤操作发生在文件从工作目录添加到暂存区有效，如果文件已经添加到暂存区，过滤不起作用，需要执行git reset 再重新add。

如果有些已经忽略的文件，想要提交，使用git add会添加失败：

```shell
The following paths are ignored by one of your .gitignore files:
b.txt
Use -f if you really want to add them.
```

需要使用git add -f强制添加



#### git status

git status用来查看当前目录所有文件的状态：

Untracked files:  未跟踪的文件，也就是在之前的库中没有的新文件。

Changes not stagged for commit:  已经在跟踪，有修改的文件，但尚未进入缓冲区。

Changes to be committed:  已经进入缓冲区的文件，尚未提交。

//todo



#### git commit

git commit 主要是将暂存区里的改动给提交到本地的版本库。每次使用git commit 命令我们都会在本地版本库生成一个40位的哈希值，这个哈希值也叫commit-id，

commit-id在版本回退的时候是非常有用的，它相当于一个快照,可以在未来的任何时候通过与git reset的组合命令回到这里.

初次使用git时，使用commit会提示

> Please tell me who you are.

需要先设置name与email

> git config user.email "you@example.com"
>
> git config user.name "Your Name"

如果打算在所有的项目中使用同样的name与email，可以使用

> git config --global user.email "you@example.com"

> git config --global user.name "Your Name"

局部配置的优先级高于全局配置

其余的git配置可以使用**git config --list**查看

**git commit -m "message"**:  这种是比较常见的用法，-m 参数表示可以直接输入后面的“message”，如果不加 -m参数，那么是不能直接输入message的，而是会调用一个编辑器一般是vim来让你输入这个message。

如果 message很长，可以用

git commit -m ‘

message1

message2

message3

’

**git commit -a -m “massage”**:  在上一个基础上加上-a，可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过git add添加到暂存区，但是未跟踪的文件是无法提交的，不建议使用。

**git commit --amend**:  追加提交，它可以在不增加一个新的commit-id的情况下将新修改的代码追加到前一次的commit-id中。



**git commit --help**

#### git push

一般格式为 **git push <远程主机名> <本地分支名>:<远程分支名>**，一般情况下，远程分支可以省略，如：

> git push origin master

上面命令表示，将本地的`master`分支推送到`origin`主机的`master`分支。如果`master`不存在，则会被新建。

如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。

>git push origin :master
>
>等同于
>git push origin --delete master

如果当前分支与远程分支存在追踪关系，则本地分支与远程分支都可以省略

第一次Push  如果clone项目用的是http或https的形式，会要求输入gitlab的用户名和密码

如果是ssh形式，会提示如下内容：

> git@gitlab.example.com: Permission denied (publickey).
> fatal: Could not read from remote repository.
>
> Please make sure you have the correct access rights
> and the repository exists.

需要在本地生成公匙，配置到gitlab上

打开git bash，输入 ssh-keygen

> Generating public/private rsa key pair.
> Enter file in which to save the key (/c/Users/zsq/.ssh/id_rsa):

输入要保存公匙的位置

> Enter passphrase (empty for no passphrase):

输入密码，该密码是你本地的密码，一般为空，之后再确认一次

> The key fingerprint is:
> SHA256:+ZevZ8FndDt3MLiz57DoFN/i15AD5SJTS2jsc8TkDBg zsq@DESKTOP-D3T7UCH
> The key's randomart image is:
> +---[RSA 2048]----+
> |        E+.+.    |
> |        . ++= .  |
> |         o +o=   |
> |         .= * + o|
> |        S .= = =o|
> |         . ooo*o=|
> |          o *o.B+|
> |         . +.== .|
> |         .o +Bo  |
> +----[SHA256]-----+

生成成功，到你生成目录下，找到一对以 `id_dsa` 或 `id_rsa` 命名的文件，其中带有`.pub`扩展名的是公匙，另一个是私匙，用记事本等工具打开`id_rsa.pub`，打开gitlab，右上角个人头像，settings，左侧导航栏SSH Keys，将`id_rsa.pub`中全部内容复制到Key中，title可以给这个公匙起个名字，点击Add key添加成功，之后就可以正常push了。





#### git log

在项目中运行git log，会看到以下格式输出

```shell
$ git log
commit e8db666a19d276150da9a5bfb4bdd2024a26533b (HEAD -> master, origin/master)
Author: zhaoshenqiao <zsqqfyun@163.com>
Date:   Tue Sep 18 15:27:09 2018 +0800

    second commit

commit 476950d6782325c3b9b4403280765de01c5f2fd8
Author: zhaoshenqiao <zsqqfyun@163.com>
Date:   Tue Sep 18 15:23:14 2018 +0800

    init program

```

commit指本次提交的commitId，Author 作者  Date提交时间，下面的部分是提交时的说明，即git commit -m "message"中的message。

#### git reset

- HEAD: 当前分支的当前提交的别名，默认指向当前分支最近的一次提交。

- 工作区（working directory）:即为当前工作目录，我们能够看到的代码

- 暂缓区（stage index）:git add将工作目录中的文件放入到暂缓区

  git add其实做了两件事：

  - 将本地文件的时间戳、长度，当前文档对象的id等信息保存到一个树形目录中去（index，即平时说的暂存区） 
  - 将本地文件的内容做快照并保存到Git 的对象库 

![291c4dc9-f58f-351e-a39f-fb070f211b32](C:\Users\zsq\Desktop\git文档\images\291c4dc9-f58f-351e-a39f-fb070f211b32.png)

- 历史记录区（history）:可以理解为git log看到的东西。

git reset --mixed：此为默认方式，不带任何参数的git reset，就是这种方式，它回退到某个版本，只保留源码，回退history和index信息

git reset --soft：回退到某个版本，只回退了history的信息
git reset  --hard：彻底回退到对应版本，三个区中的信息全部回退，慎用

git reset HEAD~0 最近一个提交

git reset HEAD~1 上一次提交

git reset HEAD~2 上一次的 上一次的提交（倒数第三次）

git reset HEAD~3 倒数 第四次的 提交

git reset commitId 回退到指定版本

> git commit --amend 
>
> 等价于
>
> git reset HEAD~1 --soft ;   git commit

#### git branch

一般用于分支的操作，比如创建分支，查看分支等等，

- git branch：不带参数：列出本地已经存在的分支，并且在当前分支的前面用"*"标记

- git branch -r：查看远程版本库分支列表

- git branch -a：查看所有分支列表，包括本地和远程

- git branch dev：创建名为dev的分支，创建分支时需要是最新的环境，创建分支但依然停留在当前分支

- git branch -d dev：删除dev分支，如果在分支中有一些未merge的提交，那么会删除分支失败，此时可以使用 git branch -D dev：强制删除dev分支，

- git branch -vv：可以查看本地分支对应的远程分支

- git branch -m oldName newName：给分支重命名

#### git checkout

checkout命令用于从历史提交（或者暂存区域）中拷贝文件到工作目录，也可用于切换分支。

**当给定某个文件名**（或者打开-p选项，或者文件名和-p选项同时打开）时，git会从指定的提交中拷贝文件到暂存区域和工作目录。比如，`git checkout HEAD~ foo.c`会将提交节点*HEAD~*(即当前提交节点的父节点)中的`foo.c`复制到工作目录并且加到暂存区域中。（如果命令中没有指定提交节点，则会从暂存区域中拷贝内容。）

注意当前分支不会发生变化(HEAD指向原处)。

![262348217421730](C:\Users\zsq\Desktop\git文档\images\262348217421730.jpg)

**当不指定文件名**，而是给出一个（本地）分支时，那么*HEAD*标识会移动到那个分支（也就是说，我们“切换”到那个分支了），然后暂存区域和工作目录中的内容会和*HEAD*对应的提交节点一致。新提交节点（下图中的a47c3）中的所有文件都会被复制（到暂存区域和工作目录中）；只存在于老的提交节点（ed489）中的文件会被删除；不属于上述两者的文件会被忽略，不受影响。

![262350274921880](C:\Users\zsq\Desktop\git文档\images\262350274921880.jpg)

　　**如果既没有指定文件名，也没有指定分支名**，而是一个标签、远程分支、SHA-1值或者是像*master~3*类似的东西，**就得到一个匿名分支，称作detached HEAD（被分离的HEAD标识）**。这样可以很方便地在历史版本之间互相切换。比如说你想要编译1.6.6.1版本的git，你可以运行`git checkout v1.6.6.1`（这是一个标签，而非分支名），编译，安装，然后切换回另一个分支，比如说`git checkout master`。然而，当提交操作涉及到“分离的HEAD”时，其行为会略有不同。

![262354500707332](C:\Users\zsq\Desktop\git文档\images\262354500707332.jpg)



#### git merge

- git merge dev：将dev分支合并到当前分支

  - 如果另一个分支是当前提交的祖父节点，那么合并命令将什么也不做。 另一种情况是如果当前提交是另一个分支的祖父节点，就导致*fast-forward*合并。指向只是简单的移动，并生成一个新的提交

  - 否则就是一次真正的合并。默认把当前提交(*ed489* 如下所示)和另一个提交(*33104*)以及他们的共同祖父节点(*b325c*)进行一次三方合并。

    结果是先保存当前目录和索引，然后和父节点*33104*一起做一次新提交。

![270006531338299](C:\Users\zsq\Desktop\git文档\images\270006531338299.jpg)

#### git remote

- git remote -v：查看现有仓库的远端仓库
- git remote add 远端仓库地址别名 <url>：添加一个新的仓库地址
- git remote rm origin：删除名为origin的远端仓库
- git remote rename orign1 origin2：重命名



#### git过滤不需要提交的文件

编辑工作目录下的.gitignore文件，如果不存在就新建一个

```shell
#               表示此为注释,将被Git忽略
*.a             表示忽略所有 .a 结尾的文件
!lib.a          表示但lib.a除外
/TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt
 
bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
/bin:           表示忽略根目录下的bin文件
/*.c:           表示忽略cat.c，不忽略 build/cat.c
debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
**/foo:         表示忽略/foo,a/foo,a/b/foo等
a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
!/bin/run.sh    表示不忽略bin目录下的run.sh文件
*.log:          表示忽略所有 .log 文件
config.php:     表示忽略当前路径的 config.php 文件
 
/mtk/           表示过滤整个文件夹
*.zip           表示过滤所有.zip文件
/mtk/do.c       表示过滤某个具体文件
 
被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。
 
需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
!*.zip
!/mtk/one.txt
 
唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
/mtk/*
!/mtk/one.txt
 
假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！
 
----------------------------------------------------------------------------------
还有一些规则如下：
fd1/*
说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；
 
/fd1/*
说明：忽略根目录下的 /fd1/ 目录的全部内容；
 
/*
!.gitignore
!/fw/ 
/fw/*
!/fw/bin/
!/fw/sf/
说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。
```

#### git工作流

![git工作流](C:\Users\zsq\Desktop\git文档\images\git工作流.png)

