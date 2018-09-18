# git文档

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

