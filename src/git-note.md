---
title: Git Backup
date: 2017-06-20 13:46:04
tags: [git]
categories: tool
---

references: ， [2](http://ndpsoftware.com/git-cheatsheet.html)， [3](https://github.com/k88hudson/git-flight-rules)
https://gogs.io/
https://git-scm.com/book/zh/v2

<!-- TOC -->

- [图解](#图解)
- [github 设置 ssh 访问](#github-设置-ssh-访问)
- [git 初始化设置](#git-初始化设置)
- [上传本地项目到远程](#上传本地项目到远程)
- [常用命令](#常用命令)
- [时光机](#时光机)
  - [git checkout](#git-checkout)
  - [git reset](#git-reset)
  - [git revert](#git-revert)
  - [git rebase](#git-rebase)
  - [典型场景的综合用法](#典型场景的综合用法)
- [远程仓库](#远程仓库)
- [分支](#分支)
- [git diff 打补丁 patch](#git-diff-打补丁-patch)
- [暂存-恢复工作区](#暂存-恢复工作区)
- [标签](#标签)
- [.gitignore 文件](#gitignore-文件)
- [LF 和 CRLF](#lf-和-crlf)
- [git hooks](#git-hooks)
- [git subtree 子仓库](#git-subtree-子仓库)
- [husky](#husky)

<!-- /TOC -->

# 图解

[1](https://github.com/geeeeeeeeek/git-recipes/wiki/4.1-%E5%9B%BE%E8%A7%A3-Git-%E5%91%BD%E4%BB%A4)

<img src="Screenshot_1.png" width=60%>

![alt](git-full.png )

[drawio](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Untitled%20Diagram.xml#R5ZpLc6M4EIB%2FjY9OIcTzOElmdi9bNVWZqp09KiAbVbDlEnLszK9fCQTWA2LHgTjjySWo9UB0tz61Wp7Bu9X%2BL4Y2xT80x%2BXM9%2FL9DN7PfPEXQvFPSl4aSRwFjWDJSN6IwEHwQH5hJfSUdEtyXBkNOaUlJxtTmNH1GmfckCHG6M5stqCl%2BdYNWmJH8JCh0pX%2BS3JeNNLEjw%2FyvzFZFu2bQZQ2NSvUNlZfUhUopztNBL%2FO4B2jlDdPq%2F0dLqXyWr00%2Fb4N1HYTY3jNT%2BngNx2eUblV36bmxV%2Faj2V0u86xbA9m8HZXEI4fNiiTtTthXiEr%2BKpU1Sv6jB7rnrLEcEV%2B6WXKEdfKwi%2BwXsY50YvKfJpEzRYzjveDXww6PQoHxHSFOXsRTVSHOYjDpo9yPj9QttgdTOknSlZoZuyMhpT7LLvBDxoWD0rJ%2FQoPHIUXpOJUDXK23kdQTNjqQekFBKGjF%2BD16AWOoZfQ0UvF5Sq0tSJ6iVWOP0QjfgpvjvsK8Ht0AtIRdBIdX5x4nX%2BRQBOlx5JmT6YORO03UralbMueO3%2FCe8J%2Fas%2F%2FiWfvxvciVf6OGRETxkzK66HE5H%2FWjUBbbPqAOG0Fdqdmujh3aGpZQHwS3bIMG0ziiC0x17zDtZNmhbDHCK2M4RJx8mxOos8w6g3fKRHT69wAxNbCiC3bNpNXvXTYWgPB8MhAzSc7AwkLoxet2UY2qIYn3O2T1nsObteMeHDCTqcn%2BWXs%2BOWSyNejPJ%2FJxRChlVyMpTDD7YIIV20lS147RVTXPDLxVEsoc2X6iDeO24s1zU1X17ca6XpKR6J1eDsL74UElWS5FoJM%2BJ700FvJBiI29S%2BqYkXyXPa%2FLdEjLm9R9rSsGXxHS8rq98JF%2FfcaXcw967DR6z4fvb5BiWWYeL5hwTmA73PltgldLCrMLW94q%2F2Tj%2BaSzh%2BhHg%2BaBJJ8GJVAoUug4JIECjxobUPReQSy93ghmIRAQWxNGIxLoHSAQBldrQi%2FMlwkH4%2BLvQH6dlR7gFFg0u5VJ9JkTdd4CCbeEExEkBIkRqDjeeHxQCcBJmiGICMiVcbbCWYlqiqS%2FSjIuq3SWDdaRORfkkfCv6zAGERnEgn4zlATRUVzaLIvGRdJAAwxqcie6FY%2BCbaI1oIudUhzehSUFbhnCPFYB1eyYPfL8WIuJ%2Faltj%2FK%2B1vU3es2SH6K3eTKMAq8We%2BS%2BW3CrnYyZ5MyR1XRsfFUEl2MMVaoEoTheYSx170fWAMN8OUMC%2FVkMXaUPZH1Ughzwq5tRcFjKwqkqQn3OfhMC8o113aTI45rIxXjWGvLadUksIFmvBIveI%2FpOJXH5GqDMuEzP2Thfh68M5nVun3qpvPinuU7RjIPuJmrrMRofSUa7aIgC1GwjWU1FQdTqdhNwrxtMzghbAYiTA6MsDmJ2vJw2KyHzOBC0fFl84VRYu5c8MzYGIahNdBpO9eb84W%2BtdUGI4fGbsKoCWzFYsdTR7WeykBeeXAbH9uKJ0sSQN86wM395MZy1HG2azfrMzry5KE%2FNpCXQt%2FMCtjZxxh2FyIOFHtAOTkOP1%2Fy0tol05GSl93AYycvo2lx6LsZLxOH83lFFzUXjasUktsXKdeFsPRiCAMOwmA6CcJ8N0k0BcKaG1mFMNAmOzWEWVe4qe9%2FKoT1RHQXRRhMLSJEFnnORlgcT4Iw30JYOPL9S2uh4YjuTyLXsXPihORKEptcdnZrHG69N%2FX4Grc0Lvkz6%2FD43vsW%2FQYoMqEIho6y18GsufWrlcDOOpzMrBBYzJrmFAoSi1kjX9D0%2FPTODrsKxPIT4GUcUq%2BLZEczur8ZyUTx8GPapvnhJ8nw6%2F8%3D)

# github 设置 ssh 访问

```shell
# 进入 ${user}/.ssh
ssh-keygen -t rsa -C "775000738@qq.com" # 可能弹出交互"要设置passphrase?"直接回车(可通过ssh-keygen -p 修改)
# 复制xxx.pub下的text到github即可
# 测试
ssh -T git@github.com
```

github 和 gitee 同时设置 ssh 访问: https://blog.csdn.net/weixin_36191602/article/details/80946242

# git 初始化设置

```sh
$ git config --global user.name "xiaoyu"
$ git config --global user.email 775000738@qq.com

# 查看配置信息
$ git config --list
# 查看特定
$ git config user.name

```

# 上传本地项目到远程

首先需要在服务器上创建项目

```sh
# 创建裸仓库
ssh user@host git init --bare /path/to/repo.git

```

接下来分几种情况

-   本地什么都没有, 在本地克隆一个 git 仓库

```bash
git clone git@116.62.50.152:PAI_Portal/pai-portal-user-service.git
cd pai-portal-user-service
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

-   本地已经存在项目, 项目还未纳入 git

```bash
cd existing_folder
git init
git remote add origin git@116.62.50.152:PAI_Portal/pai-portal-user-service.git
git add . # 将当前文件纳入缓存区
git commit -m "Initial commit"
git branch --set-upstream-to=origin/master master # 绑定远程分支到本地分支
git push -u origin master
```

-   已经纳入 git 的项目

```bash
cd existing_repo
git remote add origin git@116.62.50.152:PAI_Portal/pai-portal-user-service.git
git push -u origin --all # `-u`表示upstream, 当有多个远程仓库是需要指定;同时会有这个效果: 以后可以直接使用git push, git pull 而不必加哪个server
git push -u origin --tags
```

# 常用命令

```sh
# add 同时 commit
git commit -a -m "xxxxx"
# 将改动提交到上次的 commit 中
# 还能顺便修改上次 commit 的提交信息(可选)
# 如果缓存区什么修改也没有, 就只修改上次 commit 的提交信息
# 注意:这条命令是生成新的快照, 所以 commit code 也会变化; 不要用在已经推送到 remote 中的内容上
git commit [-m "xxxxx"] --amend
# 如果不希望修改提交信息, 不希望弹出 vim 让你修改, 则:
git commit --amend --no-edit

# 提交信息换行
# - 单引号
git commit -m '
first line
second line
'
# - 多个 -m 项
git commit -m "line one" -m "line two"
# - 单引号前添加$
git commit -m $'first line\nsecond line'




 # 查看 指定的 commit 信息, 如 id 号, 提交注释, 作者, parent commit (比如merge形成的commit 就有两个 parent)
git show <commit id>


# [ 提交历史 ] 

# 空格键滚动, q 退出
# 可以定义多种输出方式:

-   `git log -n <n>` --显示 n 个提交;
-   `git log --oneline` --每个提交压缩到 1 行 (常用)
-   `git log --stat` --那些文件改动, 增删行数
-   `git log -p` -- 全部信息, 最详细
-   `git log --author="<pattern>"` --特定作者的提交, 支持 regex
-   `git log --grep="<pattern>"` --特定的提交信息, 支持 regex
-   `git log <since>..<until>` --特定起止标志,两个参数可以是提交 ID、分支名、HEAD 或是任何一种引用
-   `git log <file>` --特定文件的提交
-   `git log --graph --decorate --oneline` ----graph 标记会绘制一幅字符组成的图形，左边是提交，右边是提交信息。--decorate 标记会加上提交所在的分支名称和标签。--oneline 标记将提交信息显示在同一行，一目了然。

```

# 时光机

关于 reset, checkout, revert

## git checkout

可以切换分支 (见 [分支](#分支))

可以从历史 commit  中 检出 内容到 work dir. 使用: `git checkout [commit] [file]`

-   `git checkout <commit> <file>` 从历史 commit 检出文件 , 检出的文件被复制到 work dir, 且添加到 index

    类似 `git reset --soft <commit> <file>` , 区别是 git checkout 对 历史 commit 没有影响. 可以通过 `git checkout <branch/newest_commit> <file>` 又回到该文件最新版本

    如:

    `git checkout HEAD~ foo.c` 将 foo.c 的上个版本复制到 work dir, 并且 添加到 index



-   `git checkout <commit>` 检出提交 , work dir 中的所有文件都被替换为历史版本. 

    此时处于"deteched HEAD"/"head 分离状态", 处于一个匿名分支上. 任何的修改提交在回到 newest commit 之后都会被丢弃.相当于 checkout 出来的内容都是只读的 (因为所有的修改在切回去后都会丢失嘛), 

    如果查看之后,确实希望回滚, 可以使用 `git revert` 或 `git reset` 来回滚任何不想要的更改; 
    
    如果希望退出"deteched head"状态, `git checkout <branch_name/newest_commit>`即可

    如果在"deteched head"状态下还是想做修改, 在回到 head 时希望保留更改, 可以在该状态下创建分支 `git checkout -b name`, 再在该分支上进行修改提交.(相当于为这个匿名分支命名了)


-   `git checkout [--] [file]` 从 index 中复制某个文件到 work dir, 相当于丢弃 work dir 中的脏数据

    `git checkout .` 表示丢弃全部工作区文件修改;


## git reset

将 head 移动到之前的某个 commit, 即回滚到指定 commit, 隔在中间的 commit 都会丢失 , 用于回退本地分支上的 commit, 

可以和 git revert 对比, git revert 会保留区间内的 commit 然后将改动生成新的 commit;

基本用法 `git reset [id] [file]`, `id 默认 head`, `file 默认 all file (不写or用点号代替)`

回退前，查看从近到远的提交日志及版本号：`git log`，如果输出日志太多，可加参数`--pretty=oneline`or `--oneline`简化输出结果；


可以带 三个 配置项:

- `git reset --soft [id] [file]` - 回退到某个版本，回退的内容不会丢失, 保存在 index, 对 work dir 没有影响. 

    若想恢复回退内容, 直接 commit 即可. 可以用于 push 前合并 commit 信息, 或者修改上一个版本的 commit 信息

    若想继续回退内容到 work dir, `git reset [file]`. 

    如:

    git reset --soft head^ 回退到上个版本, 回退内容保存在 index

    id 默认 head, 若省略 即 git reset --soft: 没有任何效果, 无论 work dir , index 区域是否脏的    

- `git reset [--mixed] [id] [file]` -  回退到某个版本, 回退内容不会丢失, 也不会保存在index, 而是保存在 work dir. 

    若想恢复回退内容, add, commit 即可. 可用于 push 前 细化, 合并 commit 信息

    若想完全删除回退内容, `git checkout .` 删除全部文件上的更改 or `git checkout <file>`删除指定文件上的更改

    id 默认 head, 若省略 即 git reset : 清空 index 区域, 清空的内容变为 work dir 中的脏数据

    --mixed  默认选项

- `git reset --hard <id>` - 回退到某个版本, 回退内容彻底丢失, 不会保留在 index, work dir 

    如:

    git reset --hard head^ 彻底回退到上一个版本 (上上一个版本就是HEAD^^, 上100个版本是 HEAD~100)

    如果还想恢复已经删除的回退内容(重返未来), `git reflog` 查看命令历史, 使用 `git reset <commit code> --hard` reset 回去, 最近的活动用 HEAD@{0} 标记在上方显示; reflog 引用日志提供的安全网只对提交到本地仓库的更改有效，而且只有移动操作会被记录

    id 默认 head, 若省略 (即 git reset --hard): 会清空 work dir, index 区域, 如果这两个区域本来就是干净的, 则不产生任何效果
    


## git revert

https://segmentfault.com/a/1190000012897697

https://www.jianshu.com/p/5e7ee87241e4

`git revert <commit>`: 撤销指定 commit, 作用和 git reset 类似. 区别是 git revert 会删除间隔内的 commit 内容, 然后生成新的提交;

相比git reset，它是生成新的 commit。因此，git revert可以用来回退已经推送到远程的分支上的内容. 

需要分两种情况，因为有两种 commit: 

- 普通 commit

    使用 `git revert <commit id>` 即可, 生成一个新的 commit，将指定的 commit 内容从当前分支上撤除。

- merge commit (通过 merge 形成的 commit, 有两个 parent commit)

    使用 `git revert -m <1 or 2> <id>` , -m 标识出主分支, 取值 1 or 2, 也就是 `git show <id>` Merge 行里面列出来的第一个还是第二个。
    
    来自主分支上的内容将被保留, 来自另外一个分支的内容将被撤销



看看示例:

- `git convert` 可以查看帮助信息

- `git revert <id>` 撤销指定 commit (仅仅适用于 普通 commit 的 撤销)

    如:
    
    `git revert head` 撤销当前 commit, 即回退到上个版本

    `git revert HEAD~2` , 找到上上版本, 然后创建一个新的提交来撤销间隔区间内的修改

    若出现错误, 则修正冲突, 然后 git add, git revert --continue, 如果解决不了冲突希望放弃, git revert --abort

- `git commit -n <id start>..<id end>` 撤销一串连续的提交, 这是一个前开后闭区间，即不包括 commit start，但包括 commit end

    revert 命令会对每个撤销的 commit 进行一次提交，--no-commit 或者 -n 后可以最后一起手动提交。

## git rebase

- `git rebase <branch>` 移动分支, 将当前分支 rebase 到 指定分支的最前面

    这里 branch 其实可以是任何类型的提交引用（ID、分支名、标签，或是 HEAD 的相对引用）

    `git rebase -i <branch>` 交互式地进行 rebase, 一般是在 push 到远程之前对 commit 进行一次统一大清理

    执行这条交互式命令后, 会进入 vim, 是几个 commit 的 SHA id 值, 从上到下是越来越新的 commit. 如果没有比 指定的 commit 新的提交的话则出现 noop

- `git rebase <commit id>` 合并多个 commit. 当前提交点--> 指定提交点, 比指定 commit id 新的 commit 都有被合并 or 修改的可能

    如果 rebase 出现冲突退出来, rebase 会被锁定, 再次执行时, 提示有三个选项:

    -   `git rebase --abort`来忽略之前的 rebase 尝试,并恢复 HEAD 到开始的分支.
    -   `git rebase --continue`就继续上次修改, 一般是 rebase 中间处理 merge 冲突后 git add 后 使用.
    -   `git rebase --skip`是重新开始 rebase 并跳过现在所进行的处理.



进入vim 中, 初始状态所有出现的 commit id 前面都是 pick. 这个 pick 表示对该 commit 进行的操作, 有:

-   <font style="color:yellow;">pick</font> pick, 就是说保留该 commit, 也可以用缩写 p, 必须要有一个 commit 是 pick. (黄色)
-   <font color="green">squash</font> squash, 使用该 commit 但合并到前一个老的 commit 去(常用). 可以用缩写 s 代替 (绿色).
-   <font color="pink">reword</font> reword, 和 pick 类似, 但可以修改 commit 时的提交信息(中间会弹出来让你修改 commit).可以用缩写 r 代替 (紫红色).
-   edit, 使用 commit, 但停下来进行修改, 可能用于 merge 冲突.可以用缩写 e 代替.
-   fixup, 和 squash 类似, 但会舍弃 commit 信息. 可以用缩写 f (红色)
-   exec, 执行 shell 命令.可以用缩写 x


设定完毕,rebase 开始, 接下来会陆续弹出几个 vi 编辑框, 设置 commit 信息


## 典型场景的综合用法

【撤销修改--还未 git add 暂存区】

-   `git checkout -- <file>` or `git checkout <file>` : 让这个文件回到最近一次 git commit 或 git add 时的状态，修改直接撤销了; \* \* `git checkout .` : 丢弃所有文件的修改

【撤销修改--已经 add 暂存区】

-   `git reset [HEAD] <file>` 把指定文件“修改”重新放回工作区(如果没有给出 commit code 如 HEAD, 默认使用 HEAD)&rArr; `git checkout -- <file>`撤销修改
    这两个步骤<=> `git reset --hard [HEAD]` 取消缓存,而且要丢弃工作目录中修改

[撤销 commit 历史]

-   `git log --oneline` 查看需要 reset 到哪个 commit code, 然后`git reset <commit code> --soft`, 此时修改会被撤销到 index 区, 然后提交 `git commit -m "xxx"`或者进行其他操作

【回退到上一个版本--已经 commit 了】
有两个方法, 推荐 git revert

# 远程仓库

[ 查看和远程仓库的连接 ]
`git remote [-v]` -v 表示同时显示 URL 连接

【本地 Git 仓库**关联远程仓库**】
`git remote add origin <remote address>` origin 以后就是远程库名

[替换远程的地址]: `git remote set-url origin https://mirrors.ustc.edu.cn/brew.git`

[ **取消关联远程仓库** ]
`git remote rm/remove <name>`

[ 关联仓库的**更名** ]
`git remote rename <old-name> <new-name>`

【克隆远程仓库】 可以指定本地仓库名
`git clone <remote address> [custom_proj_name]`

【克隆远程指定分支 】
`git clone -b 分支名称 remote地址`

【只克隆最近一次 commit】
`git clone --depth=1 git://someserver/somerepo`

克隆到指定目录
`git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions`

【首次上传项目到远程】  
`git init`初始化成 Git 仓库&rArr;
`git checkout -b branch_name`创建本地分支&rArr;
`git add .`add 所有文件&rArr;
`git commit -m "xxxx"`本地提交&rArr;
`git remote add origin git_address`关联远程库&rArr;
`git push -u origin branch_name`推送，如果 remote 不为空，推送前要拉取：`git pull --rebase origin branch_name`
说明: `-u origin` 表示将当前分支添加为远程跟踪的分支, 之后可以直接使用 `git pull` 来推送

# 分支

- 查看有哪些分支分支

    `git branch [-a]` 查看分支/所有分支

- 切换分支 `git checkout branch_name` 

    【切换分支, 如果没有就创建】: `git checkout -b <new branch name> [base branch name]` 
    
    <=> 先创建分支 `git branch <new branch_name> [base branch name]` => `git checkout branch_name` 切换分支

- 创建分支

    - 创建远程分支 : `git push origin <local_branch>:<remote_branch>` or `git push --set-upstream origin <remote_branch>`  即推送当前分支到远程分支, 远程分支可以不存在

    - 【创建本地分支】`git checkout -b dev `

- 重命名当前分支 

    `git branch -m <branch new name>`

- 删除分支

    - 删除本地分支    `git branch -d branch_name`, 如果是 -D 则是强制删除分支,不管改动是否合并

    - 删除远程分支 

        方式 1: 推送一个空分支到远程分支 <=> 删除远程分支 `git push origin :<remote_branch>`

        方式 2: `git push origin --delete dbg_lichen_star`

- 和远程分支建立联系

    创建同时建立联系 `git checkout -b dev origin/dev`

    建立联系`git branch --set-upstream-to=origin/<remote_branch> <local_branch>`

    【已有分支和远程分支建立联系】~~`git branch --set-upstream <localBranch> origin/<remoteBranch>`~~(已经废弃)

- 推送到远程 push

    push 推送 同时建立联系 `git push --set-upstream origin origin_master` (等价 `git push -u origin local_branch_name`) or `git push origin <remoteBranchName>`

    `git push` 推送当前分支

    [ 推送所有本地分支到远程 ] `git push origin --all`

    [ 推送所有标签到远程 ] `git push <remote> --tags`

    【本地分支推送到远程对应分支】: `git pull`先拉取 => 解决冲突 => `git push -u origin local_branch_name` -u 表示关联那个远程库，后面推送可以简化为 `git push origin local_branch_name`，

- 【拉取远程分支】

    - 方法1: `git pull [remote name] [remoteBranch]` 如果报错，可能是没有建立分支关联，可以：`git branch --set-upstream dev origin/dev`. 效果等同先 fetch, 后 merge

    - 方法2: `git pull --rebase [remote name] [remoteBranch]` <=> 先 fetch, 后将当前分支 rebase 到 fetch 分支(更新的代码总是在最前), 将本地修改放在其他人已经提交的修改之后

    - 方法3:

        - 第一步: `git fetch <remote> [branch]` --- 拉取远程指定分支, 在本地通过`git branch -r`展示, 通常表示有 remotes 前缀. 
        
            然后查看 fetch 到本地的远程分支: `git checkout <remotes/origin/xxx>`, `git log`来查看这些远程分支,如果接受别人在远程分支所做的更改; 如果直接`git checkout <xxx>`,(其中 xxx 是< remotes/origin/xxx>中的 xxx)那么会在本地创建并切换到 xxx 分支

        - 第二步: `git merge <remotes/origin/xxx>` 合并

- 合并分支

    - 方法1: `git merge branch_name` 合并指定分支到当前分支,采用 fast foward 方式，删除分支后会丢掉分支信息

        会产生一个三路合并（3-way merge）和一个合并提交, 不推荐
         
        [**推荐**]的是：`git merge --no-ff -m "commit_info" branch_name`,--no-ff 表示禁用快速合并方式，merge 后会生成一个 commit，-m 表示后面紧跟着提交信息

    - 方法2: `git rebase <target_branch>` [ rebase 当前分支到目标分支最前端, 保持线性提交历史 ]

        第一步: 在需要合并的分支上`git rebase <base>` ---将当前分支 rebase 到 < base >，这里可以是任何类型的提交引用（ID、分支名、标签，或是 HEAD 的相对引用）

        第二步: 切换到主干分支, 正常 merge 

- 【合并冲突分支】: `git merge branch_name`提示有冲突 => `git status`查看冲突 => 修改后再 add，commit

- 【查看分支合并情况】: `git log --graph --pretty=oneline --abbrev-commit`

    git log --oneline

    分支合并情况-详细信息:  `git log --graph --decorate --oneline --simplify-by-decoration --all`

- 【比较两个分支，输出到 text 文件】`git diff <branchName1> <branchName2> >> d:/xxx.text`

- 合并其他分支的部分内容

    `git cherry-pick <commit_id>`

    `git cherry-pick (start_id^..end_id)` ^表示包含头部, 没有 ^ 则不包含头部, 包含尾部

# git diff 打补丁 patch

```sh
# 比较工作区与暂存区
git diff [file...] # 不加参数即默认比较工作区与暂存区所有文件

# 比较暂存区与最新本地版本库（本地库中最近一次commit的内容）
git diff --cached  [<path>...] 

#比较工作区与最新本地版本库 (HEAD 就表示最新的本地版本库)
# 如果HEAD指向的是master分支，那么HEAD还可以换成master
git diff HEAD [<path>...]

#比较工作区与指定commit-id的差异
git diff commit-id  [<path>...] 
#比较暂存区与指定commit-id的差异
git diff --cached [<commit-id>] [<path>...]
#比较两个分支/commit，输出到 text 文件
git diff <branchName1> <branchName2> >> d:/xxx.text



# 生成补丁 (工作区相对于 暂存区的修改)
git diff > patch_name 
# 先检验一下补丁能否应用
git apply --check patch_name # 如果没有任何输出，那么表示可以顺利接受这个补丁
# 打补丁
git apply patch_name

git apply --reject patch # 将能打的补丁先打上，有冲突的会生成.rej文件，此时可以找到这些文件进行手动打补丁

git diff --cached > patch //是将我们暂存区与版本库的差异做成补丁
git diff [--]HEAD > patch //是将工作区与版本库的差异做成补丁
git diff [file...] > patch # 工作区比较暂存区生成补丁

git diff Testfile > patch//将单个文件做成一个单独的补丁 (工作区 and 暂存区)
```




# 暂存-恢复工作区

【bug 分支-储藏工作区】  
修复 bug 需新建一条分支如 issue-001，修复完后合并  
`git stash`将当前工作区内容储存，使工作区看起来是干净的，用于当前工作未完成但是接到新任务，如修复 bug  
【bug 分支-回复工作区】  
`git stash list`查看储藏列表&rArr;`git stash apply [stash_id]`回复全部 stash 内容或指定 ID 的内容&rArr;`git stash drop`删除 stash；简化的命令是：`git stash pop`回复同时删除 stash

【feature 分支-开发新功能】  
添加一个新功能，新建一个 feature 分支，完成后，合并，最后，删除该 feature 分支  
现在如果新功能作废，删除没有合并的分支需要使用强制删除`git branch -D branch_name`

# 标签

类似 branch, 但是不可移动, 就好像有名字的 commit

```sh
# 创建标签
git tag -a v1.0.0 -m "内容：v1.0.0"
# 标签默认打在最新的 commit 上 
git tag <tag_name> [commit_id]

# show all tags
git tag
# 查看某个版本系列的tag
git tag -l 'v1.0.*'
# 查看指定标签的详情，可以看到你commit的内容
git show v0.0.6

# 推送标签
git push origin v1.0.0
# 推送全部标签
git push origin --tags

# 删除
# 先删除本地
git tag -d v1.0.0
# 再删除远程的
git push origin :refs/tags/v1.0.0


```



# .gitignore 文件

https://www.gitignore.io

-   `/aaa/` 过滤文件夹所有内容(以斜杠“/”开头表示根目录下的目录 or 文件)

-   `/aaa/*`: 同上

-   `/*/.settings/` 忽略 任何目录(只有一级)下的 xxx

-   `fd1/*`: 忽略目录目录名为 fd1 下的全部内容; 不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略)

-   `*.zip` 过滤所有.zip 文件

    (以星号“\*”通配多个字符, 以问号“?”通配单个字符, 以方括号“[]”包含单个字符的匹配列表)

-   `/mtk/do.c` 过滤某个具体文件

-   `!*.zip` 指定跟踪所有.zip 文件(以叹号“!”表示不忽略(跟踪)匹配到的文件或目录)

-   `!/mtk/one.txt` 指定跟踪某个具体文件

-   `!**/src/main/**` 指定跟踪任何目录下的 src/main/

git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效

如果文件已经被加入了 git 版本库, 则无法忽略. 此时需要使用命令 `git rm --cached <file>` 在 History 中删除， 这时【删除操作】会显示在 Index 中, 然后 `git commit` 将删除操作归档到 History 中， 则 指定的 file 就完全脱离 version control 了

-   `git rm --cached readme1.txt` 删除 readme1.txt 在版本库中，并保留在 work dir 和 index

-   `git rm --f readme1.txt` 删除 readme1.txt 的跟踪，并且删除本地文件

-   `git rm -r --cached .`　　对所有文件取消跟踪, 保留在本地 和 index 中. -r 表示 递归删除文件夹

-   `git rm -r --f .` 　对所有文件取消跟踪, 删除本地文件

对于取消跟踪的所有文件/文件夹, 使用 `git clean -df [xxx file]` 删除 (-d 表示文件夹, -f 表示强制)

# LF 和 CRLF

https://segmentfault.com/q/1010000011799577
https://www.cnblogs.com/sdgf/p/6237847.html

```sh
#提交时转换为LF，检出时不转换, (input 是对于 git repo 来说的)
# 默认为 true (commit就转为 lf, 检出就转为 crlf)
git config --global core.autocrlf input
#拒绝提交包含混合换行符的文件, 默认 false
git config --global core.safecrlf true

```

IDEA 的设置 File -> Settings -> Editor -> Code Style Line separator (for new lines) ，选择：Unix and OS X (\n)

idea 可以批量转换 crlf: File -> line separators -> ...

# git hooks

husky 

https://githooks.com/

TODO


# git subtree 子仓库

https://segmentfault.com/a/1190000012002151

```
git subtree add   --prefix=<prefix> <commit>
git subtree add   --prefix=<prefix> <repository> <ref>
git subtree pull  --prefix=<prefix> <repository> <ref>
git subtree push  --prefix=<prefix> <repository> <ref>
git subtree merge --prefix=<prefix> <commit>
git subtree split --prefix=<prefix> [OPTIONS] [<commit>]
```

# husky

https://github.com/typicode/husky git 钩子, 在 commit 前做点什么