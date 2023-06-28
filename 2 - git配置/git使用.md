# Git 安装、使用、一般开发流程

## 1. 安装以及环境配置

参考以下链接：

http://git.aubo-robotics.cn:8001/lw/git_training

## 2. Git 常用指令

```plaintext
# 在本地用户(root和user是两个用户)设置自己gitlab账户的用户名和密码,${GIT_USER_NAME}替换为自己的gitlab账户名,${GIT_USER_PASSWD}替换为自己的gitlab账户密码(用于CPMAddPackage克隆指定的仓库)
git config --global credential.helper store
echo "http://${GIT_USER_NAME}:${GIT_USER_PASSWD}@git.aubo-robotics.cn:8001" > ~/.git-credentials 

# 创建仓库 用于把当前所在目录（如果你使用 Windows 系统，为了避免遇到各种莫名其妙的问题，请确保各级目录名不包含中文）变成 git 可以管理的仓库
git init 

# 添加文件 git add
# 添加一个或多个文件到暂存区
git add [file1] [file2] ...

# 添加指定目录到暂存区，包括该目录中的子目录
git add [dir]

# 同时添加文件和目录：
git add [dir] [file]

# 添加当前目录下的所有文件到暂存区：
git add .


# 删除文件 git rm 
# 用于删除工作区文件，并且将这次删除放入暂存区，相当于 rm 指令与 git add 两条指令的叠加。
# 文件已提交至仓库，且工作区和暂存区均无更改
# 工作区和暂存区均删除文件
git rm [file1] [file2] ...

# 工作区和暂存区均删除整个目录
git rm -r [dir]

# 工作区和暂存区均删除所有文件和目录 
git rm -r *

# 文件已提交至仓库，工作区或暂存区有更改
#  - 工作区保留、暂存区删除文件，同时被删除项变为未跟踪状态
git rm --cached [file1] [file2] ...

#  - 工作区保留、暂存区删除整个目录，同时被删除项变为未跟踪状态
git rm --cached -r [dir]

#  - 工作区保留、暂存区均删除所有文件和目录，同时被删除项变为未跟踪状态
git rm -r *

#  - 工作区和暂存区均删除文件
git rm -f [file1] [file2] ...

#  - 工作区和暂存区均删除整个目录
git rm -fr [dir]

#  - 工作区和暂存区均删除所有文件和目录
git rm -fr *

# 文件已添加至暂存区
#  - 工作区保留、暂存区删除文件，同时文件变为未跟踪状态
git rm --cached [file1] [file2] ...

#  - 工作区保留、暂存区删除整个目录，同时目录中的所有文件变为未跟踪状态
git rm --cached -r [dir]

#  - 工作区保留、暂存区均删除所有文件和目录，同时被删除项变为未跟踪状态
git rm -r *

#  - 工作区和暂存区均删除文件
git rm -f [file1] [file2] ...

#  - 工作区和暂存区均删除整个目录
git rm -fr [dir]

#  - 工作区和暂存区均删除所有文件和目录
git rm -fr *

# 注意：未跟踪的文件不能使用 git rm 来删除

# 提交,将暂存区内容提交至仓库中
# 将暂存区所有内容提交至仓库，message 是备注信息
git commit -m [message]

# 将暂存区指定文件提交至仓库，message 是备注信息
git commit [file1] [file2] ... -m [message]

# 文件已经提交至仓库，再次修改文件后不需要执行 git add 命令，直接提交至仓库
git commit -am [message]

# 编辑多行提交信息
git commit -a

# 修改提交 
# 用于修改最后一次的提交信息或提交内容，commit id 会改变，用法详见下文"修改最近一次提交内容"的的方法一。
git commit --amend


# 查看状态 
# 用于查看在你上次提交之后是否有对文件进行再次修改，当前仓库状态信息包含：
#     当前所在分支
#     有未跟踪的文件时会给出提示
#     有已提交至仓库中的文件再次被修改时会给出提示
git status


# 推送分支
# 一般格式
git push <远程主机名> <本地分支名>:<远程分支名>

# 若本地分支推送到与之存在追踪关系的远程分支（通常两者同名），可使用简化格式，如果该远程分支不存在，则会被新建
git push <远程主机名> <本地分支名>

# 不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要 --all 选项
git push --all <远程主机名>

# 若确定本地仓库分支与远程仓库分支存在冲突，且远程仓库可覆盖，可执行
git push -f <远程主机名> <本地分支名>:<远程分支名>

# 拉取分支
# 用于从远程仓库分支获取代码并合并本地仓库分支，其实就是 git fetch 和 git merge FETCH_HEAD 指令的叠加

# 一般格式
git pull <远程主机名> <远程分支名>:<本地分支名>

# 远程分支是与当前分支合并
git pull <远程主机名> <远程分支名>


# 变基
git rebase
# 合并或修改当前分支最近的4次提交，-i表示交互式
git rebase -i HEAD~4

# 合并或修改当前分支自1234567（commit id 前7位）以来的提交，-i表示交互式
git rebase -i 1234567

# 将master分支上所有新增提交合并到当前分支，合并后，当前分支的新增提交均在 master 分支的提交之后
git rebase -i master

# rebase本地存储的origin远程对应的master分支，为了保证本地与远程一致，可以先使用git fetch origin命令
git rebase -i origin/master   

# 将指定远程仓库的全部更新取回本地
git fetch <远程主机名>

# 将指定远程仓库的指定分支的更新取回本地
git fetch <远程主机名> <分支名>


# 查看历史提交
# git log 用于查看历史提交，不包含已经删除的提交记录和 git reset 的操作记录
# 输出 commit id, author, date, commit message
git log

# 仅输出 commit id 前7个字符串和 commit message
git log --oneline

# 在git log 的基础上输出文件增删改的统计数据
git log --stat

# 输出每个commit具体修改的内容，输出的形式以diff的形式给出
git log -p

# 自定义输出的信息（包含字体和颜色），感兴趣可参考 https://www.cnblogs.com/bellkosmos/p/5923439.html，此处给出 一个例子
git log --pretty="%h %cd %an ==>%s" --date=format:'%Y-%m-%d %H:%M'
# 输出结果：
#  9e36820 2021-01-17 11:11 lianglongxiao ==>测试持续集成
#  4ff2c7b 2020-10-19 18:51 lianglongxiao ==>初步完成运动参数设置过程

# 查看dev中log有的commit，而master中log没有的commit
git log dev ^master

# 用于查看历史提交，包含已经删除的提交记录和git reset的操作记录，在版本恢复中很有作用！
git reflog


# git reset 用于回退版本，可以退回某次提交的版本，
# 将全部内容从当前版本回退3个版本，保留工作区和暂存区的内容，简单来说就是变为未提交状态
git reset --soft HEAD~3 
git reset --soft HEAD^^^

# 将全部内容从当前版本回退2个版本，保留工作区内容，简单来说就是变为未跟踪状态
git reset --mixed HEAD~2 
git reset --mixed HEAD^^ 
git reset HEAD~2

# 将全部内容从当前版本回退到指定版本(commit id为1234567)，工作区内容也一起回退
git reset --hard 1234567

# 将部分目录或文件从当前版本回退到指定版本(commit id 为1234567)，工作区内容也一起回退
git reset --hard 1234567 [dir] [file]


# git submodule 用于将一个git仓库作为另一个git仓库的子目录。它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立
# 将已存在的 Git 仓库(url 为远程仓库地址)添加为正在工作的仓库的子模块，若不指定path，子模块会将子项目放到一个与仓库同名的目录中path
git submodule add [url] [path]

# 克隆含子模块的仓库时，克隆仓库操作完成后，用于初始化并更新子模块
git submodule update --init --recursive

# 克隆含子模块的仓库时，同步初始化并更新子模块，url为主仓库远程地址
git clone --recurse -submodules [url]

# 仓库创建完成后，日常拉取所有子模块
git submodule foreach git pull

# 子模块的远程仓库地址更新后，首先需要手动更改 .gitmodules 文件中的子模块url地址，然后将新的url复制到本地配置中
git submodule sync


# git cherry-pick  用于部分代码变动（某几个提交）从一个分支转移到另一个分支
# 将其他分支的指定提交应用于当前分支
git cherry-pick [commit id]

# 将其他分支的最新提交应用于当前分支
git cherry-pick [branch name]

# 将其他分支的多个指定提交应用于当前分支
git cherry-pick [commit id 1] [commit id 2]

# 将其他分支的A到B的所有提交(不包含A)应用于当前分支
git cherry-pick A..B

# 将其他分支的A到B的所有提交(包含A)应用于当前分支
git cherry-pick A^..B

# 注意：
#     -e，--edit：打开外部编辑器，编辑提交信息
#     -n，--no-commit：只更新工作区和暂存区，不产生新的提交
#     -x：在提交信息的末尾追加一行(cherry picked from commit ...)，方便以后查到这个提交是如何产生的
#     -s，--signoff：在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作
#     -m parent-number，--mainline parent-number：如果原始提交是一个合并节点，来自于两个分支的合并，那么 Cherry pick 默认将失败，
#         因为它不知道应该采用哪个分支的代码变动。-m 配置项告诉 git，应该采用哪个分支的变动。它的参数parent-number是一个从1开始的整数，
#         代表原始提交的父分支编号，可以从git log中查看编号
#     如果操作过程中发生代码冲突，Cherry pick 会停下来，让用户决定如何继续操作
#         --continue：用户解决代码冲突后，第一步将修改的文件重新加入暂存区(git add .)，第二步使用 git cherry-pick --continue 命令，让 Cherry pick 过程继续执行
#         --abort：发生代码冲突后，退出 Cherry pick，回到操作前的样子
#         --quit：发生代码冲突后，退出 Cherry pick，但是不回到操作前的样子


# git diff 用于比较已跟踪的文件的不同
# 显示暂存区和工作区的差异，若不指定file或dir，默认比较全部文件
git diff [file] [dir]

# 显示暂存区和版本库的差异，若不指定file或dir，默认比较全部文件
git diff --cached [file] [dir]

# 显示工作区和版本库的差异，若不指定file或dir，默认比较全部文件
git diff HEAD [file] [dir]

# 比较两个分支上最后commit的内容的差别
git diff branch1 branch2

# 比较两个分支上最后commit的指定文件的差别
git diff branch1 branch2 [file]

# 比较两个分支上最后commit的有差异的文件(不详细,没有对比内容)
git diff branch1 branch2 --stat


# git stash 用于备份当前工作区已跟踪的文件
# 当前工作区内容不具备提交条件，但此时需要切换分支、拉取代码进行其它工作，此时需要git stash保存现场，待其它工作完成后，使用git stash pop恢复现场
# 保存当前工作区的内容，保存到git栈中，工作区内容与当前版本库保持一致，save及其后内容用于标记本次stash，可省略：
git stash save "test stash"

# 显示git栈中的所有工作区内容的备份
git stash list

# 清空git栈
git stash clear

# 显示做了哪些改动，默认show第一个存储,如果要显示其他存贮，后面加stash@{$num}，比如第二个
git stash show stash@{1}：

# 从git栈中获取到倒数第3次stash进去的内容，用以恢复工作区的内容，会删除栈中对应的stash，其中stash@{2}可以省略，表示倒数最后一次的stash
git stash pop stash@{2}

# 从git栈中获取到最后一次stash进去的内容，用以恢复工作区的内容，同时保留stash
git stash apply stash@{0}
git stash apply


# git checkout 用于切换分支或恢复工作区文件（文件必须为已跟踪状态）
# 在不改变工作区与暂存区内容的情况下，从当前分支切换到 dev 分支
git checkout dev

# 在不改变工作区与暂存区内容的情况下，创建新分支 dev，并从当前分支切换到 dev 分支
git checkout -b dev

# 把readme.txt文件在工作区的修改全部撤销
# 这里有两种情况：
#  一种是readme.txt自修改后还没有被放到暂存区，现在撤销修改就回到和版本库一模一样的状态
#  一种是readme.txt已经添加到暂存区后，又作了修改，现在撤销修改就回到添加到暂存区后的状态
git checkout -- readme.txt


# 以递归方式把所有后缀名为.txt 的文件在工作区的修改全部撤销
git checkout -- "*.txt"

# 把工作区全部文件的修改全部撤销
git checkout .

# 查看所有远程仓库
get remote -v
```

## 3. 基本开发流程

```plaintext
# 从远程clone仓库到本地
git clone ssh://...

# 切换到开发分支
git checkout -b <devel>

# 添加修改
git add .
# 编辑commit记录
git commit -a

# 更新远程分支(主要是更新origin/master主分支)
git fetch origin
git rebase origin/master

# 如果遇到CONFLICT冲突，在代码中解决冲突，然后
git add .
# 继续执行rebase
git rebase --continue
# 如果rebase操作失误，可以执行abort操作，取消本次rebase
git rebase --abort
# 如果在操作过程中不小心关闭窗口，可以重新打开终端，切换到仓库目录
git rebase --edit-todo

# rebase完成之后，需要push到远程的devel分支，但是由于rebase改变了devel分支的历史记录，在确保无误的问题下，需要强制push
git push -f origin <devel>
# 上述强制push有丢失更改的风险，保险起见，可以在远程新建一个分支
git push origin <devel>:<devel_fater_rebase>
```

## 4. 修改最近一次提交内容

```plaintext
# 方法一： 可以修改commit message，也可以修改提交内容
#修改需要修改的地方（只是修改commit message就不用做)
git add .  #这一步如果只是修改commit message不用输入
git commit --amend

# 方法二：与上面方法基本一致，也可以修改提交内容和commit message
git reset HEAD^
#修改需要修改的地方（只是修改commit message就不用做)
git add . #这一步如果只是修改commit message不用输入
git commit -m "new commit message" # 或者git commit -c ORIG_HEAD

# 方法三：提交到了错误的分支上的处理
git reset HEAD~ --soft
git stash
# 切换到正确的分支
git checkout name-of-the-correct-branch
git stash pop
git add .    # 或添加特定文件
git commit -m "你的提交说明"

# 方法四：rebase
# 查看修改
git rebase -i master~1 #最后一次
git rebase -i master~5 #最后五次
git rebase -i HEAD~3   #当前版本的倒数第三次状态
git rebase -i 32e0a87f #指定的SHA位置
# 需要注意最后的^号，意思是commit id的前一次提交
git rebase -i "5b3ba7a"^
```