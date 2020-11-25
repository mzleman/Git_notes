#  Git与GitHub

## Git结构 



==<font size='6'>工作区</font>==(写代码)

​	 git add 👇

==<font size='6'>暂存区</font>==(临时存储)

  git commit 👇

==<font size='6'>本地库</font>==(历史版本)



## Git和代码托管中心

**代码托管中心的任务:   维护远程库**

+ 局域网环境下
  1. GitLab服务器
+ 外网环境下
  1. GitHub
  2. 码云



## Git的文件管理机制

![image-20200805160547652](Git与Github.assets/image-20200805160547652.png)

![image-20200805161324105](Git与Github.assets/image-20200805161324105.png)

`commit对象`、`tree对象`、`blob对象`

commit对象具有一个hash索引，该索引值即版本id。一个commit对象具有一棵tree，代表仓库主目录的一组快照。

tree对象也具有一个hash索引，代表文件树id，可以理解成一个目录。

tree对象中保存了各个文件快照blob对象以及子树的hash索引。

![image-20201121150722116](Git与Github.assets/image-20201121150722116.png)

![image-20200805162312028](Git与Github.assets/image-20200805162312028.png)

不同的commit对象使用parent指针形成一张反向的链表。

## 常用git命令

### git init：初始化本地仓库

git init



### git config user.xxx：设置签名

设置签名可以区分不同开发人员的身份。

签名的形式:  用户名：xxx   Email地址：xxxx@xx.com

<font color='red'>这里的签名和登录远程库的账号密码没有任何关系</font>

+ 设置项目级别/仓库级别的签名,仅在当前本地库范围有效。

  git config user.name  ==mazheng== 

  git config user.email  ==mzleman@126.com==

  配置信息保存在了当前本地仓库目录下的 ./.git/config文件中

  

+ 设置用户级别的签名，系统用户权限内有效。

  git config --global user.name =='admin'==   👈 可以加引号

  git config --global user.email  =='xxx@126.com'==

  配置信息保存在用户根目录下 ~/.gitconfig文件中



+ (基本不用) 设置系统级别的签名，对系统所有登录的用户有效。

  git config --system user.name ==admin==

  git config --system user.email ==xxx@126.com==

  

![image-20201121121008005](Git与Github.assets/image-20201121121008005.png)



*签名优先级*规则：采取就近原则，项目级别签名的优先级更高，两种签名（local和global）至少设置一个。



### git status：状态查看

git status



### git add：添加到暂存区

git add ==filePath==

git add -u  👈将显示"<font color="red">modified</font>"的`已经被git管理的文件`提交到暂存区。

git add * 	👈将显示"<font color="red">untracked</font>"和<font color="red">modified</font>"的文件都提交到暂存区。



### git commit：提交到本地库

git commit

git commit -m =="提交信息"==

git commit -am =="提交信息"== 👈 工作区直接提交到本地库，不提交到暂存区（不推荐使用）



### git rm：清除对文件和目录的跟踪、删除文件

git rm --cached ==filename==  👈清除git对该文件的跟踪，但文件系统中不会删除该文件

git rm  ==filename== 👈清除git对该文件的跟踪，同时在文件系统中删除该文件

git rm -r --cached ==dirname== 👈清除对目录的跟踪，不删除目录

git rm -r --force/-f ==dirname== 👈清除对目录的跟踪，同时删除该目录

> 不带参数的git rm命令相当于带默认参数 --force



### git mv：重命名、移动文件

git mv ==oldname==  ==newname== 👈重命名文件

git mv ==filename== ==tragetPath== 👈移动文件



### git diff：比较文件的前后差异

**当前分支下的比较命令：**

+ git diff 

   比较所有被git管理的文件在`工作区`与`暂存区`之间的区别

+ git diff ==[filename1== ==filename2...]== 

   比较指定文件在`工作区`与`暂存区`之间的区别

+ git diff --cached 

   比较所有被git管理的文件在`暂存区`与`本地库`之间的区别

+ git diff --cached ==[filename1== ==filename2...]== 

  比较指定文件在`暂存区`与`本地库`之间的区别

+ git diff commitHash1 commitHash2 [==filename==] 

  比较同一分支下不同commit对象指定文件/全部文件的区别



**不同分支commit之间的比较：**

+ git diff ==branch1== ==branch2==

  比较两个分支指针指向的commit对象之间所有文件的区别

+ git diff ==branch1== ==branch2==   --  [==filename1== ==filename2...==]

  git diff ==commitHash1== ==commitHash2==   --  [==filename1== ==filename2...==]

  比较不同commit指定文件的区别



### git stash：藏匿处

git  stash命令可以临时存储"尚未完成编辑"以及“需要临时藏匿”的文件，在需要的时候再加载到工作区。

> 将stash中的内容正常拉取的前提是：拉取出的内容与当前工作区文件状态不冲突，否则会报错，需要合并冲突。

+ git stash list

  查看藏匿的所有文件

+ git stash

  将当前工作区所有尚未暂存的全部修改藏匿起来。

+ git stash --==filename==

  将指定文件的修改藏起来。

+ git stash -m"==message=="

  将当前工作区所有尚未暂存的全部修改藏匿起来，*并写入提示信息。*

+ git stash pop n

  将stash中索引为n的工作区内容取出来。

+ git stash apply n

  将stash中索引为n的工作区内容取出来,

  > apply 和 pop 的区别在于apply不会将索引为n的内容从stash中移除。

+ git stash clear

  清空stash中的所有内容





### git restore：恢复操作

+ git restore ==filename1 filename2== 

  撤销工作区指定文件的修改。

  👉 等价于 git checkout -- ==filename1== ==filename2==

+ git restore *

  撤销工作区所有文件的修改。

  👉 等价于 git checkout -- *

  

+ git restore --staged ==filename1== ==filename2...==

  撤销暂存区指定内容相对于本地库的修改。

  > 撤销暂存区内容的操作不会影响工作区。

+ git restore --staged  *

  撤销暂存区所有内容相对于本地库的修改。

  👉等价于 git reset --mixed HEAD



### git checkout

应用：

1. 切换分支

   git checkout ==branchName==

2. 基于某一commit对象新建分支，同时切换到该分支

   git checkout -b ==branchName== ==commitHash==

3. 分离头指针

   git checkout ==commitHash==

4. 撤销工作区内容修改

   git checkout -- ==filename==  👈 单文件

   git checkout -- ==filename1== ==filename2...==   👈 多文件

   git checkout -- *  👈 所有文件





## 分支 

**定义：在版本控制过程中，使用多条线同时推进多个任务。**

 

![image-20200804222504990](Git与Github.assets/image-20200804222504990.png)

**使用分支的优势：**1. 同时并行推进多个功能的开发，提高开发效率。2. 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。



### 分支常用命令 

#### 1. 创建分支

+ git branch ==分支名== 基于当前commit创建新分支

+ git branch ==分支名== ==commitHash== 基于指定commit创建新分支

+ git checkout -b ==newBranch== ==targetBranch/commitHash== 在某个版本处创建新分支,同时切换到该分支 ⭐

  指针关系：HEAD 👉 newBranch 👉 targetBranch/commitHash

#### 2. 查看所有分支

git branch -v

git branch -av  查看包含远程分支在内的所有分支

#### 3. 切换到分支

git checkout ==分支名==



#### 4. 合并分支

git merge ==被合并分支名==

+ 第一步，切换到接受修改的分支（要合并其他分支的主分支）

  git checkout [主分支]

+ 第二步，执行git merge [被合并分支名]

**解决合并冲突：**

合并主分支与被合并分支时，若同一个文件的同一个位置在两个分支内的内容有差异，则这时产生合并冲突。

+ 第一步：合并分支，若出现冲突，git会发出提示需要解决冲突，或取消合并。

+ 第二步：若继续合并，则首先应编辑发生冲突的文件内容位置，删除标记冲突的特殊符号。

  ![image-20200804231102746](Git与Github.assets/image-20200804231102746.png)

+ 第三步：git add ==发生冲突的文件== 或   git add .

+ 第四步：git commit -m "日志信息"   <font color="red">此时commit一定不能带具体文件名</font>>

#### 5. 删除分支

git branch -d ==branchName== 👈可以删除已经被合并过的分支

git branch -D ==branchName== 👈可以删除任意分支

> 若要删除当前分支，则应先切换到其他分支再删除。



### 使用指针管理分支 

+ **新建分支testing，并切换到testing。**

  底层：新建一个分支指针testing指向当前版本，同时HEAD指针指向testing指针。

  ![image-20200805163243391](Git与Github.assets/image-20200805163243391.png)





+ **在testing分支下提交新的版本873ab2。**

  底层：在testing分支下创建了一个新的commit对象，hash索引为87ab2，该对象的parent指针指向testing指针正在指向的commit对象，再把testing指针指向新建的commit对象。

  ![image-20200805163507534](Git与Github.assets/image-20200805163507534.png)





+ **切换回master主分支**

  底层：只需要将HEAD指针指向master即可。

  ![image-20200805171322879](Git与Github.assets/image-20200805171322879.png)

  

  

  

+ **在master主分支内提交新的版本**

  底层：新建一个commit对象，hash索引设为c2b92，parent指针指向前一个commit对象f30ab。master指针指向该新建的commit对象。

  > *HEAD指针无需移动，一直指向master即可*

  ![image-20200805171504066](Git与Github.assets/image-20200805171504066.png)

## 查看提交记录

git log   完整的查看历史记录

完整的查看历史记录

![image-20200804174018030](Git与Github.assets/image-20200804174018030.png) g



**git log --pretty=oneline**   

一次提交记录显示成一行，同时显示完整的版本号

![image-20200804174101612](Git与Github.assets/image-20200804174101612.png) 



**git log --oneline** ⭐

一次提交记录显示成一行，同时显示版本号的前几位

![image-20200804174138482](Git与Github.assets/image-20200804174138482.png) 



**git reflog** 

包含移动步数 HEAD@{移动步数}

![image-20200804174328155](Git与Github.assets/image-20200804174328155.png) 



**git log --all** 

查看所有分支的历史版本

**git log --all --graph**  ⭐

以分支图的形式在终端中显示所有分支的历史版本

**git log --oneline --all --graph** ⭐⭐

​			



## 分离头指针

git checkout ==commitHash版本hash / HEAD~n指针移动位数==

个人对分离头指针的定义：头指针不再指向某个分支指针，而指向具体版本。

问题情景： 

第一步：git checkout 版本hash  或 git checkout HEAD~n将HEAD指针指向历史版本

第二步：对代码进行修改

第三步⭐：修改添加到缓存区，提交版本到本地库==假设Git将该版本hash值设为abc123==，此时便真正处于`分离头指针状态 HEAD detached`

**分离头指针状态存在这样一个问题**：==abc123== 与所有branch指针都没有关联，此时git就要求我们新建一个分支保存该版本，否则会在distached缓存被清除后，丢失该版本。

```bash
git branch ==newBranchName== ==abc123==

或
git checkout -b newBranchName
```

![image-20201122172536098](Git与Github.assets/image-20201122172536098.png)





## 版本回退git reset 和 git revert

### 基于分支的版本回退git reset

git reset 命令主要是修改分支最近版本的指针。



### **git reset soft,hard,mixed之区别深解 ⭐**

[参考资料](https://blog.csdn.net/zpf336/article/details/80896020?utm_medium=distribute.pc_relevant.none-task-blog-title-3&spm=1001.2101.3001.4242)

**首先我们来看几个术语**

- **HEAD:** 这是当前分支版本顶端的别名，也就是在当前分支你最近的一个提交
- **Index:** 也被称为Staging area，是指一整套即将被下一个提交的文件集合。他也是将成为HEAD的父亲的那个commit
- **Working Copy:** Working copy代表你正在工作的那个文件集

当你第一次checkout一个分支，HEAD就指向当前分支的最近一个commit。在HEAD中的文件集（实际上他们从技术上不是文件，他们是blobs（一团），但是为了讨论的方便我们就简化认为他们就是一些文件）和在index中的文件集是相同的，在working copy的文件集和HEAD,INDEX中的文件集是完全相同的。所有三者(HEAD,INDEX(STAGING),WORKING COPY)都是相同的状态，GIT很happy。

![img](https://img-blog.csdn.net/20180703134050437)

当你对一个文件执行一次修改，Git感知到了这个修改，并且说：“嘿，文件已经变更了！你的working copy不再和index,head相同！”，随后GIT标记这个文件是修改过的。

![img](https://img-blog.csdn.net/20180703134315399)

然后，当你执行一个git add,它就stages the file in the index，并且GIT说：“嘿，OK，现在你的working copy和index区是相同的，但是他们和HEAD区是不同的！”

![img](https://img-blog.csdn.net/20180703134238194)

当你执行一个git commit,GIT就创建一个新的commit，随后HEAD就指向这个新的commit，而index,working copy的状态和HEAD就又完全匹配相同了，GIT又一次HAPPY了。

![img](https://img-blog.csdn.net/20180703134050437)



**Reset 参数**

git reset 有一些参数，不同参数对应的细节变化又是不一样的，具体如下：

**`--mixed：`**

> --mixed是reset的默认参数，也就是当你不指定任何参数时的参数。它将重置HEAD到另外一个commit,并且重置index以便和HEAD相匹配，但是也到此为止。working copy不会被更改。所有该branch上从original HEAD（commit）到你重置到的那个commit之间的所有变更将作为local modifications保存在working area中，（被标示为local modification or untracked via git status)，但是并未staged的状态，你可以重新检视然后再做修改和commit。



> ![img](https://img-blog.csdn.net/20180703134315399)

`--soft：`

> --soft参数告诉Git重置HEAD到另外一个commit，但也到此为止。如果你指定--soft参数，Git将停止在那里而什么也不会根本变化。这意味着index,working copy都不会做任何变化，所有的在original HEAD和你重置到的那个commit之间的所有变更集都放在stage(index)区域中。
>
> ![img](https://img-blog.csdn.net/20180703134238194)

`--hard：`

> --hard参数将会重置HEAD返回到另外一个commit，重置index以便反映HEAD的变化，并且重置working copy也使得其完全匹配起来。这是一个比较危险的动作，具有破坏性，数据因此可能会丢失！如果真是发生了数据丢失又希望找回来，那么只有使用：**git reflog**命令了。makes everything match the commit you have reset to.你的所有本地修改将丢失。如果我们希望彻底丢掉本地修改但是又不希望更改branch所指向的commit，则执行：



### reset 参数归纳总结：

`git reset --hard `

![image-20200804205051569](Git与Github.assets/image-20200804205051569.png) 

1. 切换到目标分支

   git checkout master

2. 指定要回退的版本

   git reset --hard ==版本索引值==            使用索引值前进后退

   git reset --hard HEAD^            后退一个版本

   git reset --hard HEAD^^          后退两个版本 （几个^就后退几个版本）

   git reset  --hard HEAD ~==n==     后退n个版本 （^换成了~）

   > 注意：在某个分支上执行git reset 会导致丢失原版本和回退版本之间的版本。



 `git reset --mixed `

执行git reset 相当于执行 git reset --mixed 即默认使用mixed。

将分支的指针指向某个commit，同时暂存区与该commit一致，工作区代码保留。

应用场景： 

1. 撤回暂存区的内容。

   git reset HEAD

2. 撤回暂存区一个或多个文件，暂存区的撤回内容不会对工作区造成影响。

   git reset HEAD -- ==file1== ==file2...==

   `相当于 `git restore --staged ==file1== ==file2...==

   





`git reset --soft`

将分支的指针指向某个commit，暂存区和工作区仍保留原来状态。

HEAD！= INDEX = WORKING COPY

由于暂存区仍然保持回退前的状态，所以如果想要撤销回退，直接git commit即可。



### **常见情景：删除文件并找回**

前提：删除前，文件存在时的状态提交到了本地库。（本地库中存储了具有该文件的状态版本）。

操作：git reset --hard [指针位置]

+ 删除操作已经提交到了本地库：调整指针位置到该文件存在的历史版本。
+ 删除上做尚未提交到本地库：指针位置调整为HEAD（最新版本）。

```bash
//demo：使用版本的后退来撤销删除文件的操作

git rm new.txt
git commit -m "delete new.txt"

git reset --hard HEAD^     👈回退一个版本，即返回到删除new.txt前的版本
```



```
//demo：删除文件的操作尚未提交到本地库，但拉取到了缓存区，恢复该文件的方法👇

//git reset命令的--hard参数的含义是强制本地库、缓存区、工作区同时回退到某个版本

//可利用该特性来拉回缓存区的内容

git rm new.txt

git commit --mixed HEAD  👈回退到当前版本，使缓存区与本地库一致。
//或者使用 git restore --staged new.txt + git restore new.txt分两步解决
```





### git revert

1. git revert ==commitHash==

   撤销指定commit版本的操作，这个操作也会生成一个新的commit，指定版本commit之前及之后的操作均不受影响。

   > **需要注意的是，`如果git revert指定了一个非上一版本的历史版本`，则该版本的parent的某些内容`可能会`和当前版本发生冲突，这就需要`合并冲突`才能提交git revert产生的新版本。**
   >
   > 发生冲突的情况举例：
   >
   > current1  " echo abc > new.txt "👈HEAD
   >
   > history2  "create a.txt"
   >
   > history3   "echo 123> new.txt"   👈 git revert history3
   >
   > history4  "create new.txt"
   >
   > 可见，history4版本下new.txt内容为空，history3版本下new.txt内容为123，current1版本new.txt内容为abc，如果撤销history3的commit，则history4和current1之间便丢失了中间过程（git存储的是文件内容的变化），会导致历史链断裂。
   >
   > 不发生冲突的情况举例：
   >
   > current1  " echo abc > new.txt "👈HEAD
   >
   > history2   "echo 123> new.txt" 
   >
   > history3  "create a.txt"  👈 git revert history3
   >
   > history4  "create new.txt"
   >
   > 上述的情况不会发生冲突是因为撤销history3并不会使history2和history4之间丢失中间的变化过程。

2. git revert ==commitHash1== ==commitHash2...==

   撤销指定的多个commit版本。

3. git revert ==olderCommit..oldCommit==

   撤销 (olderCommit, oldCommit]之间的commit，左开右闭，即会保留olderCommit的提交。

   > current 👈HEAD
   >
   > history1
   >
   > oldCommit    ← revert end
   >
   > ....
   >
   > olderCommit  ← revert start
   >
   > ....
   >
   > 如上所示，如果(start, end]之间的commit撤销后，olderCommit和 [ history, currnet]之间丢失了中间过程，同样会导致冲突。



​	综上所述：使用git revert时最推荐的做法还是根据从新到旧的顺序，依次撤销commit，尽量不要存在间隔。

## 变基命令 rebase

git rebase -i ==baseCommit==

> -i / --interactive  代表交互式

### 应用情景1：修改commit的message

1. **修改最近一次提交的commit的message**

   git commit --amend， 会出现以下文件内容：

   ![image-20201121220040518](Git与Github.assets/image-20201121220040518.png)

   修改顶部的message内容，:wq保存即可。

   

2. **修改历史任意一个commit的message**

   例如，我们需要修改 hash值为balf22b的commit对象的message，则我们`需要以balf22b的parent为基础`进行变基操作。

   ==commit 18b080c==   👈`parent`   ==commit balf22b== 👈`parent` ==commit child123==

   ![image-20201121221505369](Git与Github.assets/image-20201121221505369.png) 

   ```bash
   $ git rebase -i init00 进入交互式页面。（一个文件）
   ```

   ![image-20201121221531229](Git与Github.assets/image-20201121221531229.png)

   pick 命令代表使用该commit对象

   reword 命令代表修改该commit对象的message

   drop 命令代表丢弃该commit对象，即从历史版本链中删除该版本。

   squash 命令代表合并commit

   ......

   ⭐ 可见，我们这里应该使用reword 

   ![image-20201121221721667](Git与Github.assets/image-20201121221721667.png)

   ![image-20201121221837885](Git与Github.assets/image-20201121221837885.png)



> 这里解释为何必须选择目标commit的父级commit进行变基：
>
> 如果直接基于需要修改message的commit进行变基，则修改该commit的message后，该commit的hash值便发生了改变（例如由abc123变为def456），其后的版本commit的parent指针仍然指向abc123，所以就发生了版本链的断裂，而且abc123指向不明，就会报错。



### 应用情景2：合并commit

1. **合并连续的commit**

   ![image-20201121223201907](Git与Github.assets/image-20201121223201907.png) 

   如上图，连续编辑了两次index.html的操作可以合并成一个。

   ```bash
   #这里，853f263和d1bc279可以合并成一个，则必须使用853f263的父级commit进行变基操作。
   $ git rebase -i 30438ce
   ```

   ![image-20201121223508509](Git与Github.assets/image-20201121223508509.png)

   ```bash
   #使用squash命令，将d1bc279合并到853f263
   ```

   ![image-20201121223715612](Git与Github.assets/image-20201121223715612.png)

   ![image-20201121223831593](Git与Github.assets/image-20201121223831593.png)

   ![image-20201121223924154](Git与Github.assets/image-20201121223924154.png)



2. **合并不连续的commit**

   ![image-20201121224221399](Git与Github.assets/image-20201121224221399.png) 

   在两次变基index.html中间有提交README的commit，则要合并间隔的commit`0775df9`和`5e1d10e`需要特殊操作。

   > 注意：
   >
   > ​	合并0775df9和5e1d10e，要求合并的结果与 0775df9到5e1d10e之间任何一个版本不存在冲突，才允许合并。

   ```bash
   $git rebase -i 30438ce
   ```

   ![image-20201121224428436](Git与Github.assets/image-20201121224428436.png)

   ![image-20201121224534459](Git与Github.assets/image-20201121224534459.png)

   ![image-20201121224638413](Git与Github.assets/image-20201121224638413.png)

   ![image-20201121224751464](Git与Github.assets/image-20201121224751464.png)





### 应用场景3：删除一个或多个commit

git rebase -i ==baseHash==

然后对指定的commit使用`drop命令`，其用法逻辑与git revert类似。但需要注意的是，一旦drop某版本，就会丢失该版本。而git revert会保留原版本，创建新版本。 

![image-20201123172927502](Git与Github.assets/image-20201123172927502.png)

### 变基操作失败

git status查询状态，若处于rebase交互状态但抛出了错误，需要处理冲突。

或者可以使用git rebase --abort跳出rebase交互。







## Git仓库本地备份

假设本地有个git仓库的地址为 `~/Desktop/Git`

想要在本地建立地址为 `D:/repository/Git`的备份仓库。

### **使用本地库备份**

1. 切换到D:/repository目录（父级目录）

2. 执行命令

   ```bash
   # 命令格式： git clone 被备份仓库目录名/.git 备份仓库目录名  
   git clone ~/Desktop/Git/.git Git
   ```

3. 测试：

   ```bash
   cd ~/Desktop/Git # 切换到被备份的仓库
   git remote add localBackup D:/repository/Git
   git push -u localBackup master # 将master分支上传到本地备份库
   ```

   

### **使用裸仓库备份**

裸仓库：不含有工作区的Git仓库，由于不存在工作区，所以在该仓库目录下不能执行用于更新的git命令，该仓库只能通过其他仓库push或自身去fetch来更新。

> 网络释义：使用 git init --bare <repo> 或git clone --bare <url> <repo>可以创建一个裸仓库，并且这个仓库是可以被正常 clone 和 push 更新的， 裸仓库不包含工作区，所以并不会存在在裸仓库上直接提交变更的情况。

1. 切换到D:/repository

2. 执行带--bare参数的git clone命令，创建裸仓库（不带工作区）

   ```bash
   # 使用哑协议
   git clone --bare ~/Desktop/Git/.git Git 
   # 或使用file协议
   git clone --bare file://C:\\Users\\Mazheng\\Desktop\\Git\\.git Git 
   
   # 此时 D:/repository/Git成为一个裸仓库。
   ```

3. 测试：

   ```bash
   cd ~/Desktop/Git # 切换到被备份的仓库
   git remote add localBackup D:/repository/Git
   git push -u localBackup master # 将master分支上传到本地备份库
   ```

   



## 本地库与GitHub远程库交互

### 本地库内容推送到远程库 

1. 首先在GitHub账户内创建仓库。

2. 在本地新建一个git仓库。

3. 本地库提交版本后向远程库推送。

   以http协议的URL向GitHub仓库推送为例

   git remote add ==URL别名/主机名== ==远程仓库URL==

   例：git remote add origin https://github.com/mzleman/MyFirstTest.git 

4. 输入GitHub账号与密码

   <font color="red">只有远程库仓库的Owner以及Collaborator才有权限向远程库推送，同时如果本地库的远程库版本比远程库当前版本旧（远程库被他人更新，用户本地的版本旧，或者是拉取的版本旧），则无法向远程库推送。必须拉取新的远程库到本地后，再更新内容，才可以向远程库推送。</font>

5. git push -u ==URL别名/主机名== ==本地分支名==

   -u参数是将本地的分支与远程主机的同名分支关联起来。同时git push命令用于将当前分支提交到远程库。

6. 在使用过一次带-u的git push命令后，只需要在切换到当前分支后直接使用git push就可以将当前分支推送到远程对应的分支。

   ```bash
   git remote add origin https://github.com/mzleman/MyFirstTest.git 
   git push -u origin master  
   👆 第一次建立本地分支与远程分支之间的关联，并将本地的master分支推送到origin主机下的同名分支(这里就是master)。
   
   👇
   git checkout master
   git push  在已经建立好远程分支关联的情况下，可以直接使用git push将本分支推送到远程分支
   ```



### SSH密钥登录

**通过以下步骤：**

1. 客户端生成私钥和公钥。

2. GitHub账户内保存客户端SSH公钥

3. 本地库设置远程库SSH地址的别名，例如==origin==

   ```bash
   git remote add origin <SSH>
   ```

**就可以在本地库使用SSH远程地址向GitHub仓库推送，具体可百度或看视频**

[参考视频1](https://www.bilibili.com/video/BV1Ep4y1Q7KR?p=31)

[参考视频2](https://www.bilibili.com/video/BV1pW411A7a5?p=42)

![image-20201124175026945](Git与Github.assets/image-20201124175026945.png)



### 远程库拷贝到本地

1. 切换到准备存放本地库的`父级目录`。

2. 拷贝远程库到本地

   git clone ==GitHub仓库的URL(https/ssh)== ==本地库文件夹名==

   <font color="red">git clone命令会做三件事： ①完整的把远程库下载到本地 ；②创建origin远程地址别名 ；③在本地库目录中执行git init命令，初始化本地库。</font>
   
   > 注意：如果在服务端设置了公钥，本地用户配置了公钥和私钥，则使用ssh地址克隆时，可以免密推送。
   
3. 切换到本地库目录

   cd ==本地库文件夹名==



### 远程库的拉取 

本地库拉取远程库的较新版本，若本地库的历史记录中已经拉取过了远程库该版本，本地库将不发生任何改变，并提示本地库已经是最新版本。 

+ **间接拉取某个分支**

  1. git fetch ==远程库别名== ==分支名==

     执行fetch操作后，git本地库会创建一个临时的新分支，分支名为  ==远程库别名/分支名== ，这个分支可供本地库拥有者查看远程库拉取的内容，再决定是否合并到本地库某个分支。

  2. 切换到要合并远程库的本地库分支，执行git merge ==远程库别名/分支名==

  3. 若发生合并冲突，解决冲突后执行git add 和git commit操作。
  
  

+ **直接拉取某个分支**
  1. git pull ==远程库别名== ==分支名==
  2. 若发生合并冲突，解决冲突后执行git add和git commit操作。



## 团队协同合作

多数情况下，一个项目是由多人协同开发。

假设现有两个人维护开发同一个分支，可能会产生以下的情景：

### 情景1. 两人修改了不同的文件

Coder1 在commit abc123的基础上修改了`index.html`

Coder2 在commit abc123的基础上修改了`style.css`

Coder1 提交了修改了index.html的版本commit def456，同时push到远端分支。

Coder2 提交修改style.css的版本，再向远端push时会<font color ="red">报错</font>

### 情景2. 两人修改了同一文件的不同区域

Coder1 在commit abc123的基础上修改了index.html的`第一行`

Coder2 在commit abc123的基础上修改了index.html的`第二行`

Coder1 提交了修改了index.html的版本commit def456，同时push到远端分支。

Coder2 提交修改index.html的版本，再向远端push时会<font color ="red">报错</font>。



> 以上两种情况，只需要后提交的Coder在提交前进行一次 fetch + merge 的操作，就可以解决问题。因为多人修改不同的文件、修改同一文件的不同区域都不会引发合并冲突。
>
> fetch + merge = pull
>
> 但是建议使用fetch后查看一下当前分支和远端分支的区别。



### 情景3. 两人修改了同一文件的同一区域

Coder1 在commit abc123的基础上修改了index.html的`第一行`

Coder2 在commit abc123的基础上修改了index.html的`第一行`

Coder1 提交了修改了index.html的版本commit def456，同时push到远端分支。

Coder2 提交修改index.html的版本，再向远端push时会<font color ="red">报错</font>

> 这种情况要求Coder2进行一次 fetch + merge + 合并冲突 的操作，然后再将合并后新版本push到远程库。



### 情景4. 一人只修改了文件名，另一人只修改了该文件内容

Coder1 在commit abc123的基础上将index.html改名为index.htm

Coder2 在commit abc123的基础上修改了index.html的`第一行`

Coder1 提交了修改了index.html的版本commit def456，同时push到远端分支。

Coder2 提交自己的的版本（包含修改了第一行的index.html），再向远端push时会<font color ="red">报错</font>

> 此时需要Coder2将远端分支 fetch + merge  / pull 即可。由于修改文件名的操作并不会修改blob对象的哈希值，所以Git在Coder2拉取def456时能感知到index.html只是修改了文件名，而没有修改文件内容，所以可以直接合并。



### 情景5. 两人同时修改了一个文件的文件名

Coder1 在commit abc123的基础上将index.html改名为index.htm

Coder2 在commit abc123的基础上将index.html改名为main.htm

Coder1 提交了修改了index.html的版本commit def456，同时push到远端分支。

Coder2 提交自己的新版本（包含main.htm），再向远端push时会<font color ="red">报错</font>

> 此时需要Coder2将远端分支 fetch + merge / pull 拉取远端分支最新内容。由于修改文件名的操作不会修改blob对象的hash值，Coder2的新版本和远端分支最近版本（Coder1提交的def456）之间的冲突`在于文件名的冲突`，所以需要依次执行
>
> git rm index.html
>
> git rm main.htm/index.htm
>
> git add index.htm/main.htm
>
> 即在main.htm 和 index.htm中选择一个，提交成新commit后再push到远端分支。



### 情景6. 两人同时修改了一个文件的文件名，并对内容都做了修改。

Coder1 在commit abc123的基础上将index.html改名为index.htm，修改了第一行内容。

Coder2 在commit abc123的基础上将index.html改名为main.htm，修改了第一行内容。

Coder1 提交了修改了index.html的版本commit def456，同时push到远端分支。

Coder2 提交自己的新版本（包含main.htm），再向远端push时会<font color ="red">报错</font>

> 尽管不建议这样处理，但还是进行简单说明处理办法：
>
> 在这种情况下，Git会认为 远端分支删除了index.html、新增了index.htm，本地新增了main.htm，只需要简单的合并即可，并不会发生冲突。

> ❗  **强烈建议** ❗：在重命名某个文件后进行一次commit操作，同时push到远程库。让重命名文件的操作能够让团队协作者发现，从而可以提前发现问题。！！！



## 跨团队协作 

![image-20200805225957085](Git与Github.assets/image-20200805225957085.png)