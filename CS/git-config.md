# Git Config

> git, config

# &sect; `git config`

> `git` 配置

`git` 配置分为三种作用域(`<Scope>: <file-name>`)

1. system: `gitconfig`
2. global: `.gitconfig`
3. local: `config`

## &para; `git config`

### config[^1]

```bash
# Method 1
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --system user.name "John Doe"
$ git config --system user.email johndoe@example.com
$ git config --local user.name "John Doe"
$ git config --local user.email johndoe@example.com

# Method 2
$ git config --global --edit
$ git config --system --edit
$ git config --local --edit

# Method 3
$ vim ~/.gitconfig  # global config
$ vim <git-build-path>/gitconfig  # system config
$ vim <project-path>/config  # local config
```

### 使用代理加速代码传输速度

1. 对 https 传输协议进行加速

```bash
git config --global http.https://github.com.proxy http://localhost:1080
git config --global https.https://github.com.proxy http://localhost:1080
```

2. 对 git 协议进行加速

编辑 `~/.ssh/config` 文件，添加 `ProxyCommand` 命令。
connect.exe 文件需要自己下载，Windows中使用 `scoop`， 可以用 `scoop install connect` 进行安装。

```cmd
Host github.com
	ProxyCommand connect.exe -S 127.0.0.1:1080 %h %p
	HostName github.com
	PreferredAuthentications publickey
	IdentityFile C:\Users\liujh.BPG\.ssh\github_id_rsa
```

# &sect; Problem

1. 域网络中，无法访问到配置文件

> 域网络中，`cmd`/`powershell` 的默认启动文件夹为 `P:\`, `git` 创建即访问 `.gitconfig` 配置文件

**解决方案**: 默认从 `%HOMEDRIVER%%HOMEPATH`(实际案例为`P:\`) 文件夹中取配置文件, 但是 `P:\` 为公共网盘，没有创建文件的权限，因此通过添加 `%HOME%` 环境变量，使 `git` 从 `%HOME%` 中获取配置文件，从而解决问题

# 参考

[^1]: https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration "Git Configuration"
