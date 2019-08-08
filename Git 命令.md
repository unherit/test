## **Git 命令**

#### 1.查看用户和邮箱

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 ~/Desktop (master)
> $ git config user.name
> unherit
> 
> unherit@DESKTOP-8ICRK9D MINGW64 ~/Desktop (master)
> $ git config user.email
> 870633189@qq.com
> ```

#### 2.修改用户和邮箱

> ```java
> $ git config --global user.name "username"  //username 写要更改的用户名
> $ git config --global user.email "email"	//Email 写新的邮箱地址
> ```

#### 3.查看版本信息

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 ~/Desktop (master)
> $ git version
> git version 2.22.0.windows.1
> 
> unherit@DESKTOP-8ICRK9D MINGW64 ~/Desktop (master)
> $ git --version
> git version 2.22.0.windows.1
> ```

#### 4.初始化本地仓库

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git init
> Reinitialized existing Git repository in D:/test/.git/
> ```

#### 5.添加文件

##### 5.1 add文件

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git add HashMap扩容.md
> ```

##### 5.2 查看暂存区状态

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git status
> On branch master
> Your branch is up to date with 'origin/master'.
> 
> Changes to be committed:
>   (use "git reset HEAD <file>..." to unstage)
> 
>  new file:   "HashMap\346\211\251\345\256\271.md"
> ```

##### 5.3  commit 将暂存区的文件添加到版本库

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git commit -m '第一次提交'
> [master bb3462e] 第一次提交
>  1 file changed, 132 insertions(+)
>  create mode 100644 "HashMap\346\211\251\345\256\271.md"
> ```

##### 5.4 显示提交和工作树等之间的更改

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git diff
> diff --git a/test.md b/test.md
> index d51a2f7..7019f2a 100644
> --- a/test.md
> +++ b/test.md
> @@ -1 +1,2 @@
> -弟弟亚辉
> \ No newline at end of file
> +*sad*
> +# dd
> ```

##### 5.5 强制删除

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git rm -f test.md
> rm 'test.md'
> ```

#### 6.分支管理

> **1 Git branch**
>
> 　　　　一般用于分支的操作，比如创建分支，查看分支等等，
>
> 　　　　**1.1 git branch**
>
> 　　　　　　不带参数：列出本地已经存在的分支，并且在当前分支的前面用"*"标记
>
> 　　　　**1.2 git branch -r**
>
> 　　　　　　查看远程版本库分支列表
>
> 　　　　**1.3 git branch -a**
>
> 　　　　　　查看所有分支列表，包括本地和远程
>
> 　　　　**1.4 git branch dev**
>
> 　　　　　　创建名为dev的分支，创建分支时需要是最新的环境，创建分支但依然停留在当前分支
>
> 　　　　**1.5 git branch -d dev**
>
> 　　　　　　删除dev分支，如果在分支中有一些未merge的提交，那么会删除分支失败，此时可以使用 git branch -D dev：强制删除dev分支，
>
> 　　　　**1.6 git branch -vv** 
>
> 　　　　　　可以查看本地分支对应的远程分支
>
> 　　　　**1.7 git branch -m oldName newName**
>
> 　　　　　　给分支重命名
>
> 　　**2. Git checkout**
>
> 　　　　1. 操作文件  2. 操作分支
>
> 　　　　**2.1 操作文件**
>
> 　　　　　　2.1.1 git checkout filename 放弃单个文件的修改
>
> 　　　　　　2.1.2 git checkout . 放弃当前目录下的修改
>
> 　　　　**2.2 操作分支**
>
> 　　　　　　2.2.1 git checkout master 将分支切换到master
>
> 　　　　　　2.2.2 git checkout -b master 如果分支存在则只切换分支，若不存在则创建并切换到master分支，repo start是对git checkout -b这个命令的封装，将所有仓库的分支都切换到master，master是分支名，
>
> 　　　　**2.3 查看帮助**
>
> 　　　　　　git checkout --help

#### 7.生产ssh密钥

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ssh-keygen -t rsa -C "这里换上你的邮箱"
> ```

#### 8.添加ssh到GitHub

![1565252709762](C:\Users\unherit\AppData\Roaming\Typora\typora-user-images\1565252709762.png)

#### 9.连接远程仓库



> ```java
> $ git remote add origin https://github.com/unherit/test.git
> 
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git remote
> origin
> 
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git remote -v
> origin  https://github.com/unherit/test.git (fetch)
> origin  https://github.com/unherit/test.git (push)
> ```

#### 10.从GitHub上克隆项目到本地

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/b
> $ git clone https://github.com/unherit/test.git
> Cloning into 'test'...
> remote: Enumerating objects: 8, done.
> remote: Counting objects: 100% (8/8), done.
> remote: Compressing objects: 100% (5/5), done.
> remote: Total 8 (delta 0), reused 8 (delta 0), pack-reused 0
> Unpacking objects: 100% (8/8), done.
> ```



#### 10.提交到远程仓库

> ```java
> unherit@DESKTOP-8ICRK9D MINGW64 /d/test (master)
> $ git push -u origin master
> ```

