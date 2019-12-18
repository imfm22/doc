# Python 格式互相转换

> python, str

[toc]

# &sect; python3 字符串，数字，二进制格式转换[^1]

```sh
数字
    整数 -1 0 1
    浮点 -0.1 0.0 1.0
    二进制 0b11        结果 3
    八进制 0o77        结果 63
    16进制 0xFF        结果 255
 
字符串 <class 'str'>
    纯字符串 'str' "str" '''str''' """str"""
    字符串数字(二进制 0b) '0b0'     转成字符 str(0b10) 结果 '2'     ## 可以前置补零str(0b00000010)
    字符串数字(八进制 0o) '0o0'     转换字符 str(0o77) 结果 '63'    ## 可以前置补零str(0o0077)
    字符串数字(十进制)    '0'       转换字符 str(100)  结果 '100'   ## 不能前置补零
    字符串数字(16进制 0x) '0x0'     转换字符 str(0xFF) 结果 '255'   ## 可以前置补零str(0x00FF)
二进制 <class 'bytes'>
    二进制字节表示 b''    # ASCII 字符 0-9 a-z A-Z 等
```

## &para; 数字 &rArr; 字符串
```python
## 数字 转 字符串 ##  255(10进制)  0b11(2进制)  0xFF(16进制)
## (10进制数)
>>> bin(255)            '0b11111111'
>>> oct(255)            '0o377'
>>> hex(255)            '0xff'
## (2进制数)
>>> bin(0b11)           '0b11'
>>> hex(0xFF)           '0xff'
## (16进制数)
>>> bin(0xFF)           '0b11111111'
>>> hex(0xFF)           '0xff'
```

## &para; 字符串 &rArr; 数字
```python
## 字符串 转 数字（十进制数）##  '123'(以10进制解析)  '10'(以2进制解析)  'a'(以16进制解析)
## (10进制表示的字符串)
>>> int('123')          123         ## 十进制字符转十进制数字
>>> int('123',10)       123         ## 默认是十进制
## (二进制表示的字符串)
>>> int('100',2)        4           ## 二进制的 100 等于 十进制的 4（可以不加前置 0b）
>>> int('0b100',2)      4           ## 二进制的 100 等于 十进制的 4
>>> int('0b0100',2)     4           ## 可以前置补零
## (16进制表示的字符串)
>>> int('a',16)         10          ## 16进制的 a 等于 十进制的 10（可以不加前置 0x）
>>> int('0xa',16)       10          ## 16进制的 a 等于 十进制的 10
>>> int('0x0a',16)      10          ## 16进制的 a 等于 十进制的 10（可以前置补零）
>>> int('10',16)        16          ## 16进制的10 等于 十进制的 16（可以不加前置 0x）
>>> int('0x10',16)      16          ## 16进制的10 等于 十进制的 16
>>> int('0x0010',16)    16          ## 16进制的10 等于 十进制的 16（可以前置补零）
```

## &para; 数字 &rArr; 数字
```python
## 数字 转 数字 ##  0b11  0xFF
## 十进制 转 十进制
>>> int(255)            255         # 无意义操作
>>> 255                 255         # 无意义操作
## 二进制 转 十进制
>>> int(0b11)           3           # 可加前置零 int(0b0011)
>>> 0b11111111          255         # 等价
## 16进制 转 十进制
>>> int(0xFF)           255
>>> 0xff                255         # 等价 且 忽略大小写
>>> 0xFF                255         # 等价 且 忽略大小写
## 十进制 转 二进制（使用 数字 转 字节码/字符）
255 等价 0b11111111
## 十进制 转 16进制（使用 数字 转 字节码/字符）
255 等价 0xff
```

## &para; 字符串 &rArr; 字节码
```python
## 字符串 转 字节码 ##
>>> bytes('abc','utf-8')              b'abc'
>>> bytes('编程','utf-8')             b'\xe7\xbc\x96\xe7\xa8\x8b'
>>> bytes('Python3编程','utf-8')      b'Python3\xe7\xbc\x96\xe7\xa8\x8b'
>>> 'Python3编程'.encode('UTF-8')     b'Python3\xe7\xbc\x96\xe7\xa8\x8b'
>>> S = 'Python3编程'                 'Python3编程'
>>> B = bytes(S,'utf-8')              b'Python3\xe7\xbc\x96\xe7\xa8\x8b'
>>> FMT = str(len(B)) + 's'           '13s'
>>> struct.pack(FMT,B)                b'Python3\xe7\xbc\x96\xe7\xa8\x8b'
## 以16进制数字写的字符串，直接转成一样的字节码（2个16进制字符才是一个字节）
>>> bytes.fromhex('01')               b'\x01'                       # 单字节
>>> bytes.fromhex('0001')             b'\x00\x01'                   # 双字节
>>> bytes.fromhex('aabbccddeeff')     b'\xaa\xbb\xcc\xdd\xee\xff'   # 多字节
```

## &para; 字节码 &rArr; 字符串
```python
## 字节码 转 字符串 ##  取出16进制表示的内容
>>> b'abc'.decode('UTF-8')                                  'abc'
>>> b'Python3\xe7\xbc\x96\xe7\xa8\x8b'.decode('UTF-8')      'Python3编程'
>>> b'\xaa\xbb\xcc\xdd\xee\xff'.hex()                       'aabbccddeeff'
>>> b'0'.hex()                                              '30'    ## 字符0在ASCII码上的数字（数字是16进制表示）== 48（十进制）
>>> b'1'.hex()                                              '31'
>>> b'z'.hex()                                              '7a'
```

## &para; 数字 &rArr; 字节码
```python
## 数字 转 字节码（是二进制，用16进制显示） ##
# 10进制数 转 字节码
import struct
>>> struct.pack('B',0)          b'\x00'
>>> struct.pack('B',1)          b'\x01'
>>> struct.pack('B',101)        b'e'                    ## 101 对应 16进制的 0x65（此处返回值是显示为当前整数 101 对应的 ASCII字符 e）
>>> struct.pack('B',255)        b'\xff'                 # 无符号最大单字符可以表示的数字
>>> struct.pack('>i',255)       b'\x00\x00\x00\xff'     # 4字节大端表示的数字
>>> struct.pack('<i',255)       b'\xff\x00\x00\x00'     # 4字节小端表示的数字
# 2进制数 转 字节码
import struct
>>> struct.pack('B',0b11111111)     b'\xff'
>>> struct.pack('>i',0b111)         b'\x00\x00\x00\x07'     # 0b111 等于 7（10进制）
>>> struct.pack('>i',0b1111)        b'\x00\x00\x00\x0f'     # 0b1111 等于 15（10进制）
>>> struct.pack('>i',0b11111)       b'\x00\x00\x00\x1f'     # 0b11111 等于 31（10进制）
# 16进制数 转 字节码
import struct
>>> struct.pack('B',0xff)           b'\xff'
>>> struct.pack('>i',0xfff)         b'\x00\x00\x0f\xff'
```

## &para; 字节码 &rArr; 转数字
```python
## 字节码 转 数字 ##
import struct          16进制表现                10进制等值
>>> struct.unpack('B', b'\xff')                      (255,)          # 单字节
>>> struct.unpack('>i', b'\x00\x00\x00\xff')         (255,)          # 4字节，大端模式
>>> struct.unpack('<i', b'\x00\x00\x00\xff')         (-16777216,)    # 4字节，小端模式
## 手动 转换
字节码 -> 字符串
>>> B = b'\xe9'
>>> S = B.hex()
>>> S                                               # 值 'e9'
字符串（16进制格式）-> 数字（10进制）
>>> int(S,16)                                       # 值 233
```

## &para; ASCII 字符和数字
```python
## ASCII 字符 和 数字
字节    b'\x05'
字符串   '\x05'
## 将一个整数 (0-1114111) 转换为 一个字符（整数对应的 ASCII 字符）
## ValueError: chr() arg not in range(0x110000)
>>> chr(0)                  '\x00'
>>> chr(1)                  '\x01'
>>> chr(97)                 'a'
>>> chr(1114111)            '\U0010ffff'
>>> len(chr(101))           1  # 长度为 1个字符
>>> len(chr(1114111))       1  # 长度为 1个字符
 
## 将一个 ASCII字符 转换为 一个整数
>>> ord('\x00')             0
>>> ord('\x01')             1
>>> ord('a')                97
>>> ord('0')                48
>>> ord('1')                49
>>> ord('A')                65
>>> ord('Z')                90
>>> ord('\U0010ffff')       1114111
```
 
## &para; ASCII 字符和字节码
```python
## ASCII 字符 和 bin(字节)
 
from binascii import b2a_hex, a2b_hex
>>> a2b_hex('ab')
b'\xab'
 
>>> b2a_hex(b'ab')
b'6162'
>>> a2b_hex(b'6162')
b'ab'
```

# 参考

[^1]: https://blog.csdn.net/PanDD_0_1/article/details/86687140 "python3 字符串，数字，二进制格式转换"