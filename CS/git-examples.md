# git cmd examples

> git, cmd, examples

# &sect; `^/~`

> `^` 和 `~` 后面不带数字时都表示向上移一位

```bash
# 检出第二个父节点上一位
$ git checkout HEAD^2

# 检出第一个父节点上两位
$ git checkout HEAD~2

# 合并操作
$ git checkout HEAD~^2~2
```

# &sect; `git branch`

```bash
# Create a branch
$ git branch bugFix

# 将 master 分支强制指向 HEAD 的第 3 级父提交
$ git branch -f master HEAD~3

# 将 master 强制定位到指定的 commit-id 上
$ git branch -f master <commit-id>

# 创建新分支并跟踪远程分支
$ git branch -u origin/master noMaster
```

# &sect; `git checkout`

```bash
# Switch branch to bugFix
$ git checkout bugFix

# Create a branch bugFix and switch to it
$ git checkout -b bugFix

# 创建新分支并跟踪远程分支, 参见 git branch
$ git checkout -b noMaster origin/master
```

# &sect; `git commit`

```bash
$ git commit -m "<commit-message>"

# 修改 commit
$ git commit --amend
```

# &sect; `git describe`

> 描述最近的一个锚点(标签 tag)
```bash
$ git describe <ref: 提交记录的引用, 默认为 HEAD>
```

# &sect; `git merge`

> 将其他分支并入当前分支
```bash
$ git merge <other-branch>
```

# &sect; `git push`

> 向远程仓库提交代码
```bash
$ git push

# 向远程仓库提交分支代码，分支代码跟踪关系可用
# git checkout -b ... / git branch -u ...
$ git push <remote-repo> <branch>
$ git push origin master

# 向远程仓库推送代码并指定分支
$ git push <remote-repo> <src>:<desctination>
$ git push origin foo^:master

# 删除远程分支 foo(把空分支推送上去)
$ git push origin :foo
```

# &sect; `git pull`

> 从远程仓库拉取代码并合并\
> 相当于 git fetch + git merge
```bash
$ git pull

# 使用 rebase 代替 merge
$ git pull --rebase

# git pull 参数, 当前本地检出分支 master
# 将远程 bar 分支拉取到本地 orgin/bar 位置下并创建本地分支 foo
# 将本地分支 foo 合并到 master
$ git pull origin bar:foo
```

# &sect; `git fetch`

> 从远程仓库拉取代码
```bash
$ git fetch

# 从远程仓库拉取代码到指定分支
$ git fetch <remote-repo> <branch>
$ git fetch origin master

$ git fetch origin foo~1:bar

# 在本地创建新分支 bar
$ git fetch origin :bar
```

# &sect; `git rebase`

```bash
# 对包括当前的 commit 的最近三个 commit 进行重新排序
$ git rebase -i HEAD~3

# 将当前分支与其他分支合并
$ git rebase <branch-name>

# 将分支合并到目标并将当前分支切换到 <branch>
$ git rebase <target> <branch>
```

# &sect; `git reset`

> 仅可用于本地
```bash
# 放弃所有 [commit] 后的 commit, 在本地保存更改
$ git reset [commit]

# 放弃所有更改并回到某个特定的 commit
$ git reset --hard [commit]
```

# &sect; `git revert`

> 可用于远程仓库
```bash
$ git revert HEAD~3
```

# &sect; `git tag`

```bash
$ git tag <tag-name> <target-name: commit-id>
```