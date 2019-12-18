# neovim config

> neovim, config

[toc]

# &sect; install

## On manjaro deepin

```bash
$ yay -S neovim
$ yay -S python-neovim # 安装 python3 的支持
```

安装 python3 neovim 相关模块

```bash
$ workon py # 我的 python 虚拟环境
$ pip install neovim
$ pip install pynvim
```

安装 manjaro linux 上对系统剪贴板的支持

```bash
$ yay -S xsel # xsel/xclip 都可以
```

## On Windows

### Via scoop

```powershell
$ scoop install neovim
```

# &sect; config

## 1. vimrc 文件

> neovim 的配置文件完全适配 vim 的配置文件, 可以直接软连接到 `~/.vimrc` 文件上

## 2. neovim config 文件

neovim 文件配置地址

> linux: `~/.config/nvim/init.vim`\
> windows: `~/AppData/Local/nvim/init.vim`

文件内容

**!!! Attention: modify `let g:python3_host_prog='/python/executable/path'`**

```vim
call plug#begin('~/.vim/plugged')
Plug 'junegunn/vim-easy-align' " Shorthand notation; fetches https://github.com/junegunn/vim-easy-align
Plug 'https://github.com/junegunn/vim-github-dashboard.git' " Any valid git URL is allowed
Plug 'SirVer/ultisnips' | Plug 'honza/vim-snippets' " Multiple Plug commands can be written in a single line using | separators

Plug '~/my-prototype-plugin' " Unmanaged plugin (manually installed and updated)

Plug 'scrooloose/nerdtree'  " file list
Plug 'majutsushi/tagbar'  " show tags in a bar (functions etc) for easy browsing
Plug 'davidhalter/jedi-vim'   " jedi for python
Plug 'tiagofumo/vim-nerdtree-syntax-highlight'  "to highlight files in nerdtree
Plug 'Vimjas/vim-python-pep8-indent'  "better indenting for python
Plug 'kien/ctrlp.vim'  " fuzzy search files
Plug 'tweekmonster/impsort.vim'  " color and sort imports
Plug 'wsdjeg/FlyGrep.vim'  " awesome grep on the fly
Plug 'w0rp/ale'  " python linters
Plug 'airblade/vim-gitgutter'  " show git changes to files in gutter
Plug 'tpope/vim-commentary'  "comment-out by gc
Plug 'roxma/nvim-yarp'  " dependency of ncm2
Plug 'ncm2/ncm2'  " awesome autocomplete plugin
Plug 'HansPinckaers/ncm2-jedi'  " fast python completion (use ncm2 if you want type info or snippet support)
Plug 'ncm2/ncm2-bufword'  " buffer keyword completion
Plug 'ncm2/ncm2-path'  " filepath completion

Plug 'https://github.com/vim-scripts/fcitx.vim.git' " 自动切换中英文
Plug 'vim-airline/vim-airline' " 状态栏
Plug 'vim-airline/vim-airline-themes'
Plug 'jiangmiao/auto-pairs' " 括号匹配

call plug#end() " Initialize plugin system
" ...
```

# &sect; neovim plugin

## &para; vim-plug

> github: `https://github.com/junegunn/vim-plug`

### Install vim-plug on Linux

```bash
$ curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

### Install vim-plug on Windows via Powershell

```powershell
md ~\AppData\Local\nvim\autoload
$uri = 'https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
(New-Object Net.WebClient).DownloadFile(
  $uri,
  $ExecutionContext.SessionState.Path.GetUnresolvedProviderPathFromPSPath(
    "~\AppData\Local\nvim\autoload\plug.vim"
  )
)
```

在 vim 中，进入命令模式

```vim
:PlugInstall
```

