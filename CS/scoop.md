# scoop

# &sect; scoop 安装

> 前提条件

- Powershell 5+
- .NET Framework 4.5+

> 安装命令

Note: 修改执行策略

```powershell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```

安装命令(可能需要梯子)

```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')

# or shorter
iwr -useb get.scoop.sh | iex
```

# &sect; scoop 配置

## &para; 使用代理

假设本地代理端口为 1080

实测使用 `http://localhost:1080` 时，会出现如下错误

```cmd
未能解析此远程名称: 'http'
URL https://github.com/Kitware/CMake/releases/download/v3.16.2/cmake-3.16.2-win64-x64.zip is not valid
```
使用以下方式可以正常使用代理

```powershell
scoop config proxy 127.0.0.1:1080
```

## &para; 使用 `aria2` 作为下载工具

默认使用 `scoop install aria2` 之后会优先调用 aria2

手动打开或关闭 aria2 分别使用以下命令

```powershell
scoop config aria2-enabled true
scoop config aria2-enabled false
```

## &para; scoop 默认配置文件

默认配置文件为 `~/.config/scoop/config.json`

参考配置项如下

```json
{
    "lastupdate":  "2020-01-06T14:08:52.4199763+08:00",
    "SCOOP_REPO":  "https://github.com/lukesampson/scoop",
    "SCOOP_BRANCH":  "master",
    "proxy":  "127.0.0.1:1080",
    "aria2-enabled":  true
}
```