## GitHub 建仓库
``` shell
echo "# my_configuration" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:luoyueyuguang/my_configuration.git
git push -u origin main
```

``` shell
git remote add origin git@github.com:luoyueyuguang/my_configuration.git
git branch -M main
git push -u origin main
```
## git config
`git config`用来设置git的某些配置
## Git Basics 
### 获取仓库
- 初始化目录
	`git init`
- 实现跟踪
	`git add`
- 提交
	`git commit`

### 克隆仓库
- clone 
`git clone [url]`
- 重命名目录
`git clone [url] repo`

### *git文件的生命周期*
![[Pasted image 20240422215022.png]]

### 检查文件状态
- `git status`
- 状态简览`git status -s / git status --short
### 忽略文件
- 使用.gitignore
	1. 可以使用标准的glob模式匹配
	2. /开头可以防止递归
	3. / 结尾指定目录
	4. 开头加!取反
### 查看修改
- `git diff`比较目录与暂存区域快照的差异
- `git diff --cached / git diff --staged` 比较已暂存与已提交
### 提交更新
- `git commit` 默认使用`$EDITOR`来查看，也可通过`git config --global core.editor`来设置
- `git commit -m "info"`将提交信息和命令放在同一行
- `git commit -a`跳过暂存步骤，将所有跟踪过的文件暂存并提交
### 移除文件
- `rm file`file出现在未暂存清单中
- `git rm file`file不再纳入版本管理
- `git rm --cached file`保留file但不让git跟踪
- 使用`glob`模式
	`git rm log/\*.log` `*`前面的`\`是因为Git有它自己的文件匹配模式， 不能让`shell`帮忙展开
### 移动文件
- `git mv src dst`在*Git*中对文件改名
	等价于
	```
	mv src dst
	git rm src
	git add dst
	```
### 查看提交历史
- `git log` 显示所有
- `git log -p`显示每次提交差异
	如`git log -p -2`*-2*表示最近两次
- `git log --stat`查看每次提交的简略信息
- `git log --pretty`指定不同于使用默认格式的方式来展示提交历史
	- `git log --pretty=oneline`，`oneline`可以换成`short`、`full`、`fuller`
	- `git log --pretty=format:"%h - %an, %ar : %s"`

| --pretty=format选项 | 说明                         |
| ----------------- | -------------------------- |
| `%H`              | 提交对象（commit）的完整哈希字串        |
| `%h`              | 提交对象的简短哈希字串                |
| `%T`              | 树对象（tree）的完整哈希字串           |
| `%t`              | 树对象的简短哈希字串                 |
| `%P`              | 父对象（parent）的完整哈希字串         |
| `%p`              | 父对象的简短哈希字串                 |
| `%an`             | 作者（author）的名字              |
| `%ae`             | 作者的电子邮件地址                  |
| `%ad`             | 作者修订日期（可以用 --date= 选项定制格式） |
| `%ar`             | 作者修订日期，按多久以前的方式显示          |
| `%cn`             | 提交者（committer）的名字          |
| `%ce`             | 提交者的电子邮件地址                 |
| `%cd`             | 提交日期                       |
| `%cr`             | 提交日期，按多久以前的方式显示            |
| `%s`              | 提交说明                       |
- `git log --pretty=* --graph`添加ASCII来进行形象展示

| `git log`常用选项     | 说明                                                                 |
| ----------------- | ------------------------------------------------------------------ |
| `-p`              | 按补丁格式显示每个更新之间的差异。                                                  |
| `--stat`          | 显示每次更新的文件修改统计信息。                                                   |
| `--shortstat`     | 只显示 --stat 中最后的行数修改添加移除统计。                                         |
| `--name-only`     | 仅在提交信息后显示已修改的文件清单。                                                 |
| `--name-status`   | 显示新增、修改、删除的文件清单。                                                   |
| `--abbrev-commit` | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。                                     |
| `--relative-date` | 使用较短的相对时间显示（比如，“2 weeks ago”）。                                     |
| `--graph`         | 显示 ASCII 图形表示的分支合并历史。                                              |
| `--pretty`        | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。 |
- 关于`git log` 的输出

|选项|说明|
|---|---|
|`-(n)`|仅显示最近的 n 条提交|
|`--since`, `--after`|仅显示指定时间之后的提交。|
|`--until`, `--before`|仅显示指定时间之前的提交。|
|`--author`|仅显示指定作者相关的提交。|
|`--committer`|仅显示指定提交者相关的提交。|
|`--grep`|仅显示含指定关键字的提交|
|`-S`|仅显示添加或移除了某个关键字的提交|
### 撤消操作
- `git commit --amend`重新提交
- `git reset HEAD <file>`取消暂存区的文件，加入`--hard`会修改工作目录文件
- `git checkout -- [file]`将file回至上一次commit的状态
### 查看远程仓库
- `git remote -v`　展示远程仓库简写及其url
- `git remote add <shortname> <url>`添加新的远程仓库
- `git fetch [remote-name]` 拉取远程仓库但不做任何操作
- `git push [remote-name] [branch-name]`推送到远程服务器上游分支
- `git remote show [remote-name]`显示远程仓库更多信息
- `git remote rename old new`重命名远程仓库简写名
- `git remote rm [remote-name]`移除远程仓库
### 打标签
- `git tag`列出标签　
- `git tag -l 'some'`显示有关some的tag，可以使用模式匹配
- `git tag -a <tagname> -m <msg>`创建一个附注标签
- `git show <tagname>`查看标签及其提交信息
- `git push [remote-name] [tagname]`传送标签到远程服务器上
- `git push [remote-name] --tags`传送所有不在服务器上的tag
- `git checkout -b [branchname] [tagname]`在特定标签上创建一个新分支
### Git别名
- `git config --global alias.anme 'oname'` 
	- example：取消暂存的别名
		`git config --global alias.unstage 'reset HEAD --'`
	- `git config --global alias.visual '!gitk'` **!** 可用于执行外部命令
## Git Branching
### 分支基础
### 分支新建与合并
- `git branch [branch-name]`创建新分支，实质是在当前提交对象上创建一个分支
- `git checkout testing`切换分支
- `git checkout -b [branch-name]`创建分支并切换到那个新分支
	　等价于
	``` shell
	git branck [branch-name]
	git checkout [branch-name]
	```
- `git branch -d [branch-name]`删除分支
- `git merge [branch-name]`合并分支，合并完后commit
### 分支冲突时的合并
1. 处理完冲突内容
2. `git merge`
3. `git commit`
	*important refer*
	- https://www.progit.cn/#_%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6
	- https://www.bilibili.com/video/BV1di421Z7qN/?spm_id_from=333.337.search-card.all.click&vd_source=4a7e14c600e846de3987c26c033c3016
### 分支管理
- `git branch`得到分支列表
- `git branch -v`查看每个分支的最后一个提交
- `git branch --merged`查看已合并分支
- `git branch --no-merged`查看未合并分支
- `git branch -D [branch-name]`强制删除未合并的分支
### 分支开发工作流
***https://www.progit.cn/#_%E5%88%86%E6%94%AF%E5%BC%80%E5%8F%91%E5%B7%A5%E4%BD%9C%E6%B5%81***
- 长期分支
- 特性分支
### 远程分支
***https://www.progit.cn/#_remote_branches***
- `git remote add name url`添加远程分支
- `git push (remote) (branch)`推送本地分支到同名的远程分支
- `git push [remote] [remote-branch]:local-branch]`推送本地分支到远程分支
- `git checkout -b　local-branch remote-branch`建立一个基于远程分支的本地分支
- `git checkout --track [remotename]/[branch]`跟踪仓库上的远程分支
- `git branch -u [remotename]/[branch]`设置上游分支
- `git branch -vv`列出更详细的本地分支信息
- `git pull`相当于`git fetch`加`git merge`,比较魔法，慎用
- `git push [remote] --delete [branch]`删除远程分支*只是删除指针，git服务器会保存数据直至垃圾回收*
### 变基
***https://www.progit.cn/#_rebasing***
*流程*
``` shell
git checkout exp　//切换分支
git rebase base　//以基分支开始变基
git checkout base //换回基分支
git merge exp　//开始合并
```
变基后提交历史会是线性的，这种办法有利有弊
- `git rebase --onto master server client`取出 `client` 分支，找出处于 `client` 分支和 `server` 分支的共同祖先之后的修改，然后把它们在 `master` 分支上重放一遍
- `git rebase [basebranch] [topicbranch]`将`topicbranch`变基到`basebranch`上
- `git pull --rebase`即`git pull`然后`git rebase`


## 分布式Git
### 分布式Git工作流
- 集中式工作流
- 集成管理者工作流
- 司令官与副官工作流
- `git diff --check`解决空白问题
- commit的模板
```
Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase will confuse you if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent
```
- `git merge --squash branch`合并分支
- `git request-pull [remote_branch] [local_branch]`
### 通过邮件的项目
***Documentation/SubmittingPatches in Git source code***有更详细的邮件指令
- `git format-path [branch]`生成commit的patch,加`-<n>`可以指定最近几次
- `diff`也可以生成补丁
- `imap`区块设置，在`~/.gitconfig`下
```
[imap]
  folder = "[Gmail]/Drafts"
  host = imaps://imap.gmail.com
  user = user@gmail.com
  pass = p4ssw0rd
  port = 993
  sslverify = false
```
- `cat *patch | git imap-send`发送补丁序列到imap服务器的草稿文件夹中
- `smtp`区块设置
``` 
[sendemail]
  smtpencryption = tls
  smtpserver = smtp.gmail.com
  smtpuser = user@gmail.com
  smtpserverport = 587
```
- `git send-email *.patch`发送补丁
- `git apply patch`应用补丁
- `git apply --check patch`检查补丁是否可以顺利应用
- `git am patch`应用`format-patch`来生成`patch`才可以
- `git am --resolved`解决完冲突后继续运用下一个补丁
- `git am -3 *.patch`尝试三方合并
- `git am -i *.patch`进入交互模式
- `git merge-base b1 b2`找出两个分支的公共祖先
- `git diff b1...b2`三点语法 ,比较b2与(b1和b2的公共)的区别
- ***[关于三点和两点](https://doc.devpod.cn/git/git-log-17104903.html)*** 
### 合并contribution
- 合并工作流
- 大项目合并工作流
- 变基与拣选工作流
- `git cherry-pick commit-hash`将提交拉取到当前分支。
- `git rerere` *reuse recorded resolution* 重用已记录的解决方案,可以通过`git config --global rerere.enabled true`设置为True,自动启用。
- 为发布打标签和给出公钥
- `git describe`给出提交的描述
- `git shortlog`给出提交简报
`
## GitHub
### ***[GitHub](https://www.progit.cn/#_github)***
### Octokit
***[OCTOkit(Official clients for the GitHub API)]([http://github.com/octokit](http://github.com/octokit))***
### ***[GitHub docs](https://docs.github.com/en)***

## Git Tools
### 选择修订版本
- `sha1` git提供简短的sha1值来选择
- 在Git中,可以用分支名代表最近的提交
- `git rev-parse branch`查看分支指向的sha1
- `git reflog`查看引用日志
- `git show ref@{*}`使用类似的格式来查看引用的提交记录
- 在引用尾部后加上`^`会查看引用的父提交
	- example:`git show HEAD^`展示HEAD的父提交
	- example:`git show d921970^2`展示d921970的第二父提交(仅适合merge的分支)
- 在引用后加上`~`也可以查看引用的父提交，但`~`后加数字有很大的不同
	- example:`git show HEAD~2`相当于`git show HEAD^^`即git的父提交的父提交
- `git log b1..b2`展示在b2分支而不在b1分支中的提交,**两点语法**
- 可以使用`^`和`--not`,表示非的意思
	example
``` bash
git log refA..refB
git log ^refA refB
git log refB --not refA
git log refA refB ^refC
git log refA refB --not refC
```
- `git log b1...b2`展示b1和b2包含但不是两者公有的提交
### 交互式暂存
***[交互式暂存](https://www.progit.cn/#_interactive_staging)***
- `git add -i`通过加入`-i`参数,进入暂存模式
### 储藏与清理
- `git stash/git stash save`不提交，储藏自己的修改
- `git stash list`查看储藏的东西
- `git stash apply`应用储藏
- `git stash apply --index`应用储藏的同时也将索引应用
- `git stash --keep-index`不储藏任何通过`git add`命令添加的东西
- `git stash -u` 储藏未跟踪文件
- `git stash --patch`给出交互式提示
- `git stash branch name`根据储藏创建新分支
- `git stash --all`移除每一样东西并存放在栈中
***
- `git clean -d -n`清除冗余文件或者清理工作目录
- `git clean -d -n`做一次模拟告诉将要删除什么
- `git clean -f -d` 强制删除
- `git clean -n -d -x`使用`-x`删除`.gitignore`中忽略的文件
- `git clean -i`进入交互模式
### 签署工作
- `gpg --list-keys`查看GPG密钥
- `gpg --gen-key`生成GPG密钥
- `git config --global user.signingkey　[gpg-key]`设置git签署的私钥
***
- `git tag -s <tag> -m 'info`用`-s`替代`-a`来用私钥签署标签
- `git tag -v [tag-name]`验证签名
- `git commit -a -S -m 'signed commit'`使用`-S`来签署提交
- `git log --show-signature`查看签名
- `git pull/merge --verify-signatures branch`检查并拒绝没有携带可信 GPG 签名的提交
- `git merge --verify-signatures -S  signed-branch`　`-S`选项用来签署属于自己生成的合并提交
### 搜索
- `git grep -n [pattern]`显示行号
- `git grep --count [pattern]`显示哪些文件包含匹配以及每个文件包含了多少个匹配
- `git grep -p [pattern] [file]`查看匹配的行属于哪个方法或函数
- `git grep --break --heading -n -e '#define' --and \( -e LINK -e BUF_MAX \) v1.8.0` `--and`可以进行多个匹配,`--break` 和 `--heading` 选项使输出更加容易阅读
***
- `git log -SZLIB_BUF_MAX --oneline`使用 `-S` 选项来显示新增和删除该字符串的提交
- `git log -L :func:file`展示代码中一行或者一个函数的历史,函数可以使用正则表达式来代替
### 重写历史
- `git commit --amend`修改最后一次提交
- ***git中可以使用变基工具来变基一系列提交***
- `git rebase -i [commit]`进入交互模式
- ***交互式变基流程***
	1. 通过`git rebase -i [commit]`看到提交脚本
	2. 修改提交脚本
	3. 确认,剩下的可以根据Git提示来
***[Git的rebase交互模式来重写历史](https://www.progit.cn/#_rewriting_history)***
### 重置揭秘
#### 三棵树
***[关于三棵树的详解](https://www.progit.cn/#_git_reset)***
Git实际上就是通过管理三棵树来进行一系列工作的

| 树                 | 用途                 |     |
| ----------------- | ------------------ | --- |
| HEAD              | 上一次提交的快照，下一次提交的父结点 |     |
| Index             | 预期的下一次提交的快照        |     |
| Working Directory | 沙盒                 |     |
|                   |                    |     |
- `git reset --soft [commit]`移动HEAD的指向
- `git reset --mixed [commit]`默认为`mixed`,相比于soft更新了Index
- `gi reset --hard　[commit]`相比于`mixed`,更新了工作目录,非常危险，因为强制覆盖了工作目录.
- `git reset hash file`拉取文件的对应版本
- 利用`git reset`可以实现压缩提交的目的,即先`git reset --soft`回退,再`git commit`
***
- `git checkout`也是靠操纵三棵树来实现的
-  `git checkout [branch]` 与运行 `git reset --hard [branch]` 非常相似
- `checkout` 对工作目录是安全的，它会通过检查来确保不会将已更改的文件吹走。 会在工作目录中先试着简单合并一下，这样所有*还未修改过的*文件都会被更新。 而 `reset --hard` 则会不做检查就全面地替换所有东西。
- `reset` 会移动 HEAD 分支的指向，而 `checkout` 只会移动 HEAD 自身来指向另一个分支。
- `git checkout file`从index中回复file
- `git　checkout`也接受`--patch`选项

| HEAD                       | Index | Workdir | WD Safe? |        |
| -------------------------- | ----- | ------- | -------- | ------ |
| **Commit Level**           |       |         |          |        |
| `reset --soft [commit]`    | REF   | NO      | NO       | YES    |
| `reset [commit]`           | REF   | YES     | NO       | YES    |
| `reset --hard [commit]`    | REF   | YES     | YES      | **NO** |
| `checkout [commit]`        | HEAD  | YES     | YES      | YES    |
| **File Level**             |       |         |          |        |
| `reset (commit) [file]`    | NO    | YES     | NO       | YES    |
| `checkout (commit) [file]` | NO    | YES     | YES      | **NO** |
### 高级合并
- `git merge --abort`中断合并
- `git merge -Xignore-space-change whitespace`忽略空白
***
- `git`可以手动处理文件再进行合并
1. `git show :1:hello.rb > hello.common.rb`可以先通过`git show`来释放一份拷贝,1为共同祖先,2为基版本,3为要合并的版本
2. 修改文件
3. `git merge-file -p hello.ours.rb hello.common.rb hello.theirs.rb > hello.rb`使用`git merge-file`来重新合并
***
- `git checkout --conflict=diff3/merge file`选择展示冲突的样式
- `git checkout --ours/--theirs`快速合并,选择一边
- `git log --oneline --left-right HEAD...MERGE_HEAD`
- `git log --oneline --left-right --merge`
***
- `git revert -m 1 HEAD`还原提交,实质上是生成一个新提交
- `git merge -Xours/-Xtheirs branch`合并其中一方作为最终结果
- `git merge -s ours branch`将自己的分支作为最终结果
***
 - `git read-tree --prefix=dir/ -u branch`拉取一个分支到自己的目录中
- `git merge --squash -s recursive -Xsubtree=rack rack_branch`采用递归合并策略来更新子目录
- `git diff-tree -p rack_branch`比较子目录和分支的差异
### Rerere
- `git rerere status`记录合并状态
- `git rerere diff`显示解决前与解决后
### 使用Git来进行调试
- `git blame file`追踪文件
- `git blame　-L s,e file`限制输出行数
- `git blame　－Ｃ　-L s,e file` `-C`参数用来分析原始出处
***
- `git bisect start`开启二分查找
- `git bisect bad`告诉系统提交有问题
- `git bisect good [good_commit]`告诉bisect目前最正常的提交
- `git bisect good`告诉系统提交没问题
- `git bisect reset`重置HEAD指针到最开始
- `git bisect start [good-commit] [bad-commit]`设置正常与不正常提交
- `git bisect run [sh]`执行测试脚本
- `git bisect`进行二分查找流程
	1. 找到好提交与坏提交
	2. 进入中间提交
	3. 标记中间提交
	4. 二分,回到2
### 子模块
- `git submodule add [remote-url] [optional-name]`添加子模块,最后可以自己命名子模块
- 子模块有关保存着在`.gitmodules`文件中
- `git diff --cached --submodule`查看漂亮的差异输出
- `git submodule init`初始化本地配置文件
- `git submodule update` 从项目中抓取所有数据并检出父项目中列出的合适的提交
- `git clone --recursive [remote-url]`自动初始化并更新仓库中的每一个子模块
- 进入子模块目录,`git fetch`然后`git merge`来更新代码，用于进行子模块的更新
- `git submodule update --remote`，Git 将会进入子模块然后抓取并更新
- `git config -f .gitmodules submodule.[name].branch [branch]`设置子模块默认分支
- `git config status.submodulesummary 1`显示子模块的更改摘要
- `git log -p --submodule`查看子模块提交日志
- `git submodule update --remote --rebase`合入子模块上游分支到本地
- `git submodule update --remote --merge`合入子模块上游分支到本地
- `git push --recurse-submodules=check`子模块改动如果没有推送则会`push`失败
- `git push --recurse-submodules=on-demand`尝试推动子模块
***
- 子模块冲突解决方案
1. 首先解决冲突
2. 然后返回到主项目目录中
3. 再次检查 SHA-1 值
4. 解决冲突的子模块记录
5. 提交我们的合并
***
- `git submodule foreach 'command'`在每一个子模块中运行任意命令
- 如果创建一个新分支，在其中添加一个子模块，之后切换到没有该子模块的分支上时，仍然会有一个还未跟踪的子模块目录
- 移除它然后切换回有那个子模块的分支，需要运行 `git submodule update --init` 来重新建立和填充
### 打包
 - `git bundle create [bundle-name] [commit-list]`创立bundle包
- `git clone repo.bundle repo`从bundle包克隆
- `git bundle verify bundle`检查是否合法包以及是否可导入
- `git bundle list-heads bundle`列出顶端
- `git fetch ../commits.bundle [bundle-branch]:[my-branch]`导入bundle包中的分支
- `git replace old-commit new-commit`替换提交
### 凭证存储
***[Git凭证存储](https://www.progit.cn/#_credential_caching)***
``` bash
[credential]
    helper = store --file /mnt/thumbdrive/.git-credentials
    helper = cache --timeout 30000
```
## 自定义Git
- `git config --global user.name "name"`
- `git config --global user.email *@*.com`
- Git配置文件找寻路径
	1. `/etc/gitconfig`给`git config`传递`--system`会读写
	2. `~/.gitconfig`或`~/.config/git/config`传递`--global`会读写
	3. `.git/config`只对当前版本库有效
- `git config --global core.editor editor`配置默认编译器
- `git config --global commit.template file`设置提交模板
- `git config --global core.pager ''`设置分页器
- `git config --global user.signingkey <gpg-key-id>`设置密钥
- `git config --global core.excludesfile file`设置忽略文件的模板
- `git config help.autocorrect time`给出$\dfrac{1}{10}$time来做出反应是否应用自动修正的命令
- `git config --global color.ui *`配置终端输出着色
- `color.*`git可以对不同的命令配置是否着色
- 还有子选项用来输出各个部分着色,例:`git config --global color.diff.meta "blue black bold"`
***
可以使用其他的合并与比较工具
- `git mergetool --tool-help`查看支持哪些工具
- 使用`git config`来配置
***
- `git config --global core.autocrlf true`在Win上,将
- `git config --global core.autocrlf input`在Linux和Mac上，告诉 Git 在提交时把回车和换行转换成换行，检出时不转换
- git默认被打开的三个选项是：`blank-at-eol`，查找行尾的空格；`blank-at-eof`，盯住文件底部的空行；`space-before-tab`，警惕行头 tab 前面的空格.默认被关闭的三个选项是：`indent-with-non-tab`，揪出以空格而非 tab 开头的行（你可以用 `tabwidth` 选项控制它）；`tab-in-indent`，监视在行头表示缩进的 tab；`cr-at-eol`，告诉 Git 忽略行尾的回车
 - `git config --global core.whitespace sth`设置空格处理
- `git apply --whitespace=warn <patch>`Git 在应用补丁时发出警告
- `git apply --whitespace=fix <patch>自动修复
- 这两个选项也能用于rebase
***
- `git config --system receive.fsckObjects true`服务器端Git 能够确认每个对象的有效性以及 SHA-1 检验和是否保持一致
- `git config --system receive.denyNonFastForwards true`禁用强制更新(push -f诸如)
- `git config --system receive.denyDeletes true`禁止通过推送删除分支和标签
***
- `.gitattributes`或`.git/info/attributes`中是设置
- 可以通过配置Git属性来比较二进制文件
- git可以编写自己的过滤器来实现文件提交或检出时的关键字替换
- `“smudge”`过滤器会在文件被检出时触发
- `“clean”`过滤器会在文件被暂存时触发
***
- 可以增加一些关键字到Git属性文件中去
- `export-ignore`不归档,例:`test/ export-ignore`
- `export-subst`可以将`git log` 的格式化和关键字展开处理应用于被 `export-subst` 属性标记的部分文件
***
- GIt可以通过属性文件设置合并策略,l例:`database.xml merge=ours` `git config --global merge.ours.driver true`
***
- 钩子都被存储在 Git 目录下的 `hooks` 子目录中。 也即绝大部分项目中的 `.git/hooks`,把一个正确命名且可执行的文件放入 Git 目录下的 `hooks` 子目录中，即可激活该钩子脚本
- ***[Git钩子](https://www.progit.cn/#_git_hooks)***
## Git与其他系统
- 　[GIt与其他系统](https://www.progit.cn/#_git_hooks`)
- 看文档
## Git内部原理
***[Git内部原理](https://www.progit.cn/#_git_internals)***
- `.git文件夹`
``` bash
HEAD
config*
description
hooks/
info/
objects/
refs/
```
- `description` 文件仅供 GitWeb 程序使用
 - `config` 文件包含项目特有的配置选项
 - `info` 目录包含一个全局性排除（global exclude）文件
 - `hooks` 目录包含客户端或服务端的钩子脚本（hook scripts）
 -  `objects` 目录存储所有数据内容
 - `refs` 目录存储指向数据（分支）的提交对象的指针
 - `HEAD` 文件指示目前被检出的分支
 - `index` 文件保存暂存区信息
***
- `git hash-object`可将任意数据保存于 `.git` 目录，并返回相应的键值,`-w`存储数据对象,`--stdin`从标准输入读取内容
 - `git cat-file`从Git取回文件内容,`-p`可自动判断,`-t`告诉对象类型
- `git update-index --add --cacheinfo file-type object-hash file`添加文件并创建暂存区,创建树对象
- `git write-tree`将暂存区内容写入树对象
- `git read-tree --prefix=dir hash`将一个已有的树对象作为子树读入暂存区
- `git commit-tree hash`创建commit树对象,`-p`指定父commit,example:`echo 'second commit' | git commit-tree 0155eb -p fdf4fc3`
***
- git头部信息为类型＋空格＋数据内容长度＋空字节
- Git 会将上述头部信息和原始数据拼接起来，并计算出这条新内容的 SHA-1 校验和
- Git 会通过 zlib 压缩这条新内容
- 写入`'.git/objects/' + sha1[0,2] + '/' + sha1[2,38]`,ruby演示如下
``` ruby
$ irb
>> content = "what is up, doc?"
=> "what is up, doc?"
>> header = "blob #{content.length}\0"
=> "blob 16\u0000"
>> store = header + content
=> "blob 16\u0000what is up, doc?"
>> require 'digest/sha1'
=> true
>> sha1 = Digest::SHA1.hexdigest(store)
=> "bd9dbf5aae1a3862dd1526723246b20206e5fc37"
>> require 'zlib'
=> true
>> zlib_content = Zlib::Deflate.deflate(store)
=> "x\x9CK\xCA\xC9OR04c(\xCFH,Q\xC8,V(-\xD0QH\xC9O\xB6\a\x00_\x1C\a\x9D"
>> path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]
=> ".git/objects/bd/9dbf5aae1a3862dd1526723246b20206e5fc37"
>> require 'fileutils'
=> true
>> FileUtils.mkdir_p(File.dirname(path))
=> ".git/objects/bd"
>> File.open(path, 'w') { |f| f.write zlib_content }
=> 32
```
***
- 引用(refs):一个文件来保存 SHA-1 值，并给文件起一个简单的名字，然后用这个名字指针来替代原始的 SHA-1 值
- refs目录
``` bash
$ ls .git/refs/ -F1
heads/
tags/
```
- `git update-ref <ref> hash`更新某个引用
- `git update-ref refs/heads/[new-branch] commit-hash`在某个提交上创建hash
- `git symbolic-ref HEAD`查看 HEAD 引用对应的值
- `git symbolic-ref HEAD ref`设置 HEAD 引用的值
***
- Git 最初向磁盘中存储对象时所使用的格式被称为“松散（loose）”对象格式。 但是，Git 会时不时地将多个这些对象打包成一个称为“包文件（packfile）”的二进制文件，以节省空间和提高效率
- `git gc`手动打包
- `git verify-pack -v *.idx`查看已打包内容
***
- 引用规格的格式由一个可选的 `+` 号和紧随其后的 `<src>:<dst>` 组成，其中 `<src>` 是一个模式（pattern），代表远程版本库中的引用；`<dst>` 是那些远程引用在本地所对应的位置。 `+` 号告诉 Git 即使在不能快进的情况下也要（强制）更新引用
- 不能在模式中使用部分通配符,但可以使用命名空间（或目录）来达到类似目的
```bash
#invalid
fetch = +refs/heads/qa*:refs/remotes/origin/qa*

#valid
[remote "origin"]
	url = https://github.com/schacon/simplegit-progit
	fetch = +refs/heads/master:refs/remotes/origin/master
	fetch = +refs/heads/qa/*:refs/remotes/origin/qa/*
```
- 可以配备`push`值
 `git push branch :<dst>`删除远程服务器引用,将`<src>`留空，就是在删除`<dst>`
***
-  ***[git传输协议](https://www.progit.cn/#_%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)***
- `git gc --auto`执行自动垃圾回收,修改 `gc.auto` 与 `gc.autopacklimit` 的设置来改动`gc`
- `git reflog`了解曾经做过什么
- `git branch [new-branch] [commit-hash]`创建新分支指向某个提交
- `git fsck --full`检查数据完整性
***
 - `git rev-list --objects --all`列出所有提交的 SHA-1、数据对象的 SHA-1 和与它们相关联的文件路径
- `git prune --expire now`完全移除某些对象
***
#### 环境变量
- 像通常的程序一样，Git 的常规行为依赖于环境变量
- **`GIT_EXEC_PATH`**子程序路径,`git --exec-path`可查看
- **`GIT_CONFIG_NOSYSTEM`**禁用系统级别的配置文件
- **`GIT_PAGER`**显示多页输出的程序，没有设置，就会用 `PAGER`
- **`GIT_EDITOR`**编辑器。 如果没设置，就会用 `EDITOR`
- **`GIT_DIR`** 是 `.git` 目录的位置. 没有会按照目录树逐层向上查找 `.git` 目录，直到到达 `~` 或 `/`
- **`GIT_CEILING_DIRECTORIES`** 控制查找 `.git` 目录的行为
- **`GIT_WORK_TREE`** 是非空版本库的工作目录的根路径 没指定就用 `$GIT_DIR` 的父目录
- **`GIT_INDEX_FILE`** 是索引文件的路径（只有非空版本库有）
- **`GIT_OBJECT_DIRECTORY`** 用来指定 `.git/objects` 目录的位置
- **`GIT_ALTERNATE_OBJECT_DIRECTORIES`** 一个冒号分割的列表 (格式类似 `/dir/one:/dir/two:…`) 用来告诉 Git 到哪里去找不在 `GIT_OBJECT_DIRECTORY` 目录中的对象. 如果你有很多项目有相同内容的大文件，这个可以用来避免存储过多备份。
- **`GIT_GLOB_PATHSPECS` and `GIT_NOGLOB_PATHSPECS`** 控制通配符在路径规则中的默认行为
- **`GIT_LITERAL_PATHSPECS`** 禁用上面的两种行为
- **`GIT_ICASE_PATHSPECS`** 让所有的路径规格忽略大小写
- **`GIT_AUTHOR_NAME`** 是 “author” 字段的可读的名字。
- **`GIT_AUTHOR_EMAIL`** 是 “author” 字段的邮件。
- **`GIT_AUTHOR_DATE`** 是 “author” 字段的时间戳。
- **`GIT_COMMITTER_NAME`** 是 “committer” 字段的可读的名字。
- **`GIT_COMMITTER_EMAIL`** 是 “committer” 字段的邮件。
- **`GIT_COMMITTER_DATE`** 是 “committer” 字段的时间戳。
- **`GIT_CURL_VERBOSE`** 告诉 Git 显示所有由curl库产生的消息
- **`GIT_SSL_NO_VERIFY`** 告诉 Git 不用验证 SSL 证书
- 如果 Git 操作在网速低于 **`GIT_HTTP_LOW_SPEED_LIMIT`** 字节／秒，并且持续 **`GIT_HTTP_LOW_SPEED_TIME`** 秒以上的时间，Git 会终止那个操作。 这些值会覆盖 `http.lowSpeedLimit` 和 `http.lowSpeedTime` 配置的值。
- **`GIT_HTTP_USER_AGENT`** 设置 Git 在通过 HTTP 通讯时用到的 user-agent。 默认值类似于 `git/2.0.0`
- **`GIT_DIFF_OPTS`**　用来控制在 `git diff` 命令中显示的内容行数。
- **`GIT_EXTERNAL_DIFF`** 用来覆盖 `diff.external` 配置的值
- **`GIT_DIFF_PATH_COUNTER`** 和 **`GIT_DIFF_PATH_TOTAL`** 对于 `GIT_EXTERNAL_DIFF` 或 `diff.external` 指定的程序有用。 前者表示在一系列文件中哪个是被比较的（从 1 开始），后者表示每批文件的总数。
- **`GIT_MERGE_VERBOSITY`** 控制递归合并策略的输出
- **`GIT_TRACE`** 控制常规跟踪，它并不适用于特殊情况。 它跟踪的范围包括别名的展开和其他子程序的委托
- **`GIT_TRACE_PACK_ACCESS`** 控制访问打包文件的跟踪信息 第一个字段是被访问的打包文件，第二个是文件的偏移量
- **`GIT_TRACE_PACKET`** 打开网络操作包级别的跟踪信息
- **`GIT_TRACE_PERFORMANCE`** 控制性能数据的日志打印
- **`GIT_TRACE_SETUP`** 显示 Git 发现的关于版本库和交互环境的信息
- 如果指定了 **`GIT_SSH`**， Git 连接 SSH 主机时会用指定的程序代替 `ssh`
- **`GIT_ASKPASS`** 覆盖了 `core.askpass` 配置
- **`GIT_NAMESPACE`** 控制有命令空间的引用的访问
- **`GIT_FLUSH`** 强制 Git 在向标准输出增量写入时使用没有缓存的 I/O
- **`GIT_REFLOG_ACTION`** 让你可以指定描述性的文字写到 reflog 中

## Think about Git
### what is git
git的核心在于分布式，git是多仓库的。
git的核心在于那在于那个状态变化周期.
使用git的命令可以实现不少操作