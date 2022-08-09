# Vim 配置手册

[Vim下载地址](https://www.vim.org/download.php)，分为适用 MS-Windows 系统的 [GVim](https://www.vim.org/download.php#pc)，适用 Unix 的 [Vim](https://www.vim.org/git.php)，适用 Mac 的 [MacVim](https://github.com/macvim-dev/macvim/releases)。

- Mac

  ```shell
  # 编辑 ~/.zshrc 在文件末尾添加
  vim ~/.zshrc
  
  alias vim="/Applications/MacVim.app/Contents/MacOS/Vim"
  alias mvim="/Applications/MacVim.app/Contents/MacOS/MacVim"
  ```

  

## 语言环境部署

因为我平时适用的语言比较多，如：Go，Rust，Python，PHP，JS等。所以 YCM 插件我要配置全语言版本。因此在配置 Vim 之前，我需要把相对应的语言环境部署完成。

#### Go 环境部署

- 下载并安装 Go 语言，[下载地址](https://go.dev/dl/)；

- 配置环境变量：

  - Mac：

    ```shell
    # 编辑 ~/.zshrc 在文件末尾添加 Go 代理及环境变量
    vim ~/.zshrc
    
    # Go 国内代理
    export GO111MODULE=off
    export GOPROXY=https://goproxy.cn
    # Go 环境变量
    export GOROOT=/usr/local/Cellar/go/1.17.8/libexec
    export GOPATH=$HOME/go
    export PATH=$GOROOT/bin:$GOPATH/bin:$PATH
    ```

    

  - Windows

    右键我的电脑 -> 属性 -> 高级系统设置 -> 环境变量

    ```properties
    # 添加用户变量 GOPATH（代码存放位置），D:\go 是 Go 工程代码存放位置
    变量名：GOPATH
    变量值：D:\go
    
    # 添加系统变量 GOROOT（Go 安装位置），C:\go 是 Go 代码存放位置
    变量名：GOPATH
    变量值：C:\go
    
    # 编辑环境变量，双击 PATH 点击新建，添加
    C:\go\bin
    ```

#### Python 环境部署

- 下载并安装 Python，[下载地址](https://www.python.org/downloads);

- 配置环境变量：

  - Mac

    ```shell
    # Mac 系统可以通过 homebrew 安装
    brew install python
    
    # 添加环境变量
    vim ~/.zshrc
    # 在文件末尾添加，
    export PATH="/usr/local/Cellar/python@3.9/3.9.10/bin:$PATH"
    ```

  - Windows

    右键我的电脑 -> 属性 -> 高级系统设置 -> 环境变量

    ```properties
    # 编辑环境变量，双击 PATH 点击新建，添加
    C:\Python\bin
    ```

#### NodeJS 环境部署

- 下载并安装 NodeJS，[下载地址](https://nodejs.org/zh-cn/download/);

- 配置环境变量：安装完成后，自动配置了环境变量，无需我们手动配置。

  - Mac 

    ```
    # Mac 系统可以通过 homebrew 安装
    brew install node
    ```

#### JDK 环境部署

- 下载并安装 OpenJDK，[下载地址](http://jdk.java.net/18);

- 配置环境变量：

  - Mac

    ```shell
    # Mac 系统可以通过 homebrew 安装，安装后自动添加环境变量
    brew install openjdk
    ```

  - Windows

    右键我的电脑 -> 属性 -> 高级系统设置 -> 环境变量

    ```properties
    # 新建系统变量，C:\jdk-18 是 JDK 存放位置
    变量名：JAVA_HOME
    变量值：C:\jdk-18
    
    # 新建系统变量
    变量名：CLASSPATH
    变量值：%JAVA_HOME%\bin;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
    
    # 编辑环境变量，双击 PATH 点击新建，添加
    %JAVA_HOME%\bin
    %JAVA_HOME%\jre\bin
    ```

#### Rust 环境部署

- 下载并安装 Rust，[Mac / Linux 安装说明地址](https://www.rust-lang.org/learn/get-started)，[其它版本安装说明地址](https://forge.rust-lang.org/infra/other-installation-methods.html)；

- 配置环境变量：

  - Mac

    ```shell
    # Mac 系统可以通过 homebrew 安装，安装后自动添加环境变量
    brew install openjdk
    ```

  - Windows

    下载 [i686-pc-windows-gnu](https://static.rust-lang.org/dist/rust-1.59.0-i686-pc-windows-gnu.msi) 并安装。右键我的电脑 -> 属性 -> 高级系统设置 -> 环境变量

    ```properties
    # 添加环境变量，xxx 为系统用户名
    C:\Users\xxx\.cargo\bin 
    ```



## Vim 插件安装

#### Vundle 安装

vim 插件管理器，能够搜索、安装、更新和移除 vim 相关插件。

```bash
# 下载 Vundle 到 bundle 文件夹
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

#### Vundle 配置

```nim
set nocompatible								" 关闭兼容模式
filetype off                  	" 关闭文件类型侦测

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " 启用自动补全
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```







## Vim 基本配置

编辑或新建 vimrc 文件 `vim ~/.vimrc`， 添加如下内容：

```nim
" ==============================================================================
" Vim 基本配置 
" ==============================================================================
" 设置 vimrc 修改保存后立刻生效，不用再重新打开
" 建议配置完成后将这个关闭
autocmd BufWritePost $MYVIMRC source $MYVIMRC

" =============== YCM 配置 ===============
set nocompatible								" 关闭兼容模式 YCM需求
filetype off                  	" 关闭文件类型侦测 YCM需求
  
" =============== 外观设置 ===============
set number                      " 显示行号
set showtabline=0               " 隐藏顶部标签栏
set guioptions-=r               " 隐藏右侧滚动条
set guioptions-=L               " 隐藏左侧滚动条
set guioptions-=b               " 隐藏底部滚动条
set cursorline                  " 突出显示当前行
set cursorcolumn                " 突出显示当前列
set langmenu=zh_CN.UTF-8        " 显示中文菜单
set helplang=cn 								" 帮助文档中文显示
set transparency=10							" 设置窗口透明度

" =============== 编码风格设置 ===============
syntax enable										" 开启文件类型检测
set encoding=utf-8							" 编码格式为UTF-8
set nu 													" 设置行号
set nowrap                      " 设置代码不折行"
set cursorline 									" 突出显示当前行
" set cursorcolumn 	  					" 突出显示当前列
set showmatch 									" 显示括号匹配
set tabstop=4 									" 设置Tab长度为4空格
set shiftwidth=4 								" 设置自动缩进长度为4空格
set backspace+=indent,eol,start " set backspace&可以对其重置
set autoindent 									" 继承前一行的缩进方式，适用于多行注释
set scrolloff=5                 " 距离顶部和底部5行
set laststatus=2                " 命令行为两行
let mapleader=";" 							" 定义快捷键的前缀，即<Leader>


" =============== 其它相关设置 ===============
set mouse=a                     " 启用鼠标
set selection=exclusive
set selectmode=mouse,key
set matchtime=5
set ignorecase                  " 搜索时大小写不敏感
set incsearch										" 开启实时搜索
set hlsearch                    " 高亮搜索项
set noexpandtab                 " 不允许扩展table
set whichwrap+=<,>,h,l
set autoread
filetype plugin indent on    		" 启用自动补全 YCM需求
" 退出插入模式指定类型的文件自动保存
"autocmd InsertLeave *.go,*.rs,*.sh,*.php,*.py,*.js,*.html,*.md write


" =============== 系统剪切板复制粘贴 ===============
" v 模式下复制内容到系统剪切板
vmap <Leader>c "+yy
" n 模式下复制一行到系统剪切板
nmap <Leader>c "+yy
" n 模式下粘贴系统剪切板的内容
nmap <Leader>v "+p








```







