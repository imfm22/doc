# File Format

# &sect; Resources

网站资源:

- [FileFormat.Info](http://www.fileformat.info/index.htm)

# &sect; 字符编码标准

## &para; 字符集与字符编码

1. **字符集(Character Set)**

> 计算机可以表示的字符组成的一个集合, 集合里的每一个字符都对应一个唯一的序号, 用以表征该字符在集合中的位置

常见字符集有:

- Unicode: Universal Coded Character Set, 通用字符编码集
- ASCII: America Standard Code for Information Interchange，美国标准信息交换码

对于今天通用的 Unicode 标准, 用的字符集称作 `UCS-4`（4-bytes Universal Character Set，4 字节通用字符集），里面的每一个字符的标识都是 4 比特 32 位的二进制数，这个数我们称之为**字符代码**[^2]。

2. **字符集编码**

> 字符集编码则是指字符在传输过程当中用于表示字符的二进制序列。

现在国内常用的字符集编码有

- UTF-8
- GBK
- Big5

## &para; **发展历程**

1. 美国

字符编码标准本质上是为了解决计算机上的信息交换问题，最开始由美国主导，因为是字母文字，所以只需要 8 位 bit 就可以建立所有字母与二进制码的对应关系了, 这个就是 ASCII 码, ASCII 的字符集和编码方式相同，因此在术语上也不做区分。

2. 中国

中国大陆为建立本国语言与二进制的对应关系[^4]

> 详见扩展阅读

- GB2312-80(Windows 中命名为 ANSI)
- GBK: 汉字内码扩展规范,
- GB18030: 国家标准 GB 18030-2005《信息技术中文编码字符集》

3. Unicode

- Unicode 1.1/ISO/IEC 10646-1: 1993
  - UCS-2: 2 字节通用字符集
  - UTF-16 LE (小端) 在 Windows 中也称为 _Unicode_
  - 因为美国的原因没有使用 4 个字节
- UCS-4: 4 字节通用字符集

## &para; Unicode 和 UTF-8 的区别

- **Unicode**: Universal Coded Character Set
- **UTF**: Unicode Transformation Format[^3]
  - **UTF-8**: An 8-bit variable-width encoding which maximizes compatibility with ASCII

> UTF-8 是变长的字符编码传输格式，在内存中，程序执行过程中不易定位，因此，多数语言在执行过程中会通过查表法将 UTF-8 转换成定长的 Unicode 码。

> 字符编码传输格式都会有一张与 Unicode 码相互转换的表。

### 内存中的字符

1. Windows 操作系统

Windows 目前为 16 位，参见知乎
[vczh](https://www.zhihu.com/question/52346583)
的回答

2. Linux 操作系统

> 待考证

### &rArr; 实例[^1]

以汉字举例, 以下是 '汉' 字及其 Unicode 编码

```
A chinese character:      汉
it's unicode value:       U+6C49
convert 6C49 to binary:   01101100 01001001
```

当要存储这个字符到文件时，可以简单的将二进制值 `0110110001001001` 直接存到磁盘。

但是考虑到 Ascii 字符仅占 8 位，而中文字符占 16 位，当计算机读取二进制时，无从分辨哪些字符是 8 位，哪些是 16 位的，因此再存储的时候采用 UTF-8 编码方式

| 1st Byte   | 2nd Byte   | 3rd Byte   | 4th Byte   | Number of Free Bits | Maximum Expressible Unicode Value |
| :--------- | :--------- | :--------- | :--------- | :------------------ | :-------------------------------- |
| `0xxxxxxx` |            |            |            | 7                   | `007F` hex (127)                  |
| `110xxxxx` | `10xxxxxx` |            |            | (5+6)=11            | `07FF` hex (2047)                 |
| `1110xxxx` | `10xxxxxx` | `10xxxxxx` |            | (4+6+6)=16          | `FFFF` hex (65535)                |
| `11110xxx` | `10xxxxxx` | `10xxxxxx` | `10xxxxxx` | (3+6+6+6)=21        | `10FFFF` hex (1,114,111)          |

对于 '汉' 字而言，需要 16 个自由位，因此其实际存储方式内容为 `1110xxxx10xxxxxx10xxxxxx`，将 '汉' 字的 16 位二进制数拆分成 4+6+6 的形式，分别放在对应位置然后就可以存储了。

```
Header  Place holder    Fill in our Binary   Result
1110    xxxx            0110                 11100110
10      xxxxxx          110001               10110001
10      xxxxxx          001001               10001001
```

因此 '汉' 的 UTF-8 编码为 `111001101011000110001001`。

读取文件时只要按照 UTF-8 的编码方式解析出 Unicode 字符并对应字符表就可以得到相应字符了。

### &rArr; Python3 中的应用

```python
>>> hex(ord('汉')) # 输出为 Unicode 编码
'0x6c49'
>>> bin(ord('汉'))
'0b110110001001001'
>>> with open('UTF-8', 'w', encoding='UTF-8') as f:
>>>     f.write('汉 cn') # 写入 UTF-8 字符
>>> with open('UTF-8', 'rb') as f:
>>>     print(f.readline())
b'\xe6\xb1\x89 cn'
>>> s.encode('UTF-8')
b'\xe6\xb1\x89 cn'
>>> han_bin = 0b111001101011000110001001 # '汉' 的 UTF-8 编码
>>> hex(han_bin)
'0xe6b189' # 等同于 \xe6\xb1\x89
>>> u'\u6c49'
'汉'
>>> hex(ord('😊')) # emoji, 占用4个字节
'0x1f60a'
>>> '😊'.encode('utf-8') # utf-8 编码
b'\xf0\x9f\x98\x8a'
>>> len('😊')
1
```

# 参考

[^1]: https://stackoverflow.com/questions/643694/what-is-the-difference-between-UTF-8-and-unicode 'The difference between unicode and UTF-8'
[^2]: https://www.zhihu.com/question/52346583 '计算机中为何不直接使用 UTF-8 编码进行存储而要使用 Unicode 再转换成 UTF-8 ？'
[^3]: https://en.wikipedia.org/wiki/Unicode 'Wikipedia, Unicode'
[^4]: https://www.zhihu.com/question/19677619 'GB2312、GBK、GB18030 这几种字符集的主要区别是什么？'

## GB2312、GBK、GB18030 详解[扩展阅读]

- GB2312-80(Windows 中命名为 ANSI)
  - 1981 年实施
  - 共收录 6763 个汉字, 也包括其余 682 个字符
    - 拉丁字母
    - 希腊字母
    - 日文平假名及片假名字母
    - 俄语西里尔字母
  - 对任意一个图形字符都采用两个字节表示，并对所收汉字进行了“分区”处理, 每区含有 94 个汉字／符号，分别对应第一字节和第二字节。这种表示方式也称为区位码
    - 01-09 区为特殊符号
    - 16-55 区为一级汉字，按拼音排序
    - 56-87 区为二级汉字，按部首／笔画排序
    - 10-15 区及 88-94 区则未有编码
    - GB 2312 的编码范围为 2121H-777EH，与 ASCII 有重叠，通行方法是将 GB 码两个字节的最高位置 1 以示区别
- GBK: 汉字内码扩展规范,
  - GBK 共收入 21886 个汉字和图形符号
    - GB 2312 中的全部汉字、非汉字符号。
    - BIG5 中的全部汉字。
    - 与 ISO 10646 相应的国家标准 GB 13000 中的其它 CJK 汉字，以上合计 20902 个汉字。
    - 其它汉字、部首、符号，共计 984 个
  - GBK 向下与 GB 2312 完全兼容，向上支持 ISO 10646 国际标准，在前者向后者过渡过程中起到的承上启下的作用
  - GBK 采用双字节表示，总体编码范围为 8140-FEFE 之间，首字节在 81-FE 之间，尾字节在 40-FE 之间，剔除 XX7F 一条线。GBK 编码区分三部分：
    - 汉字区
      - GBK/2：OXBOA1-F7FE, 收录 GB 2312 汉字 6763 个，按原序排列；
      - GBK/3：OX8140-AOFE，收录 CJK 汉字 6080 个；
      - GBK/4：OXAA40-FEAO，收录 CJK 汉字和增补的汉字 8160 个。
    - 图形符号区
      - GBK/1：OXA1A1-A9FE，除 GB 2312 的符号外，还增补了其它符号
      - GBK/5：OXA840-A9AO，扩除非汉字区。
    - 用户自定义区
      - GBK 区域中的空白区，用户可以自己定义字符。
- GB18030: 国家标准 GB 18030-2005《信息技术中文编码字符集》
  - GB 18030 与 GB 2312-1980 和 GBK 兼容，共收录汉字 70244 个
    - 与 UTF-8 相同，采用多字节编码，每个字可以由 1 个、2 个或 4 个字节组成。
    - 编码空间庞大，最多可定义 161 万个字符。
    - 支持中国国内少数民族的文字，不需要动用造字区。
    - 汉字收录范围包含繁体汉字以及日韩汉字
  - GB 18030 编码是一二四字节变长编码
    - 单字节，其值从 0 到 0x7F，与 ASCII 编码兼容。
    - 双字节，第一个字节的值从 0x81 到 0xFE，第二个字节的值从 0x40 到 0xFE（不包括 0x7F），与 GBK 标准兼容。
    - 四字节，第一个字节的值从 0x81 到 0xFE，第二个字节的值从 0x30 到 0x39，第三个字节从 0x81 到 0xFE，第四个字节从 0x30 到 0x39。
