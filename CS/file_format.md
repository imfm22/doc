# File Format

# &sect; Resources

网站资源:

- [FileFormat.Info](http://www.fileformat.info/index.htm)

# &sect; Unicode 编码

## &para; Unicode 和 utf-8 的区别[^1]

以汉字举例, 以下是 '汉' 字及其 Unicode 编码
```
A chinese character:      汉
it's unicode value:       U+6C49
convert 6C49 to binary:   01101100 01001001
```

当要存储这个字符到文件时，可以简单的将二进制值 `0110110001001001` 直接存到磁盘。

但是考虑到 Ascii 字符仅占8位，而中文字符占16位，当计算机读取二进制时，无从分辨哪些字符是8位，哪些是16位的，因此再存储的时候采用 utf-8 编码方式

| 1st Byte   | 2nd Byte   | 3rd Byte   | 4th Byte   | Number of Free Bits | Maximum Expressible Unicode Value |
| :--------- | :--------- | :--------- | :--------- | :------------------ | :-------------------------------- |
| `0xxxxxxx` |            |            |            | 7                   | `007F` hex (127)                  |
| `110xxxxx` | `10xxxxxx` |            |            | (5+6)=11            | `07FF` hex (2047)                 |
| `1110xxxx` | `10xxxxxx` | `10xxxxxx` |            | (4+6+6)=16          | `FFFF` hex (65535)                |
| `11110xxx` | `10xxxxxx` | `10xxxxxx` | `10xxxxxx` | (3+6+6+6)=21        | `10FFFF` hex (1,114,111)          |

对于 '汉' 字而言，需要16个自由位，因此其实际存储方式内容为 `1110xxxx10xxxxxx10xxxxxx`，将 '汉' 字的16位二进制数拆分成 4+6+6 的形式，分别放在对应位置然后就可以存储了。

```
Header  Place holder    Fill in our Binary   Result         
1110    xxxx            0110                 11100110
10      xxxxxx          110001               10110001
10      xxxxxx          001001               10001001
```
因此 '汉' 的 utf-8 编码为 `111001101011000110001001`。

读取文件时只要按照 utf-8 的编码方式解析出 Unicode 字符并对应字符表就可以得到相应字符了。

### &rArr; Python3 中的应用

```python
>>> hex(ord('汉')) # 输出为 Unicode 编码
'0x6c49'
>>> bin(ord('汉'))
'0b110110001001001'
>>> with open('utf-8', 'w', encoding='utf-8') as f:
>>>     f.write('汉 cn') # 写入 utf-8 字符
>>> with open('utf-8', 'rb') as f:
>>>     print(f.readline())
b'\xe6\xb1\x89 cn'
>>> s.encode('utf-8')
b'\xe6\xb1\x89 cn'
>>> han_bin = 0b111001101011000110001001 # '汉' 的 utf-8 编码
>>> hex(han_bin)
'0xe6b189' # 等同于 \xe6\xb1\x89
>>> u'\u6c49'
'汉'
```

# 参考

[^1]: https://stackoverflow.com/questions/643694/what-is-the-difference-between-utf-8-and-unicode, 'The difference between unicode and utf-8'