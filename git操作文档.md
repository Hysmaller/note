git 常用命令整理

 git命令查询网站： https:\/\/git-scm.com\/docs\/git-commit



使用git的原则和我尚未理解的问题：

1、？git branch -a \/\/显示所有的本地分支和远程分支 在不同电脑的本地仓库，执行本命令显示的远程仓库分支是不一样的 这个为什么呢

2、删除的时候，不要使用 git rm 文件名，而是使用 git rm --cached 文件名，这样先删除缓存区里的文件，本地文件夹中的文件保留，这样不会误删文件，以后还可以找到这个文件。 但是这样存在的问题是，若不删除本地文件，切换分支会失败，即在切换分支前删除文件夹中的文件，或将其剪切到其它位置即可，然后再切换分支。

3、每次切换分支的原则是 首先 commit，再切换分支 。若之前执行了 rm --cached操作，则需要将本地文件删除或挪到其它位置再切换分支，否则会切换分支失败

4、



初始化配置

我的生效了的命令：



初始化：

①、ssh-keygen -t rsa -C "superliji163@youremail.com" \/\/在本地创建ssh key

②、回到github，进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴id\_rsa.pub文件的内容

③、ssh -T git@github.com 然后输入 yes ，若看到“You’ve successfully authenticated”，表成功脸上git

④、git config --global user.name "slowmickey"

⑤、git config --global user.email "superliji163@youremail.com"

⑥、git init

⑦、git remote add origin git@github.com:yourName\/yourRepo.git

⑧、git remote -v 查看本地文件夹 和git服务器库的绑定情况



其它：

git status \/\/显示当前工作区的状态，包括各个文件的状态，非常有用

git remote -v \/\/查看本文件夹对应的远程仓库的信息

git clone git@github.com:slowmickey\/JavaWeb.git \/\/在当前文件夹下新建一个仓库名的子文件夹，获取这个 repository 的内容到子文件夹中，在clone完成之后，Git 会自动为你将此远程仓库命名为origin（origin只相当于一个别名，运行git remote –v或者查看.git\/config可以看到origin的含义），并下载其中所有的数据，建立一个指向它的master 分支的指针，我们用\(远程仓库名\)\/\(分支名\) 这样的形式表示远程分支，所以origin\/master指向的是一个remote branch（从那个branch我们clone数据到本地）“

git merge origin\/master \/\/获取远程服务器上主分支的版本 ，并自动合并 ，不会改变本地版本库中的以修改的部分

git reset --hard \/\/将本地分支的代码更改为master分支的最新代码 ，会丢失本地所有的更改 ，但不删除本地新建的新文件， 使用 git pull 无效的时候可以使用它

git checkout readme.txt \/\/撤销对readme.txt文件的更改，使其与获取到的时候保持一致

git add rename.txt ; git commit -m "T440-WIN10 first commit" ; git push origin master ;

\/\/将本地库的rename.txt文件上传到github中 ，git上 pull requests里查不到记录，直接就提交到code里了 commit里可以看到提交记录

git add -A ; git commit -m "T440-WIN10 second commit" ; git push origin master ;

\/\/将本地库中所有的远程库中没有的文件\(排除.gitignore文件中指定的文件夹和文件\)上传到远程库中

git rm 1.txt ; git commit -m "T440-WIN10 second commit" ; git push origin master ;

\/\/删除本地库中的 1.txt，并提交到远程库，删除库中的1.txt，同时删除本地文件夹中的 1.txt文件

git rm --cached 1.txt ; git commit -m "T440-WIN10 second commit" ; git push origin master ;

\/\/删除本地库中的 1.txt，并提交到远程库，删除库中的1.txt，不删除本地文件夹中的 1.txt文件，但其为加入到本地版本库中 \/\/使用本方式来删除比较好

git reset --hard ;\# 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git push origin master

git pull origin master

git commit -m "注释" \/\/若有更改，

git commit -am "注释"



git branch \/\/查看所有的本地分支，及当前处于哪个分支

git branch -a \/\/显示所有的本地分支和远程分支 在不同电脑的本地仓库，执行本命令显示的远程仓库分支是不一样的 ？？？ 这个为什么呢

git branch a \/\/新建本地分支a，当前分支不变

git checkout a \/\/当前分支切换到分支a

git checkout -b b \/\/新建本地分支b，当前分支切换到分支b

git branch -d a \/\/删除分支a ，若a分支的代码未合并到本地master分支，则删除失败

git branch -D a \/\/强制删除分支a，不管其是否合并到本地的master分支



一种分支策略： master分支用于发布版本、dev分支用来干活、feature分支用于新功能开发

git checkout -b dev \# 创建本地dev分支用于开发

git checkout -b bug \# 创建本地bug分支用于bug处理

git checkout -b feature \# 创建本地feature分支用于新功能开发

git push origin dev \# 生成远程dev分支

git push origin bug \# 生成远程bug分支

git push origin feature \# 生成远程feature分支



git remote add b https:\/\/github.com\/slowmickey\/JavaWeb.git \/\/新建一个名字为b的远程分支，需要push后才生效

git push -u b b \/\/将本地分支b的内容push到远程分支b中 ，这里会弹框让输入git用户名密码



一个master分支和test分支都修改 1.txt文件，然后合并的例子 -- 非注释的部分是提倡使用的流程，每次都是先commit再切换分支

 \/\/开始时位于master分支,在一个本地分支上做了修改之后，必须执行 commit，再切换到另一个分支，否则会，现象举例如下：

 \/\/例 ：若在test分支上对1.txt做了修改，但是不commit直接切换到master分支，则在master分支也可以看到test中刚刚对1.txt的修改，然后这时在master中执行 commit，也可以提交，这时再切换到test分支，会发现刚刚在本分支中对1.txt所做的修改不见了。 -- 本现象对 add 一样有效 rm稍有不同

 \/\/ 若在test分支下执行 rm --cached 1.txt，然后commit，这个时候不手动删除文件夹中的1.txt文件，执行切换到master分支的操作，会提示切换失败，不允许切换，手动删除1.txt文件后就可以切换了

①、git checkout -b test \/\/创建并切换到b分支

②、修改1.txt文件 \/\/在test分支上

③、git commit -am "test modify" \/\/在test分支上提交修改 这必须-am，若是-m会报错

④、git checkout master \/\/切换到master分支

⑤、修改1.txt文件 \/\/在master分支上

⑥、git commit -am "master modify" \/\/在master分支上

⑦、git merge test \/\/在master分支上，执行完变 master\|merging

⑧、编辑1.txt文件 \/\/在 master\|merging 状态下

⑨、git add 1.txt \/\/在 master\|merging 状态下

⑩、git commit -m "" \/\/在 master\|merging 状态下，执行完变master状态



reset指令详解： git reset \[--hard\|soft\|mixed\|merge\|keep\] \[&lt;commit&gt;或HEAD\] :将当前的分支重设（reset）到指定的&lt;commit&gt;或者HEAD（默认，如果不显示指定commit，默认是HEAD，即最新的一次提交）

其中：

commit：可以使用git log 查询commit名

hard:重设working directory、index、head指向 soft:只改head指向 mixed&lt;缺省&gt;:仅改 index merge:用得不多 keep:用得不多



使用git log 和 git reset 恢复之前的版本， \/\/在master分支上

①、git log \/\/查看每次commit的版本号，然后鼠标右键复制想恢复的版本号

②、git reset --hard 版本号\/head~n \/\/文件夹、缓冲区、本地仓库全都恢复到这个版本号对应的状态 ，若使用head~n，代表回退n个版本。 当前版本为回退0个版本



使用git checkout 文件名 和 git checkout head 文件名 从缓冲区 和 本地库中恢复文件

①、git checkout 文件名 ：从缓冲区中恢复文件

②、git checkout head 文件名 ： 从本地库中恢复head所指向的版本





使用git revert撤销某次操作 ，此次操作之前和之后的commit和history都会保留，并且把这次撤销作为一次最新的提交

需要注意的是，有时候执行revert之后，需要执行文件合并 ，使用 revert 比使用 reset 更加安全

①、 git revert HEAD 撤销前一次 commit

②、 git revert HEAD^ 撤销前前一次 commit

③、 git revert 提交号 （比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）撤销指定的版本，撤销也会作



git push指令详解：

①、git push &lt;远程主机名&gt; &lt;本地分支名&gt;:&lt;远程分支名&gt; \/\/如 git push origin master:master



git pull指令详解：



git clone指令详解：

①、git clone 版本库的网址 \[本地目录名\] \/\/如 git clone https:\/\/github.com\/jquery\/jquery.git

 \/\/版本库的网址，除支持 http外，还支持 ssh、git、file 等。

②、$ git clone -o jQuery https:\/\/github.com\/jquery\/jquery.git 克隆并指定远程主机名为jQuery



git remote指令详解： 为了便于管理，Git要求每个远程主机都必须指定一个主机名。 git 一个本地仓库可以对应多个远端服务器，有的可能只有读权限，有的可能有读写权限

 克隆版本库的时候，所使用的远程主机自动被Git命名为origin。如果想用其他的主机名，需要用git clone命令的-o选项指定。

①、git remote ：列出所有远程主机。

②、git remote -v ：列出所有远程主机的名字、网址及读写权限

③、git remote show 主机名 ：可以查看主机的详细信息 ，包括Url及其各分支的分支名

④、git remote add 主机名 网址 ： 添加一个远端服务器 -- 一个本地仓库可以对应多个远端服务器

⑤、git remote rm 主机名 ： 删除本地仓库和远端服务器的对应关系

⑥、git remote rename 原主机名 新主机名 ： 更改远程主机的名字

