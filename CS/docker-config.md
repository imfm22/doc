# docker config

> docker

> !!!personal info: 阿里云加速器地址

# &sect; 1. docker install

## &para; 1.1 linux

### &rArr; ubuntu[^1]

```bash
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装 Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce
 
# 安装指定版本的Docker-CE:
# Step 1: 查找Docker-CE的版本:
# apt-cache madison docker-ce
#   docker-ce | 17.03.1~ce-0~ubuntu-xenial | http://mirrors.aliyun.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
#   docker-ce | 17.03.0~ce-0~ubuntu-xenial | http://mirrors.aliyun.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
# Step 2: 安装指定版本的Docker-CE: (VERSION 例如上面的 17.03.1~ce-0~ubuntu-xenial)
# sudo apt-get -y install docker-ce=[VERSION]
```

### &rArr; manjaro

```bash
yay -S docker
```

### 权限管理

将当前用户添加到 `docker` 用户组, 可以不使用 `sudo` 命令使用 `docker`

```bash
sudo usermod -aG docker $USER
id $USER    # 测试当前用户是否加入到 docker 用户组
```

添加完用户组之后, 需要注销登录或者重启

重启之后可以输入以下命令测试是否加入成功

```bash
docker images
```

如果终端可以正常显示而不会提示权限问题, 说明已经成功加入

## &para; 1.2 windows

对于Windows 10以下的用户，推荐使用Docker Toolbox

Windows安装文件：http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/

对于Windows 10以上的用户 推荐使用Docker for Windows

Windows安装文件：http://mirrors.aliyun.com/docker-toolbox/windows/docker-for-windows/

# &sect; 2. 镜像加速器

## &para; 2.1 阿里云加速[^2]

> 阿里云加速需要[申请容器镜像服务](https://cr.console.aliyun.com/)\
> `镜像中心` > `镜像加速器` 中可以看见用户的加速器地址

### linux

针对Docker客户端版本大于 1.10.0 的用户

通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://<id>.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### windows

1. Docker Toolbox

针对安装了Docker Toolbox的用户，您可以参考以下配置步骤：
创建一台安装有Docker环境的Linux虚拟机，指定机器名称为default，同时配置Docker加速器地址。

```cmd
docker-machine create --engine-registry-mirror=https://<id>.mirror.aliyuncs.com -d virtualbox default
```

查看机器的环境配置，并配置到本地，并通过Docker客户端访问Docker服务。
```cmd
docker-machine env default
eval "$(docker-machine env default)"
docker info
```

2. Docker for Windows

针对安装了Docker for Windows的用户，您可以参考以下配置步骤：

在系统右下角托盘图标内右键菜单选择 Settings，打开配置窗口后左侧导航菜单选择 Daemon。
切换 `basic` 按钮到 `advanced` ，然后编辑窗口内的JSON串，填写下方加速器地址：

实测本机 json 文件位于 `C:\Users\<username>\.docker\daemon.json`, 可自行查找具体位置
```json
{
  "registry-mirrors": ["https://<id>.mirror.aliyuncs.com"]
}
```

编辑完成后点击 Apply 保存按钮，等待Docker重启并应用配置的镜像加速器。

注意
Docker for Windows 和 Docker Toolbox互不兼容，如果同时安装两者的话，需要使用hyperv的参数启动。
```cmd
docker-machine create --engine-registry-mirror=https://xy5997r5.mirror.aliyuncs.com -d hyperv default
```
Docker for Windows 有两种运行模式，一种运行Windows相关容器，一种运行传统的Linux容器。同一时间只能选择一种模式运行。

## &para; 其他加速器[^3]

填写方法与阿里云加速相同

### 163加速

url: http://hub-mirror.c.163.com

### Docker 官方提供了在中国的加速器

url: https://registry.docker-cn.com

# 参考

[^1]: https://yq.aliyun.com/articles/110806?spm=5176.8351553.0.0.1daf1991aLpZGN "阿里云 docker-ce 安装指南"
[^2]: https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors "阿里云镜像加速器"
[^3]: https://blog.51cto.com/12924846/2345420 "docker 系列之镜像加速"
