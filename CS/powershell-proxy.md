# PowerShell 代理设置

## &para; 无法解析网址

> `Unable to resolve package source 'https://www.powershellgallery.com/api/v2'`

Powershell 默认使用 IE 的 Internet 代理

当本机开启代理服务时, 如果在 `Internet 选项` => `连接` => `局域网设置` 中使用了自动配置脚本

此时开启下方代理服务器, 即可正常访问
