# matplotlib

> matplotlib, python

[toc]

# &sect; font

## &para; 中文字体显示

### &rArr; linux[^1] 下的中文字体显示

1. 下载 [SimHei.ttf](https://github.com/StellarCN/scp_zh/blob/master/fonts/SimHei.ttf) 字体

2. 找到当前用户的 matplotlib 文件配置文件夹

ps. Windows 上测试成功字体为 `YaHei Consolas Hybrid 1.12` 

> 方法1

```python
import matplotlib
print(matplotlib.matplotlib_fname())
```

> 方法2

```bash
$ locate -b 'mpl-data' # base directory
/home/fm22/.virtualenvs/py/lib/python3.7/site-packages/matplotlib/mpl-data
$ cd `locate -b 'mpl-data'`
$ tree -d . # 列出当前文件夹结构
.
├── fonts
│   ├── afm
│   ├── pdfcorefonts
│   └── ttf
├── images
├── sample_data
│   └── axes_grid
└── stylelib
$ tree -L 1 .
.
├── fonts
├── images
├── matplotlibrc
├── sample_data
└── stylelib
```

则 `fonts/ttf` 里存放 `SimHei.ttf` 文件，之后需要修改 `matplotlibrc` 文件

3. 清除缓存

```bash
$ rm -r ~/.cache/matplotlib
```

可能可行的替代方法(未经测试) `python`

```python
from matplotlib.font_manager import _rebuild
_rebuild()
```

4. 修改 `matplotlibrc` 文件

找到下列选项并修改
```ini
font.family         : sans-serif        
# 在最前面加上SimHei即可
font.sans-serif     : SimHei, Bitstream Vera Sans, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif 
axes.unicode_minus，将True改为False，作用就是解决负号'-'显示为方块的问题
```

5. 测试

```python
import matplotlib.pyplot as plt
fig = plt.figure()
plt.title('中文标题')
plt.plot(range(20), label='中文标签')
plt.legend()
plt.show()
```

若设置成功则会显示中文

6. 测试不成功时

调用系统字体进行显示

```bash
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import rcParams
from matplotlib.font_manager import FontProperties

rcParams['axes.unicode_minus']=False  # 防止 '-' 号变成方块
font = FontProperties(fname='/tmp/download/SimHei.ttf', size=15)

x = np.linspace(0, 3.14, 100)
y = np.sin(x) * (1 + np.power(x, 2))
plt.plot(x, y, label='$sin(x) \, x^2$') # 显示数学公式
plt.title('函数图像', fontproperties=font) # title 与 label 使用 fontproperties
plt.legend(prop=font) # legend 使用 prop
plt.show()
```

[^1]: https://www.sail.name/2018/06/09/chinese-display-cube-in-matplotlib-at-ubuntu/, "matplotlib中文显示为方框(ubuntu)"
