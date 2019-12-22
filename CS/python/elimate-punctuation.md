# Python 去除标点符号

> string, punctuation

[toc]

# &sect; 1. 方法1[^1]

> str.isalnum：\
> S.isalnum() -> bool\
> Return True if all characters in S are alphanumeric and there is at least one character in S, False otherwise.\
> 特点： 只能识别字母和数字，杀伤力大，会把中文、空格之类的也干掉

```python
>>> string = "Special $#! characters   spaces 888323"
>>> ''.join(e for e in string if e.isalnum())
'Specialcharactersspaces888323'
```

# &sect; 2. 方法2

> string.punctuation

```python
import re, string

s ="string. With. Punctuation?" # Sample string 

# 写法一：
out = s.translate(string.maketrans("",""), string.punctuation)

# 写法二：
out = s.translate(None, string.punctuation)

# 写法三：
exclude = set(string.punctuation)
out = ''.join(ch for ch in s if ch not in exclude)

# 写法四：
>>> for c in string.punctuation:
			s = s.replace(c,"")
>>> s
'string With Punctuation'

# 写法五：
out = re.sub('[%s]' % re.escape(string.punctuation), '', s)
## re.escape:对字符串中所有可能被解释为正则运算符的字符进行转义

# 写法六：
# string.punctuation 只包括 ascii 格式； 想要一个包含更广（但是更慢）的方法是使用： unicodedata module :
from unicodedata import category
s = u'String — with - «Punctuation »...'
out = re.sub('[%s]' % re.escape(string.punctuation), '', s)
print 'Stripped', out
# 输出：u'Stripped String \u2014 with  \xabPunctuation \xbb'
out = ''.join(ch for ch in s if category(ch)[0] != 'P')
print 'Stripped', out
# 输出：u'Stripped String  with  Punctuation '


# For Python 3 str or Python 2 unicode values, str.translate() only takes a dictionary; codepoints (integers) are looked up in that mapping and anything mapped to None is removed.
# To remove (some?) punctuation then, use:
import string
remove_punct_map = dict.fromkeys(map(ord, string.punctuation))
s.translate(remove_punct_map)


# Your method doesn't work in Python 3, as the translate method doesn't accept the second argument any more. 
import unicodedata
import sys
tbl = dict.fromkeys(i for i in range(sys.maxunicode) if unicodedata.category(chr(i)).startswith('P'))
def remove_punctuation(text):
	return text.translate(tbl)
```

# &sect; 3. 方法3

> re

```python
import re
s ="string. With. Punctuation?"
s = re.sub(r'[^\w\s]','',s)
```

[^1]: https://blog.csdn.net/chihwei_hsu/article/details/81604272 "Python string 去掉标点符号 最佳实践"