## Git Manual

> Git并不是唯一用来管理版本的工具，只是目前比较好用而且比较适合国内很多企业，因为其采用分布式管理，但也不是说分布式就是最好，因为国内90%的公司都用Git，那你用还是不用呢？
>
> Tip：Google就没有采用分布式工具管理代码
>
> 记录工作中有用的git操作指令，为了方便恢复上下文（主要因为English不太行：），这个手册更全面<https://mirrors.edge.kernel.org/pub/software/scm/git/docs/user-manual.html>
>

&nbsp;

### 开发模式

---

**Git flow**

项目存在两个长期分支：主分支master、开发分支develop

优点是清晰可控，缺点是相对复杂，需要同时维护两个长期分支

**Github flow**

只有一个长期分支master

优点简单

**Gitlab flow**

只存在一个主分支master，它是所有其他分支的"上游"，只有上游分支采纳的代码变化，才能应用到其他分支

&nbsp;

### 新建工程

---

**创建一个新仓库**

````
git clone ssh://git@10.2.29.14:8085/zj_35b150/kernel_4.19.git
cd kernel_4.19
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
````

**推送现有文件夹**

```
cd existing_folder
git init
git remote add origin ssh://git@10.2.29.14:8085/zj_35b150/kernel_4.19.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

**推送现有的 Git 仓库**

```
cd existing_repo
git remote rename origin old-origin
git remote add origin ssh://git@10.2.29.14:8085/zj_35b150/kernel_4.19.git
git push -u origin --all
git push -u origin --tags
```

**远程库**

使用自定义的名称（origin可改变）在本地仓库中添加远程库，不同的远程库代表不同的开发方向，可以实现一份代码可以查看不同的方向的代码

```
git remote add origin git@gitlab.com:emmajane/my-git-for-teams.git
```

列出连接至你当前仓库的远程仓库

```
git remote -v 
```

删除远程库

```
git remote remove origin
```

&nbsp;

### 上传

---

**commit**

commit并编写声明

```
git commit -s "xxx"
git commit --amend
```

查看commit具体内容

```
git log -p
```

**push**

上传之前要先配置好Git的配置username和email

```
git push <远程主机名> <本地分支名>:<远程分支名>
```

-u参数可以在推送的同时，将origin 仓库的master 分支设置为本地仓库当前分支的upstream（上游）。添加了这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从origin的master 分支获取内容，省去了另外添加参数的麻烦。

```
git push -u origin master
```

在上传本地分支时设置上游分支

```
git push --set-upstream origin remote_branch
```


&nbsp;
### 下载

---

**pull**

pull是两个无关步骤的组合：fetch和merge，或者fetch和rebase。
pull命令默认使用merge策略来更新本地分支，但是，通过添加--rebase参数，开发者可以选择通过rebase策略来更新自己的本地分支

```
git pull <远程主机名> <远程分支名>:<本地分支名>
git pull --rebase origin master
```

指定master分支追踪origin/next分支，如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名

```
git branch --track/--set-upstream-to master origin/next
```

**fetch**

将某个远程主机的更新，全部取回本地

```
git fetch <远程主机名> <远程分支名>:<本地分支名>
```

远程指定的分支与本地指定的分支相同，则可直接执行

```
git fetch origin remote_branch
```

下载远程分支到本地master分支

```
git fetch origin master:master
```

更新远端所有分支变更到本地

```
git fetch origin	
```

**rebase**

不能在一个共享的分支上进行Git rebase操作

git rebase优点是无须新增提交记录到目标分支，rebase后可以将对象分支的提交历史续上目标分支上，形成线性提交历史记录（变基），进行review的时候更加直观

融合代码到个人分支的时候使用`git rebase`，可以不污染分支的提交记录，形成简洁的线性提交历史记录

将多个commit合并

```
git rebase -i origin/master
```

**merge**

git merge优点是分支代码合并后不破坏原分支的代码提交记录，缺点就是会产生额外的提交记录并进行两条分支的合并

融合代码到公共分支的时使用`git merge`，而不用`git rebase`


&nbsp;
### 分支

---

**branch**

```
git branch -a						列出所有分支和远程分支
git branch							查看分支
git branch name						创建分支
git branch –d name					删除分支
git rebase master					合并master分支到本分支
git rebase --continue				解决冲突后合并
git merge name						合并某分支到当前分支
git checkout name					切换分支
git checkout –b name				创建+切换分支	
```

指定了origin/feature-D，就是说以名为origin 的仓库的feature-D分支为来源，在本地仓库中创建feature 分支

```
git checkout -b feature origin/feature-D
```


&nbsp;
### 回退&撤销

---

**reset**

```
git checkout -- <file>					丢弃工作区的修改
git reflog								查看销毁的版本号
git reset --hard 版本号				  恢复相应版本
git reset --hard HEAD~n					回到前n个版本
git reset --hard HEAD^					回退到上一个版本
git reset HEAD 							如果后面什么都不跟的话 就是上一次add里面的全部撤销了
git revert HEAD							撤销已经推送到remote仓库的提交
git reset HEAD XXX.py 					就是对某个py文件进行撤销了
```

不删除工作空间改动代码，撤销commit，并且撤销git add，这个为默认参数，git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

```
git reset --mixed  HEAD^
```

不删除工作空间改动代码，撤销commit，不撤销git add 

```
git reset --soft  HEAD^
```

删除工作空间改动代码，注意完成这个操作后，就恢复到了上一次的commit状态。

```
git reset --hard  HEAD^
```


&nbsp;
### 临时分支处理

---

**stash**

```
git stash							储藏分支
git stash apply						恢复储藏内容，但不删除stash内容
git stash list						查看储藏内容
git stash drop						删除stash内容
```


&nbsp;
### patch处理

---

**format-patch**

从某个commit开始为之后所有的commit都生成patch（不包括当前xxx）

```
git format-patch xxx
```

从某次提交之前的几次提交生成patch（包括当前xxx），-1即为当前commit

```
git format-patch –n xxx
git format-patch –1 xxx
```

某两次提交之间的所有commit生成patch，--stdout选项，可指定输出位置，-o指定patch的存放目录

```
git format-patch 365a..4e16 
```

**apply**

```
git apply  xxx.patch				打上patch
git apply --stat xxx.patch   　 	 	查看patch的情况
git apply --check xxx.patch   　		检查patch是否能够打上，如果没有任何输出，则说明无冲突，可以打上
```

**am**

```
git am xxx.patch					将名字为xxx.patch的patch打上
git am --signoff xxx.patch      	添加-s或者--signoff，还可以把自己的名字添加为signed off by信息，作用是注明打patch的人是谁
git am *.patch　　　　　       		 批量打上patch
git am --abort                  	撤回打的所有patch 
```

git am 和 git apply，两者之间有什么区别呢？

两者之间的区别在于commit信息，am只要打补丁成功，必定包括自带的commit的信息，当然你可以-s加上你本

人的singed off信息。而apply相当于修改了文件，并且执行了git add -u *之类的指令，但是不会生成commit信

息，这个信息需要你本人自己添加，也就是说，该补丁最后生成的commit信息是自己需要构造的。

**合并patch流程**

1、git am冲突后，可以先执行git apply --reject将不冲突的代码打入文件中。

2、按照xxx.rej的指示，手动修改冲突文件内容，修改完成后，删除xxx.rej文件。

3、执行git add .指令，将修改后的文件add进去。

4、执行git am --resolved即可将有冲突的补丁打成功，若此过程还是失败，再次执行1~4步骤即可。


&nbsp;
### log信息

---

**log**

```
git log master..feature							包含了feature分支中所有不在master分支上的commit
git log --oneline/--pretty=oneline 				以精简模式显示
git log --graph 								以图形模式显示
git log --stat 									显示文件更改列表
git log -p 										按补丁显示每个更新间的差异，比stat命令信息更全
git log -p filepath 							按补丁显示filepatch文件每个更新间的差异
git log --stat commitId/git show --stat commitId 		查看某一次提交的文件修改列表
git log --name-status							显示新增、修改和删除的文件清单

git log --merges/--no-merges 					查看所有的合并/非合并提交
git log --since/--after="date" 					某个日期之后
git log --until/--before="date" 				某个日期之前
git log -i --grep="xxx" 						在log信息中查找xxx，-i忽略大小写
git log --author='name' 						显示某个作者的commit
git log branch --								某个分支上的log
git log v1.0..									v1.0标签之后的commit

git log -- foo.py bar.py						--参数是声明后面传入的参数是文件路径，而不是分支名
git log -L :FuncName:FilePath					查看某个函数修改记录
git log -L start,end:filepath 					查看某个文件某几行范围内的修改记录
git log -S "Hello, World!" 						查看Hello, World!字符串的修改记录
```


&nbsp;
### 比较

---

**diff**

```
git diff filepath 								工作区与暂存区比较
git diff HEAD filepath 							工作区与HEAD (当前工作分支) 比较
git diff --cached								暂存区与最新本地版本库
git diff commit-id commit-id					两个commit比较
git diff branch1 branch2 --stat					两个分支所有差异，--stat只显示文件
git diff branch1..branch2						branch1比branch2多提交的内容
git log dev ^master								dev中有而master中没有的commit
```


&nbsp;
### 清除文件

---

**clean（未加入git的文件）**

清除多余文件

```
git clean -n
```

**rm（已加入git的文件）**

从暂存区删除，但不删除文件

```
git rm --cached file	
```

从暂存区删除，同时删除文件

```
git rm -f file
```

