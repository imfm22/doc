# ssh 配置

> ssh, config

[toc]

# &sect; 直接连接

1. 普通登录

```bash
$ ssh <user>@<example.com>
```

2. 使用密码登录

```bash
$ ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no -p <port> <user>@<example.com>
```

# &sect; 配置文件

## &para; linux 客户端

### 我在 Manjaro linux 上的配置

```bash
tree ~/.ssh
# =============================
# .ssh/
# ├── config    (*ssh配置*)
# ├── gitee_id_rsa
# ├── gitee_id_rsa.pub
# ├── github_id_rsa
# ├── github_id_rsa.pub
# └── known_hosts   (*已知主机*)
```

### `config` 文件配置

> `Host` 名称 \
> `HostName` 地址 \
> `PreferredAuthentications` 验证方式，如 `publickey` 或 `password` \
> `IdentityFile` 认证文件地址

```ini
# gitee
Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitee_id_rsa

# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github_id_rsa
```

复制公钥到服务器端

```bash
$ scp aliyun_id_rsa.pub ali:.ssh/authorized_keys
```

## &para; windows 客户端

windows 客户端配置与 linux 基本相同

### `config` 文件

windows 不使用代理下载 github 的内容非常慢

> 此处先下载 [`connect.exe`](https://bitbucket.org/gotoh/connect/downloads/) 文件, 然后配置 `config` 文件

```ini
Host github.com
    ProxyCommand connect -H 127.0.0.1:1080 %h %p
	HostName github.com
	PreferredAuthentications publickey
	IdentityFile C:/Users/liuja/.ssh/github_id_rsa
```

## &para; 服务器端

### aliyun 服务器配置

> 认证公钥中为客户端生成的公钥，例如 `aliyun_id_rsa.pub`

```bash
tree ~/.ssh
# =============================
# .ssh
# └── authorized_keys (*已认证公钥*)
```

# &sect; 配置远程桌面

参见[^1]

# 参考

[^1]: https://blog.csdn.net/weixin_42232749/article/details/81624156 "linux系统之间ssh远程连接图形用户界面"
