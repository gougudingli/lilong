### 一、上堂回顾

> 1.安装git
>
> ​	sudo apt-get install git
>
> 2.创建本地版本库
>
> ​	a.定位到指定的目录下	cd  xxx
>
> ​	b.创建一个空目录   mkdir xxx
>
> ​	c.定位到空目录下，使用git init初始化仓库
>
> 3.将工作区中的文件添加到版本库
>
> ​	工作区：开发
>
> ​	暂存区：git add
>
> ​	版本库：git commit
>
> 4.版本回退
>
> ​	查看仓库的状态：git status
>
> ​	查看仓库和版本库具体内容的不同：git  diff
>
> ​	查看提交的日志记录：git log   或者  git   log    --pretty=oneline
>
> ​	回退到上一个版本：git reset --hard  HEAD^
>
> ​					   git reset --hard commitId
>
> ​	查看git中执行过的命令：git reflog
>
> ​	撤销修改：
>
> ​		a.git checkout --  xxx   :丢弃工作区的修改
>
> ​		b.git reset HEAD xxxx  :撤销暂存区的修改
>
> ​		    git checkout --  xxx   :丢弃工作区的修改
>
> ​		c.git reset --hard  HEAD^  :回退到上一个版本
>
> 5.删除文件
>
> ​	rm -rf xxxx:删除工作区中的文件
>
> ​	git rm xxx:删除版本库中的文件
>
> ​	git commit -m "xxx":将修改提交到版本库
>
> 6.远程仓库
>
> ​	a.建立本机和github之间的连接	 ssh key
>
> ​	b.将本地仓库上传到远程仓库
>
> ​		git remote add origin git@github.com:用户名/远程仓库名.git
>
> ​		git push -u origin master
>
> ​		git add/git      commit/git       push origin master
>
> ​	c.将远程仓库克隆到本地
>
> ​		git clone  git@github.com:用户名/远程仓库名.git
>
> ​	d.分支管理
>
> ​		创建子分支并切换到子分支下面;git checkout -b xxxx
>
> ​		查看当前的分支：git branch
>
> ​		切换分支：git branch
>
> ​		将子分支合并到主分支： 
>
> ​				切换到主分支：git checkout master
>
> ​				合并：git merge xxx
>
> ​		删除子分支：git branch -d xxxx

### 二、git使用

#### 1.分支管理

##### 1.1冲突解决

> ```Python
> #1.当git无法自动合并分支时，就必须首先手动解决冲突，解决冲突后，再提交，合并自动完成
> #2.解决冲突就是把git合并失败的代码手动编辑为我们希望成为的内容，再提交
> #3.使用git log --graph查看分支合并图【一条直线表示主分支，如果在哪个分支上进行开发，则线条是绿色的】
>
> 演示命令：
> lichongchong@ubuntu01:~$ cd Desktop/
> lichongchong@ubuntu01:~/Desktop$ rm -rf check/
> lichongchong@ubuntu01:~/Desktop$ git clone git@github.com:yangyang-git/check.git
> 正克隆到 'check'...
> remote: Enumerating objects: 3, done.
> remote: Counting objects: 100% (3/3), done.
> remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
> 接收对象中: 100% (3/3), 完成.
> lichongchong@ubuntu01:~/Desktop$ cd check/
> lichongchong@ubuntu01:~/Desktop/check$ git checkout -b dev
> 切换到一个新分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ git branch
> * dev
>   master
> lichongchong@ubuntu01:~/Desktop/check$ touch file1.txt
> lichongchong@ubuntu01:~/Desktop/check$ vim file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git add file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "nfjng"
> [dev 18b1756] nfjng
>  1 file changed, 1 insertion(+)
>  create mode 100644 file1.txt
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支与上游分支 'origin/master' 一致。
> lichongchong@ubuntu01:~/Desktop/check$ git branch
>   dev
> * master
> lichongchong@ubuntu01:~/Desktop/check$ git merge dev
> 更新 4705b95..18b1756
> Fast-forward
>  file1.txt | 1 +
>  1 file changed, 1 insertion(+)
>  create mode 100644 file1.txt
> lichongchong@ubuntu01:~/Desktop/check$ ls
> file1.txt  README.md
> lichongchong@ubuntu01:~/Desktop/check$ git branch -d dev
> 已删除分支 dev（曾为 18b1756）。
> lichongchong@ubuntu01:~/Desktop/check$ git checkout -b feature1
> 切换到一个新分支 'feature1'
> lichongchong@ubuntu01:~/Desktop/check$ vim README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git add README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git commit  -m "feature1"
> [feature1 6cd0eab] feature1
>  1 file changed, 2 insertions(+), 1 deletion(-)
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支领先 'origin/master' 共 1 个提交。
>   （使用 "git push" 来发布您的本地提交）
> lichongchong@ubuntu01:~/Desktop/check$ cat README.md 
> # checklichongchong@ubuntu01:~/Desktop/check$ vim README.md 
> lichongchong@ubuntu01:~/Desktop/check$ it add README.md 
> it：未找到命令
> lichongchong@ubuntu01:~/Desktop/check$ git add README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "master"
> [master b6cbc38] master
>  1 file changed, 2 insertions(+), 1 deletion(-)
> lichongchong@ubuntu01:~/Desktop/check$ git merge feature1 
> 自动合并 README.md
> 冲突（内容）：合并冲突于 README.md
> 自动合并失败，修正冲突然后提交修正的结果。
> lichongchong@ubuntu01:~/Desktop/check$ vim README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git add README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git commit  -m "conflict"
> [master e41ad67] conflict
> lichongchong@ubuntu01:~/Desktop/check$ git status
> 位于分支 master
> 您的分支领先 'origin/master' 共 4 个提交。
>   （使用 "git push" 来发布您的本地提交）
>
> 无文件要提交，干净的工作区
> lichongchong@ubuntu01:~/Desktop/check$ git log --graph
> *   commit e41ad676873d8675e042e9a5548f632e6b305c44 (HEAD -> master)
> |\  Merge: b6cbc38 6cd0eab
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:41:04 2018 +0800
> | | 
> | |     conflict
> | | 
> | * commit 6cd0eabe6cf94e02255af5582f54368e723d47f6 (feature1)
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:32:57 2018 +0800
> | | 
> | |     feature1
> | | 
> * | commit b6cbc387637c4760f20fbc65499ea372bc983be7
> |/  Author: yangyang-git <18501970795@163.com>
> |   Date:   Fri Dec 21 09:35:04 2018 +0800
> |   
> |       master
> | 
> * commit 18b1756ce74e2fb9d0335e3b1861fe6b60497433
> | Author: yangyang-git <18501970795@163.com>
> | Date:   Fri Dec 21 09:28:57 2018 +0800
> | 
> |     nfjng
> | 
> lichongchong@ubuntu01:~/Desktop/check$ git branch -d feature1 
> 已删除分支 feature1（曾为 6cd0eab）。
> lichongchong@ubuntu01:~/Desktop/check$ git branch
> * master
> lichongchong@ubuntu01:~/Desktop/check$ git log --graph
> *   commit e41ad676873d8675e042e9a5548f632e6b305c44 (HEAD -> master)
> |\  Merge: b6cbc38 6cd0eab
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:41:04 2018 +0800
> | | 
> | |     conflict
> | | 
> | * commit 6cd0eabe6cf94e02255af5582f54368e723d47f6
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:32:57 2018 +0800
> | | 
> | |     feature1
> | | 
> * | commit b6cbc387637c4760f20fbc65499ea372bc983be7
> |/  Author: yangyang-git <18501970795@163.com>
> |   Date:   Fri Dec 21 09:35:04 2018 +0800
> |   
> |       master
> | 
> * commit 18b1756ce74e2fb9d0335e3b1861fe6b60497433
> | Author: yangyang-git <18501970795@163.com>
> | Date:   Fri Dec 21 09:28:57 2018 +0800
> | 
> |     nfjng
> | 
> ```

##### 1.2.分支管理策略

> master：当需要发布版本的时候，将dev合并到master上，非常稳定
>
> ​	dev：每天或者每周或者每个功能，将num1和num2合并到dev上
>
> ​		num1:开发的分支
>
> ​		num2：开发的分支
>
> 合并分支：使用git merge xxx合并分支，默认使用fast-forward模式，在这种模式下，删除分支后，会直接丢掉分支信息
>
> 尽量强制禁用fast-forward模式，git就会在merge的时候生成一个新的commit，这样，分支信息就不会被丢失掉了
>
> ```Python
> git merge --no-ff -m "123" dev
> #--no-ff：禁用fast-forward模式，保留分支的信息
> #-m "123" 合并子分支的时候，产生一个提交的节点
>
> #注意问题：合并分支实际上合并的是内容，对分支本身没有影响，删除其中的一个分支，其他的分支可以正常的工作
>
> 演示命令：
> lichongchong@ubuntu01:~/Desktop/check$ 
> lichongchong@ubuntu01:~/Desktop/check$ git checkout -b dev
> 切换到一个新分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ vim README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git add README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "gsef"
> [dev 98c6295] gsef
>  1 file changed, 1 insertion(+), 1 deletion(-)
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master 
> 切换到分支 'master'
> 您的分支领先 'origin/master' 共 4 个提交。
>   （使用 "git push" 来发布您的本地提交）
> lichongchong@ubuntu01:~/Desktop/check$ git merge --no-ff -m "123" dev
> Merge made by the 'recursive' strategy.
>  README.md | 2 +-
>  1 file changed, 1 insertion(+), 1 deletion(-)
> lichongchong@ubuntu01:~/Desktop/check$ git log --graph
> *   commit 0f61c88c7d4f9e17a7684663fc592ac91670a88a (HEAD -> master)
> |\  Merge: e41ad67 98c6295
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:58:33 2018 +0800
> | | 
> | |     123
> | | 
> | * commit 98c62954d744da085fec7df063009ed6a22bd830 (dev)
> |/  Author: yangyang-git <18501970795@163.com>
> |   Date:   Fri Dec 21 09:57:07 2018 +0800
> |   
> |       gsef
> |   
> *   commit e41ad676873d8675e042e9a5548f632e6b305c44
> |\  Merge: b6cbc38 6cd0eab
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:41:04 2018 +0800
> | | 
> | |     conflict
> | | 
> | * commit 6cd0eabe6cf94e02255af5582f54368e723d47f6
> | | Author: yangyang-git <18501970795@163.com>
> | | Date:   Fri Dec 21 09:32:57 2018 +0800
> | | 
> | |     feature1
> lichongchong@ubuntu01:~/Desktop/check$ git branch
>   dev
> * master
> lichongchong@ubuntu01:~/Desktop/check$ git checkout dev
> 切换到分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ git branch
> * dev
>   master
> lichongchong@ubuntu01:~/Desktop/check$ git checkout -b num1
> 切换到一个新分支 'num1'
> lichongchong@ubuntu01:~/Desktop/check$ git checkout -b num2
> 切换到一个新分支 'num2'
> lichongchong@ubuntu01:~/Desktop/check$ git branch
>   dev
>   master
>   num1
> * num2
> lichongchong@ubuntu01:~/Desktop/check$ git merge master
> 更新 98c6295..0f61c88
> Fast-forward
> lichongchong@ubuntu01:~/Desktop/check$ git branch
>   dev
>   master
>   num1
> * num2
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支领先 'origin/master' 共 6 个提交。
>   （使用 "git push" 来发布您的本地提交）
> lichongchong@ubuntu01:~/Desktop/check$ git merge dev
> 已经是最新的。
> lichongchong@ubuntu01:~/Desktop/check$ git branch -d dev
> 已删除分支 dev（曾为 98c6295）。
> lichongchong@ubuntu01:~/Desktop/check$ git branch
> * master
>   num1
>   num2
> lichongchong@ubuntu01:~/Desktop/check$ git checkout num1
> 切换到分支 'num1'
> lichongchong@ubuntu01:~/Desktop/check$ git merge num2
> 更新 98c6295..0f61c88
> Fast-forward
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支领先 'origin/master' 共 6 个提交。
>   （使用 "git push" 来发布您的本地提交）
> lichongchong@ubuntu01:~/Desktop/check$ git merge num1
> 已经是最新的。
> lichongchong@ubuntu01:~/Desktop/check$ git branch -d num1
> 已删除分支 num1（曾为 0f61c88）。
> lichongchong@ubuntu01:~/Desktop/check$ git branch
> * master
>   num2
> lichongchong@ubuntu01:~/Desktop/check$ git checkout num2
> 切换到分支 'num2'
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支领先 'origin/master' 共 6 个提交。
>   （使用 "git push" 来发布您的本地提交）
> lichongchong@ubuntu01:~/Desktop/check$ git branch -d num2
> 已删除分支 num2（曾为 0f61c88）。
> lichongchong@ubuntu01:~/Desktop/check$ 
> ```

##### 1.3.bug分支

> ```Python
> #修复bug时，我们可以通过创建新的分支进行修复，然后合并，最后删除
> #当手头工作没有完成时，先把当前的工作现场git stash储藏，然后取修复bug，修复后，回到工作现场，通过git stash pop 恢复原来的工作现场
>
> 演示命令：
> lichongchong@ubuntu01:~/Desktop/check$ git checkout -b dev
> 切换到一个新分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ vim README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git add README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git status
> 位于分支 dev
> 要提交的变更：
>   （使用 "git reset HEAD <文件>..." 以取消暂存）
>
> 	修改：     README.md
>
> lichongchong@ubuntu01:~/Desktop/check$ git stash
> 保存工作目录和索引状态 WIP on dev: 0f61c88 123
> lichongchong@ubuntu01:~/Desktop/check$ git status
> 位于分支 dev
> 无文件要提交，干净的工作区
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支领先 'origin/master' 共 6 个提交。
>   （使用 "git push" 来发布您的本地提交）
> lichongchong@ubuntu01:~/Desktop/check$ vim file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git add file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "file1~~~"
> [master cb3191a] file1~~~
>  1 file changed, 1 insertion(+)
> lichongchong@ubuntu01:~/Desktop/check$ git checkout dev
> 切换到分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ git status
> 位于分支 dev
> 无文件要提交，干净的工作区
> lichongchong@ubuntu01:~/Desktop/check$ git stash pop
> 位于分支 dev
> 尚未暂存以备提交的变更：
>   （使用 "git add <文件>..." 更新要提交的内容）
>   （使用 "git checkout -- <文件>..." 丢弃工作区的改动）
>
> 	修改：     README.md
>
> 修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
> 丢弃了 refs/stash@{0} (7ed0bd94d4d9032c6f598773abce49f8accf2950)
> lichongchong@ubuntu01:~/Desktop/check$ git stash list
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "readme"
> 位于分支 dev
> 尚未暂存以备提交的变更：
> 	修改：     README.md
>
> 修改尚未加入提交
> lichongchong@ubuntu01:~/Desktop/check$ git bransh
> git：'bransh' 不是一个 git 命令。参见 'git --help'。
>
> 最相似的命令是
> 	branch
> lichongchong@ubuntu01:~/Desktop/check$ git status
> 位于分支 dev
> 尚未暂存以备提交的变更：
>   （使用 "git add <文件>..." 更新要提交的内容）
>   （使用 "git checkout -- <文件>..." 丢弃工作区的改动）
>
> 	修改：     README.md
>
> 修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
> lichongchong@ubuntu01:~/Desktop/check$ git add README.md 
> lichongchong@ubuntu01:~/Desktop/check$ git commit  -m "fshjrg"
> [dev fc7e827] fshjrg
>  1 file changed, 1 insertion(+)
> lichongchong@ubuntu01:~/Desktop/check$ git status
> 位于分支 dev
> 无文件要提交，干净的工作区
> ```

##### 1.4.feature分支

> 在软件开发过程中，总有新的功能添加进来
>
> git建议：但凡有一个新的功能需要添加，则创建一个新的分支，专门用来开发新的功能，这种分支被称为feature分支，避免将主分支搞乱
> 当新的功能完成之后，只需要将子分支合并到主分支，最后就可以删除子分支

##### 1.5.多人协作

> 从远程仓库克隆的时候，实际上git会自动将本地的master分支和远程的master分支对应起来，远程仓库默认的名字为origin
>
> ```Python
> #1.查看远程仓库的名称
> 	git remote
> #2.查看远程仓库的详细信息
> 	git remote -v
>   
>   	"""
>   	fetch:抓取，从远程仓库-----》本地仓库
> 	push：推送，从本地仓库----》远程仓库
>   	"""
>
> 演示命令：
> lichongchong@ubuntu01:~/Desktop/check$ git remote
> origin
> lichongchong@ubuntu01:~/Desktop/check$ git remote -v
> origin	git@github.com:yangyang-git/check.git (fetch)
> origin	git@github.com:yangyang-git/check.git (push)
> ```

##### 1>推送分支

> 推送分支：就是把某个分支上的本地提交全部推送到远程仓库，推送时，要指定本地分支，git会把该分支推送到远程库对应的远程分支上
>
> 默认：
>
> ​	远程：master
>
> ​	本地：master
>
> 创建
>
> ​	本地：dev
>
> ​	远程：将dev推送到远程
>
> ```Python
> #1.推送本地主分支到远程仓库
> 	git push origin master
> #2.推送其他分支
> 	git push origin dev 
>   
>   
> """
> a.并不是一定要将所有的分支都需要推送到远程仓库
> b.master是主分支，因此要时刻和远程保持同步，需要推送
> c.dev分支时一个开发分支，团队中的所有成员都需要在上面开发，也需要和远程同步
> d.bug分支，feature分支只是为了在本地完成bug的修复或者新功能的实现，则没有必须要推送到远程
> e.不管本地仓库中有多少个分支，远程仓库中的分支一般不要超过2 + 团队人数
> """
>
> 演示命令：
> lichongchong@ubuntu01:~/Desktop/check$ git push origin master
> 枚举对象: 24, 完成.
> 对象计数中: 100% (24/24), 完成.
> 使用 2 个线程进行压缩
> 压缩对象中: 100% (16/16), 完成.
> 写入对象中: 100% (22/22), 1.88 KiB | 83.00 KiB/s, 完成.
> 总共 22 （差异 4），复用 0 （差异 0）
> remote: Resolving deltas: 100% (4/4), done.
> To github.com:yangyang-git/check.git
>    4705b95..4ccf117  master -> master
> lichongchong@ubuntu01:~/Desktop/check$ git push origin dev
> 总共 0 （差异 0），复用 0 （差异 0）
> remote: 
> remote: Create a pull request for 'dev' on GitHub by visiting:
> remote:      https://github.com/yangyang-git/check/pull/new/dev
> remote: 
> To github.com:yangyang-git/check.git
>  * [new branch]      dev -> dev
> lichongchong@ubuntu01:~/Desktop/check$ git checkout dev
> 切换到分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ vim file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git add file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git commit  -m "13"
> [dev a837a0a] 13
>  1 file changed, 1 insertion(+)
> lichongchong@ubuntu01:~/Desktop/check$ git checkout master
> 切换到分支 'master'
> 您的分支与上游分支 'origin/master' 一致。
> lichongchong@ubuntu01:~/Desktop/check$ git merge dev
> 自动合并 file1.txt
> 冲突（内容）：合并冲突于 file1.txt
> 自动合并失败，修正冲突然后提交修正的结果。
> lichongchong@ubuntu01:~/Desktop/check$ vim file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m ""
> U	file1.txt
> error: 无法提交，因为您有未合并的文件。
> 提示：请在工作区改正文件，然后酌情使用 'git add/rm <文件>' 命令标记
> 提示：解决方案并提交。
> fatal: 因为存在未解决的冲突而退出。
> lichongchong@ubuntu01:~/Desktop/check$ git add file1.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "gjrg"
> [master 90a650d] gjrg
> lichongchong@ubuntu01:~/Desktop/check$ git push origin master
> 枚举对象: 8, 完成.
> 对象计数中: 100% (8/8), 完成.
> 使用 2 个线程进行压缩
> 压缩对象中: 100% (3/3), 完成.
> 写入对象中: 100% (4/4), 397 bytes | 397.00 KiB/s, 完成.
> 总共 4 （差异 1），复用 0 （差异 0）
> remote: Resolving deltas: 100% (1/1), done.
> To github.com:yangyang-git/check.git
>    4ccf117..90a650d  master -> master
> lichongchong@ubuntu01:~/Desktop/check$ git push origin dev
> 总共 0 （差异 0），复用 0 （差异 0）
> To github.com:yangyang-git/check.git
>    fc7e827..a837a0a  dev -> dev
> ```

##### 2>抓取分支

> 多人协作时，大家都会往master或者dev上推送各自的修改
>
> 注意：演示团队开发，使用一个github账号，然后将两个电脑上的ssh key添加到该github账号下，其中的一个人在github中创建一个远程仓库，两个人都在各自的本地将远程仓库克隆
>
> ```Python
> #注意：先pull，再push，主要是为了解决冲突
>
> """
> 步骤：假设两个人开发
> a.两个人先克隆远程仓库
> b.各自进行各自的开发，A将自己的修改推送到远程,B首先将远程仓库中的内容拉取下来【尽量将冲突出现在本地，不要出现在远程仓库】
> c.如果有冲突，则解决冲突，解决完成之后，将所有的内容push
> d.如果要再次进行各自的修改，只要pull
> """
>
> 演示命令：
> lichongchong@ubuntu01:~/Desktop/check$ git branch
>   dev
> * master
> lichongchong@ubuntu01:~/Desktop/check$ cd ..
> lichongchong@ubuntu01:~/Desktop$ mkdir text
> lichongchong@ubuntu01:~/Desktop$ cd text/
> lichongchong@ubuntu01:~/Desktop/text$ git clone git@github.com:yangyang-git/check.git
> 正克隆到 'check'...
> remote: Enumerating objects: 29, done.
> remote: Counting objects: 100% (29/29), done.
> remote: Compressing objects: 100% (15/15), done.
> remote: Total 29 (delta 5), reused 26 (delta 5), pack-reused 0
> 接收对象中: 100% (29/29), 完成.
> 处理 delta 中: 100% (5/5), 完成.
> lichongchong@ubuntu01:~/Desktop/text$ cd check/
> lichongchong@ubuntu01:~/Desktop/text/check$ git branch
> * master
> lichongchong@ubuntu01:~/Desktop/text/check$ git checkout -b dev origin/dev
> 分支 'dev' 设置为跟踪来自 'origin' 的远程分支 'dev'。
> 切换到一个新分支 'dev'
> lichongchong@ubuntu01:~/Desktop/text/check$ git branch
> * dev
>   master
> lichongchong@ubuntu01:~/Desktop/text/check$ touch a.txt
> lichongchong@ubuntu01:~/Desktop/text/check$ vim a.txt 
> lichongchong@ubuntu01:~/Desktop/text/check$ git add a.txt 
> lichongchong@ubuntu01:~/Desktop/text/check$ git commit -m "a"
> [dev 5e9193b] a
>  1 file changed, 1 insertion(+)
>  create mode 100644 a.txt
> lichongchong@ubuntu01:~/Desktop/text/check$ git push origin dev
> 枚举对象: 4, 完成.
> 对象计数中: 100% (4/4), 完成.
> 使用 2 个线程进行压缩
> 压缩对象中: 100% (2/2), 完成.
> 写入对象中: 100% (3/3), 301 bytes | 301.00 KiB/s, 完成.
> 总共 3 （差异 0），复用 0 （差异 0）
> To github.com:yangyang-git/check.git
>    a837a0a..5e9193b  dev -> dev
> lichongchong@ubuntu01:~/Desktop/text/check$ cd ..
> lichongchong@ubuntu01:~/Desktop/text$ cd ..
> lichongchong@ubuntu01:~/Desktop$ cd check/
> lichongchong@ubuntu01:~/Desktop/check$ ls
> file1.txt  README.md
> lichongchong@ubuntu01:~/Desktop/check$ git branch
>   dev
> * master
> lichongchong@ubuntu01:~/Desktop/check$ git checkout dev
> 切换到分支 'dev'
> lichongchong@ubuntu01:~/Desktop/check$ git pull
> remote: Enumerating objects: 4, done.
> remote: Counting objects: 100% (4/4), done.
> remote: Compressing objects: 100% (2/2), done.
> remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
> 展开对象中: 100% (3/3), 完成.
> 来自 github.com:yangyang-git/check
>    a837a0a..5e9193b  dev        -> origin/dev
> 当前分支没有跟踪信息。
> 请指定您要合并哪一个分支。
> 详见 git-pull(1)。
>
>     git pull <远程> <分支>
>
> 如果您想要为此分支创建跟踪信息，您可以执行： #拉取失败，根据提示操作，原因是没有指定本地dev分支与远程origin/dev分支的链接
>
>     git branch --set-upstream-to=origin/<分支> dev
>
> lichongchong@ubuntu01:~/Desktop/check$ git branch --set-upstream-to=origin/dev dev
> 分支 'dev' 设置为跟踪来自 'origin' 的远程分支 'dev'。
> lichongchong@ubuntu01:~/Desktop/check$ git pull
> 更新 a837a0a..5e9193b
> Fast-forward
>  a.txt | 1 +
>  1 file changed, 1 insertion(+)
>  create mode 100644 a.txt
> lichongchong@ubuntu01:~/Desktop/check$ ls
> a.txt  file1.txt  README.md
> lichongchong@ubuntu01:~/Desktop/check$ git branch
> * dev
>   master
> lichongchong@ubuntu01:~/Desktop/check$ vim a.txt
> lichongchong@ubuntu01:~/Desktop/check$ git add a.txt 
> lichongchong@ubuntu01:~/Desktop/check$ git commit -m "451342"
> [dev bb1180c] 451342
>  1 file changed, 1 insertion(+)
> lichongchong@ubuntu01:~/Desktop/check$ git push origin dev
> 枚举对象: 5, 完成.
> 对象计数中: 100% (5/5), 完成.
> 使用 2 个线程进行压缩
> 压缩对象中: 100% (2/2), 完成.
> 写入对象中: 100% (3/3), 317 bytes | 317.00 KiB/s, 完成.
> 总共 3 （差异 0），复用 0 （差异 0）
> To github.com:yangyang-git/check.git
>    5e9193b..bb1180c  dev -> dev
> lichongchong@ubuntu01:~/Desktop/check$ cd ..  #切换到另外的目录
> lichongchong@ubuntu01:~/Desktop$ cd text/
> lichongchong@ubuntu01:~/Desktop/text$ cd check/
> lichongchong@ubuntu01:~/Desktop/text/check$ git branch
> * dev
>   master
> lichongchong@ubuntu01:~/Desktop/text/check$ cat a.txt 
> hrghrhg
> lichongchong@ubuntu01:~/Desktop/text/check$ git pull
> remote: Enumerating objects: 5, done.
> remote: Counting objects: 100% (5/5), done.
> remote: Compressing objects: 100% (2/2), done.
> remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
> 展开对象中: 100% (3/3), 完成.
> 来自 github.com:yangyang-git/check
>    5e9193b..bb1180c  dev        -> origin/dev
> 更新 5e9193b..bb1180c
> Fast-forward
>  a.txt | 1 +
>  1 file changed, 1 insertion(+)
> lichongchong@ubuntu01:~/Desktop/text/check$ cat a.txt 
> hrghrhg
> 24673647832
> lichongchong@ubuntu01:~/Desktop/text/check$ 
> ```

#### 2.标签管理

##### 2.1创建标签

##### 2.2操作标签

### 三、shell编程