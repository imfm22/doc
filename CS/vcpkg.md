# vcpkg

> Windows 下的开发包管理工具

## &sect; 安装与下载

### &para; Windows

因为国内网络问题, 如果出现下载出错问题, 可以更改环境变量中的 `HTTPS_PROXY/HTTP_PROXY` 值来加速下载

```powershell
$env:HTTPS_PROXY="localhost:1080"
$env:HTTP_PROXY="localhost:1080"

# 下载 vcpkg 源码
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg

# 编译源码生成 vcpkg.exe
.\bootstrap-vcpkg.bat

# 集成(需要管理员权限)
# Visual Studio 需要安装英文语言包
.\vcpkg integrate install
```

> cmake 使用 vcpkg 需要在配置时添加选项(如下命令), 其中 vcpkg 的根目录根据自己的进行调整

```powershell
cmake -S. -Bbuild -DCMAKE_TOOLCHAIN_FILE=D:\vcpkg\scripts\buildsystems\vcpkg.cmake
```

> vcpkg 默认编译 x86 32 位的库, 可以添加环境变量\
> `VCPKG_DEFAULT_TRIPLET=x64-windows`\
> 此时默认编译 64 位的库

> 或者使用显式命令

```powershell
.\vcpkg install zlib:x64-windows
.\vcpkg install zlib openssl --triplet x64-windows
```

> 库安装及 PowerShell 命令提示添加

```powershell
# 包安装, 期间会下载绿色版 git 等软件
.\vcpkg install sdl2 curl

# PowerShell 命令提示集成
.\vcpkg integrate powershell

# 重启 PowerShell 后如果无法自动补全, 则使用一下命令
echo "Import-Module 'D:/vcpkg/scripts/posh-vcpkg'" >> $PROFILE
```

> **详细文档可以参考 vcpkg 根目录下的 doc 文件夹**

## &sect; cmake find_package 配置

- `sdl2`

```cmake
find_package(SDL2 CONFIG REQUIRED)
target_link_libraries(main PRIVATE SDL2::SDL2 SDL2::SDL2main)
```