# 键盘键位设置

## &sect; 键盘 <左 Ctrl> 与 <Caps> 键位互换

打开注册表
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout` 中添加 Scancode Map 二进制项，内容为

0000 00 00 00 00 00 00 00 00
0008 03 00 00 00 1D 00 3A 00
0010 3A 00 1D 00 00 00 00 00

# 参考

[^1]: https://blog.csdn.net/bnanoou/article/details/51187673, "CSDN"
