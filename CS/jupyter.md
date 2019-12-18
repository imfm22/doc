# jupyter

> jupyter

[toc]

# &sect; install

```bash
$ pip install jupyter
```

## &para; wolfram language support

## &para; c/cpp support

> 步骤

1. 安装编译 cling

```bash
$ yay -S cling-git
$ yay -S cling-jupyter-git
```

2. jupyter kernel 添加内核支持

> opencv test

参见 `md/opencv/*/OpenCV.ipynb`

```cpp
// 配置 OpenCV 环境
#pragma cling add_include_path("/usr/local/include/opencv4")
#pragma cling add_library_path("/usr/local/lib64")
#pragma cling load("libopencv_core.so")
#pragma cling load("libopencv_highgui.so")
#pragma cling load("libopencv_ml.so")

#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;

cv::Mat mat = cv::imread("/home/fm22/Pictures/ex.jpg");
cv::imshow("ex", mat);
cv::waitKey(0);
```

## &para; java support

# &sect; config



# &sect; jupyter themes

安装 jupyter themes

```bash
$ pip install jupyterthemes
$ jt -l # 列出所有的可用主题
Available Themes: 
   chesterish
   grade3
   gruvboxd
   gruvboxl
   monokai
   oceans16
   onedork
   solarizedd
   solarizedl
$ jt -t solarizedd # 安装 solarized dark 主题
```

# &sect; python lib

## &para; maptlotlib

### Problems

#### 黑色 jupyter theme 下无法看清标签

> 解决方案1: 使用黑色主题

查看支持的主题

```python
$ plt.style.available

['bmh',
 'classic',
 'dark_background',
 'fast',
 'fivethirtyeight',
 'ggplot',
 'grayscale',
 ...
 'Solarize_Light2',
 'tableau-colorblind10',
 '_classic_test']
```

使用主题

```python
import matplotlib.pyplot as plt
plt.style.use('dark_background')
```

> 解决方案2: 使用 Seaborn 主题

**!!! windows 测试未成功 !!!**

Then put the following lines in your `~/.ipython/profile_default/startup/startup.ipy` file, Jupyter will execute them every time you open a notebook (context, style, and font_scale arguments are optional)[^1]:

> on windows

path dir: `C:\Users\liuja\.ipython\profile_default\startup`

```python
import seaborn as sns
sns.set(context='paper', style='darkgrid', rc={'figure.facecolor':'white'}, font_scale=1.2)
```

[^1]: https://www.reddit.com/r/Python/comments/4z6znd/dark_jupyter_notebook_themes_that_dont_make_plot/, "Dark Jupyter Notebook themes that don't make plot axes illegible?"