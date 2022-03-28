# Mac Vim 开发环境的部署与配置



[Vim下载地址](https://www.vim.org/download.php)，分为适用 Mac 的 [MacVim](https://github.com/macvim-dev/macvim/releases)，适用 MS-Windows 系统的 [GVim](https://www.vim.org/download.php#pc)，适用 Unix 的 [Vim](https://www.vim.org/git.php)。

```shell
# Mac 系统可以下载 MacVim.dmg 安装，也可以通过 homebrew 安装
brew cask install macvim --with-override-system-vim

# 编辑 ~/.bash_profile 在文件末尾添加
vim ~/.bash_profile

alias vim="/Applications/MacVim.app/Contents/MacOS/Vim"
alias mvim="/Applications/MacVim.app/Contents/Ma cOS/MacVim"
```

## Vim 插件安装

#### Vundle 插件管理器安装

vim 插件管理器，能够搜索、安装、更新和移除 vim 相关插件。

```bash
# 下载 Vundle 到 bundle 文件夹
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

#### Vundle 配置，安装插件的几种方式：

- 从GitHub仓库安装，格式为`Plugin 'GitHub用户名/插件仓库名'`，如`Plugin 'tpope/vim-fugitive' `

- 来自 http://vim-scripts.org/vim/scripts.html 的插件，[GitHub地址](https://github.com/vim-scripts?tab=repositories)，格式为`Plugin '插件名称'`，如`Plugin 'L9'`

- 由Git支持但不在GitHub上的插件仓库安装，格式为`Plugin 'git_address'`，

  如`Plugin 'https://gitee.com/yanzhongqian/nerdtree.git`

- 从本地Git仓库的插件安装，格式为`Plugin 'file:///home/gmarik/path/to/plugin'`,

  如`Plugin '/Users/biaowong/.vim/plugins/go_fmt_customer'

- 如果插件名称重复，可以通过定义别名的方式避免冲突，如L9插件冲突，可以通过

  `Plugin 'ascenator/L9', {'name': 'newL9'}`重新指定插件名称的方式解决冲突问题

```nim
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" 插件列表
" vim编辑器中的Git包装器插件，它可以让我们在vim编辑器中完成git操作
Plugin 'tpope/vim-fugitive'
" 用来提供一个导航目录的侧边栏
Plugin 'scrooloose/nerdtree'
" 可以使 nerdtree 的 tab 更加友好些
Plugin 'jistr/vim-nerdtree-tabs'
" 可以在导航目录中看到 git 版本信息
Plugin 'Xuyuanp/nerdtree-git-plugin'
" 可以在文档中显示 git 信息
Plugin 'airblade/vim-gitgutter'
" 查看当前代码文件中的变量和函数列表的插件，
" 可以切换和跳转到代码中对应的变量和函数的位置
" 大纲式导航, Go 需要 https://github.com/jstemmer/gotags 支持
Plugin 'majutsushi/tagbar'
" 自动补全括号的插件，包括小括号，中括号，以及花括号
Plugin 'jiangmiao/auto-pairs'
" Vim状态栏插件，包括显示行号，列号，文件类型，文件名，以及Git状态
Plugin 'vim-airline/vim-airline'
" 有道词典在线翻译
Plugin 'ianva/vim-youdao-translater'
" 代码自动完成，安装完插件还需要额外配置才可以使用
Plugin 'Valloric/YouCompleteMe'
" 下面两个插件要配合使用，可以自动生成代码块
Plugin 'SirVer/ultisnips'
Plugin 'honza/vim-snippets'
" Markdown 插件
Plugin 'iamcco/mathjax-support-for-mkdp'
Plugin 'iamcco/markdown-preview.vim'
" 高亮显示多余空格并一键去除
Plugin 'bronson/vim-trailing-whitespace'
" 状态栏插件，需要安装powerline字体
Plugin 'powerline/powerline', {'rtp': 'powerline/bindings/vim'}
" 快速注释插件
" let g:NERDSpaceDelims=1 " 注释的时候自动加个空格
Plugin 'preservim/nerdcommenter'


" Go 相关
" Go 主要插件
Plugin 'fatih/vim-go', { 'tag': '*' }
" Go 中的代码追踪，输入 gd 就可以自动跳转
Plugin 'dgryski/vim-godef'

" Python 相关
Plugin 'vim-scripts/indentpython.vim'
Plugin 'tell-k/vim-autopep8'


" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " 启用自动补全

```



## Vim 基本配置

编辑或新建 vimrc 文件 `vim ~/.vimrc`， 添加如下内容：

```shell
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


" =============== 屏幕切割 ===============
" 指定屏幕上可以进行分割布局的区域
set splitbelow               " 允许在下部分割布局
set splitright               " 允许在右侧分隔布局
" 组合快捷键：
nnoremap <C-J> <C-W><C-J>    " 组合快捷键：- Ctrl-j 切换到下方的分割窗口
nnoremap <C-K> <C-W><C-K>    " 组合快捷键：- Ctrl-k 切换到上方的分割窗口
nnoremap <C-L> <C-W><C-L>    " 组合快捷键：- Ctrl-l 切换到右侧的分割窗口
nnoremap <C-H> <C-W><C-H>    " 组合快捷键：- Ctrl-h 切换到左侧的分割窗口



" =============== Vundle 配置 ===============
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" 插件列表
" vim编辑器中的Git包装器插件，它可以让我们在vim编辑器中完成git操作
Plugin 'tpope/vim-fugitive'
" 用来提供一个导航目录的侧边栏
Plugin 'scrooloose/nerdtree'
" 在NERDTree中显示图片
Plugin 'ryanoasis/vim-devicons'
" 可以使 nerdtree 的 tab 更加友好些
Plugin 'jistr/vim-nerdtree-tabs'
" 可以在文件目录中看到 git 版本信息
Plugin 'Xuyuanp/nerdtree-git-plugin'
" 可以在文档中显示 git 信息
Plugin 'airblade/vim-gitgutter'
" 查看当前代码文件中的变量和函数列表的插件，
" 可以切换和跳转到代码中对应的变量和函数的位置
" 大纲式导航, Go 需要 https://github.com/jstemmer/gotags 支持
Plugin 'preservim/tagbar'
" 自动补全括号的插件，包括小括号，中括号，以及花括号
Plugin 'jiangmiao/auto-pairs'
" Vim状态栏插件，包括显示行号，列号，文件类型，文件名，以及Git状态
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
" 有道词典在线翻译
Plugin 'ianva/vim-youdao-translater'
" 代码自动完成，安装完插件还需要额外配置才可以使用
Plugin 'ycm-core/YouCompleteMe'
" 代码缩进提示
Plugin 'Yggdroot/indentLine'
" 代码折叠
Plugin 'tmhedberg/SimpylFold'
" 下面两个插件要配合使用，可以自动生成代码块
Plugin 'SirVer/ultisnips'
Plugin 'honza/vim-snippets'
" Markdown 插件，预览数学插件
Plugin 'iamcco/mathjax-support-for-mkdp'
" 在浏览器预览 Markdown 文档
Plugin 'iamcco/markdown-preview.vim'
" 高亮显示多余空格并一键去除
Plugin 'bronson/vim-trailing-whitespace'
" 状态栏插件，需要安装powerline字体
"Plugin 'powerline/powerline', {'rtp': 'powerline/bindings/vim'}
" 快速注释插件
" let g:NERDSpaceDelims=1 " 注释的时候自动加个空格
Plugin 'preservim/nerdcommenter'
" Ctrl + p，实现模糊匹配快速打开文件等功能
" Plugin 'kien/ctrlp.vim'
" 无论是从性能还是匹配精度上，都远远超越ctrlp，
" 快速打开或定位某个buffer、最近使用的文件（mru）、tags（包括函数、类、变量等）、
" 命令历史、文件中的某一行、vim的help、marks等
Plugin 'Yggdroot/LeaderF', { 'do': './install.sh' }
" 这个插件其实是上边ctrlp插件的一个补充，它主要是提升了文件查找的速度
Plugin 'FelikZ/ctrlp-py-matcher'

" Go 相关
" go 主要插件
Plugin 'fatih/vim-go', { 'tag': '*' }
" go 中的代码追踪，输入 gd 就可以自动跳转
Plugin 'dgryski/vim-godef'

" Python 相关
Plugin 'vim-scripts/indentpython.vim'
Plugin 'tell-k/vim-autopep8'


" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " 启用自动补全


" =============== NERDTree 配置 ===============
" 使用F3键快速调出和隐藏它
map <F3> :NERDTreeToggle<CR>

let NERDTreeChDirMode=1
" 显示书签"
let NERDTreeShowBookmarks=1
" 设置忽略文件类型"
let NERDTreeIgnore=['\~$', '\.pyc$', '\.swp$']
" 窗口大小"
let NERDTreeWinSize=25
" 修改默认箭头
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'
" How can I open a NERDTree automatically when vim starts up if no files were specified?
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
" 打开vim时自动打开NERDTree
autocmd vimenter * NERDTree
" How can I open NERDTree automatically when vim starts up on opening a directory?
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif
" 关闭vim时，如果打开的文件除了NERDTree没有其他文件时，它自动关闭，减少多次按:q!
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
" 显示行号
let NERDTreeShowLineNumbers=1
let NERDTreeAutoCenter=1
" 在终端启动vim时，共享NERDTree
let g:nerdtree_tabs_open_on_console_startup=1


" =============== vim-devicons 配置 ===============
"set encoding=UTF-8 " 已设置
"Can be enabled or disabled
let g:webdevicons_enable_nerdtree = 1
"whether or not to show the nerdtree brackets around flags
let g:webdevicons_conceal_nerdtree_brackets = 1
"adding to vim-airline's tabline
let g:webdevicons_enable_airline_tabline = 1
"adding to vim-airline's statusline
let g:webdevicons_enable_airline_statusline = 1


" =============== vim-nerdtree-tabs 配置 ===============
map <Leader>n <plug>NERDTreeTabsToggle<CR>

s
" =============== nerdtree-git-plugin 配置 ===============
" 开发的过程中，我们希望git信息直接在NERDTree中显示出来， 
" 和Eclipse一样，修改的文件和增加的文件都给出相应的标注， 
" 这时需要安装的插件就是 nerdtree-git-plugin,配置信息如下
let g:NERDTreeGitStatusIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ "Unknown"   : "?"
    \ }


" =============== tagbar 配置 ===============
nmap <F9> :TagbarToggle<CR>
let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds'     : [
        \ 'p:package',
        \ 'i:imports:1',
        \ 'c:constants',
        \ 'v:variables',
        \ 't:types',
        \ 'n:interfaces',
        \ 'w:fields',
        \ 'e:embedded',
        \ 'm:methods',
        \ 'r:constructor',
        \ 'f:functions'
    \ ],
    \ 'sro' : '.',
    \ 'kind2scope' : {
        \ 't' : 'ctype',
        \ 'n' : 'ntype'
    \ },
    \ 'scope2kind' : {
        \ 'ctype' : 't',
        \ 'ntype' : 'n'
    \ },
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
\ }


" =============== vim-airline 配置 ===============
" 去这里去选主题
" https://github.com/vim-airline/vim-airline-themes/tree/master/autoload/airline/themes
let g:airline_theme="luna" 

" 这个是安装字体后 必须设置此项
let g:airline_powerline_fonts = 1
 
" 打开tabline功能,方便查看Buffer和切换，这个功能比较不错
" 我还省去了minibufexpl插件，因为我习惯在1个Tab下用多个buffer
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#buffer_nr_show = 1

" 设置切换Buffer快捷键
nnoremap <C-N> :bn<CR>
nnoremap <C-P> :bp<CR>

" 关闭状态显示空白符号计数,这个对我用处不大
let g:airline#extensions#whitespace#enabled = 0
let g:airline#extensions#whitespace#symbol = '!'

" 在Gvim中我设置了英文用Hermit， 中文使用 YaHei Mono
if has('win32')
    set guifont=Hermit:h13
    set guifontwide=Microsoft_YaHei_Mono:h12
endif


" =============== vim-youdao-translater 配置 ===============
vnoremap <silent> <C-T> :<C-u>Ydv<CR>
nnoremap <silent> <C-T> :<C-u>Ydc<CR>
noremap <leader>yd :<C-u>Yde<CR>


" =============== YouCompleteMe 配置 ===============
" 补全菜单的开启与关闭
set completeopt=longest,menu                    " 让Vim的补全菜单行为与一般IDE一致(参考VimTip1228)
let g:ycm_min_num_of_chars_for_completion=2             " 从第2个键入字符就开始罗列匹配项
let g:ycm_cache_omnifunc=0                      " 禁止缓存匹配项,每次都重新生成匹配项
let g:ycm_autoclose_preview_window_after_completion=1       " 智能关闭自动补全窗口
"autocmd InsertLeave * if pumvisible() == 0|pclose|endif         " 离开插入模式后自动关闭预览窗口

" 补全菜单中各项之间进行切换和选取：默认使用tab  s-tab进行上下切换，使用空格选取。可进行自定义设置：
"let g:ycm_key_list_select_completion=['<c-n>']
"let g:ycm_key_list_select_completion = ['<Down>']      " 通过上下键在补全菜单中进行切换
"let g:ycm_key_list_previous_completion=['<c-p>']
"let g:ycm_key_list_previous_completion = ['<Up>']
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>"    " 回车即选中补全菜单中的当前项

" 开启各种补全引擎
let g:ycm_collect_identifiers_from_tags_files=1             " 开启 YCM 基于标签引擎
let g:ycm_auto_trigger = 1                                  " 开启 YCM 基于标识符补全，默认为1
let g:ycm_seed_identifiers_with_syntax=1                    " 开启 YCM 基于语法关键字补全
let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全
let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全
let g:ycm_collect_identifiers_from_comments_and_strings = 0 " 注释和字符串中的文字也会被收入补全

" 重映射快捷键
"上下左右键的行为 会显示其他信息,inoremap由i 插入模式和noremap不重映射组成，只映射一层，不会映射到映射的映射
inoremap <expr> <Down>     pumvisible() ? "\<C-n>" : "\<Down>"
inoremap <expr> <Up>       pumvisible() ? "\<C-p>" : "\<Up>"
inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"

"nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>           " force recomile with syntastic
"nnoremap <leader>lo :lopen<CR>    "open locationlist
"nnoremap <leader>lc :lclose<CR>    "close locationlist
"inoremap <leader><leader> <C-x><C-o>

nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> " 跳转到定义处
let g:ycm_confirm_extra_conf=0  " 关闭加载.ycm_extra_conf.py确认提示


" =============== indentline 配置 ===============
" 支持任意ASCII码，也可以使用特殊字符：¦, ┆, or │ ，但只在utf-8编码下有效
let g:indentLine_char='¦'
" 使indentline生效
let g:indentLine_enabled = 1


" =============== SimpylFold 配置 ===============
" 必须手动输入za来折叠（和取消折叠）
set foldmethod=indent                " 根据每行的缩进开启折叠
set foldlevel=99
" 使用空格键会是更好的选择,所以在你的配置文件中加上这一行命令吧：
nnoremap <space> za
" 希望看到折叠代码的文档字符串？
let g:SimpylFold_docstring_preview=1


" =============== markdown-preview 配置 ===============
" 设置 chrome 浏览器的路径（或是启动 chrome（或其他现代浏览器）的命令）
" 如果设置了该参数, g:mkdp_browserfunc 将被忽略
let g:mkdp_path_to_chrome = "chrome"
" vim 回调函数, 参数为要打开的 url
let g:mkdp_browserfunc = 'MKDP_browserfunc_default'
" 设置为 1 可以在打开 markdown 文件的时候自动打开浏览器预览，只在打开markdown 文件的时候打开一次
let g:mkdp_auto_start = 1
" 设置为 1 在编辑 markdown 的时候检查预览窗口是否已经打开，否则自动打开预览窗口
let g:mkdp_auto_open = 1
" 在切换 buffer 的时候自动关闭预览窗口，设置为 0 则在切换 buffer 的时候不自动关闭预览窗口
let g:mkdp_auto_close = 1
" 设置为 1 则只有在保存文件，或退出插入模式的时候更新预览，默认为 0，实时更新预览
let g:mkdp_refresh_slow = 0
" 设置为 1 则所有文件都可以使用 MarkdownPreview 进行预览，默认只有 markdown
let g:mkdp_command_for_global = 0
" 设置为 1, 在使用的网络中的其他计算机也能访问预览页面
" 默认只监听本地（127.0.0.1），其他计算机不能访问
let g:mkdp_open_to_the_world = 0
nmap <silent> <F8> <Plug>MarkdownPreview        	" 普通模式
imap <silent> <F8> <Plug>MarkdownPreview        	" 插入模式
nmap <silent> <C-F8> <Plug>StopMarkdownPreview    " 普通模式
imap <silent> <C-F8> <Plug>StopMarkdownPreview    " 插入模式



" =============== NerdCommenter 配置 ===============
let g:NERDSpaceDelims=1	" 注释的时候自动加个空格
nmap <leader>cc   			" 加注释
nmap <leader>cu   			" 解开注释
nmap <leader>ca 				" 切换注释的样式:/*....*/和//..的切换
nmap <leader>c<space> 	" 加上/解开注释, 智能判断
nmap <leader>cy   			" 先复制, 再注解，p可以进行黏贴
nmap <leader>cs  				" '性感的'注释
```



## YouCompleteMe 安装

因为我是多语言环境开发，经常会用到 Go，Python，Rust，PHP，JS，HTML等，所以 YCM 插件我要配置全语言安装。因此在配置 YCM 之前，我需要把相对应的语言环境部署完成。

```properties
# 安装多语言支持环境
brew install cmake go python rust nodejs openjdk
# 编辑 ~/.bash_profile 在文件末尾添加 Go 代理及环境变量
vim ~/.bash_profile

# Go 国内代理
export GO111MODULE=off
export GOPROXY=https://goproxy.cn
# Go 环境变量
export GOROOT=/usr/local/Cellar/go/1.17.8/libexec
export GOPATH=$HOME/go
export PATH=$GOROOT/bin:$GOPATH/bin:$PATH

# Python 环境变量
export PATH="/usr/local/Cellar/python@3.9/3.9.10/bin:$PATH"

#:wq 保存退出

# Vundle 安装，或通过 git 拉去代码手动安装
git clone https://github.com/ycm-core/YouCompleteMe.git ~/.vim/bundle
# --all 全语言支持安装。确保 xbuild, go, node and npm 添加到了 `PATH` 环境变量中:
cd ~/.vim/bundle/YouCompleteMe
git submodule update --init --recursive
./install.py --all
```

---

## 以下内容可忽略不看

---

## 插件说明

#### vim-fugitive

vim-fugitive 可以让我们在 Vim 命令行中运行任何的 Git 相关命令。如：`:Git ...`或`:G ...`。

|  命令   | 说明                                                         |
| :-----: | :----------------------------------------------------------- |
| Gblame  | Gblame其实就是执行git blame命令，然后直接在vim中将git的输出结果与源代码一一对应起来。这样当你读团队代码的时候发现了一个坑，然后想知道是谁写的这个坑的时候，只需要:Gblame一下，结果立马呈现。 |
| Gbrowse | Gbrowse是当你想分享一段代码给别人的时候，可以直接选中你想分享的代码，可以是单行也可以是多行，然后执行:Gbrowse，它就会帮你在浏览器中打开github的地址，然后直接定位到你所选的代码行数。被选中的代码会被高亮显示。用这种方式分享代码可比截图或复制粘贴优雅多了。但是它只支持存放在github中的代码仓库，如果你的代码放在gitlab或其他仓库中，这个插件是无法打开的。 |

#### NerdTree

文件管理插件，可以侧边栏显示或关闭NERDTree，并对NERDTree进行相关操作。

配置文件：

```shell
"使用F3键快速调出和隐藏它
map <F3> :NERDTreeToggle<CR>

let NERDTreeChDirMode=1
"显示书签"
let NERDTreeShowBookmarks=1
"设置忽略文件类型"
let NERDTreeIgnore=['\~$', '\.pyc$', '\.swp$']
"窗口大小"
let NERDTreeWinSize=25
" 修改默认箭头
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'
"How can I open a NERDTree automatically when vim starts up if no files were specified?
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
" 打开vim时自动打开NERDTree
autocmd vimenter * NERDTree
"How can I open NERDTree automatically when vim starts up on opening a directory?
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif
" 关闭vim时，如果打开的文件除了NERDTree没有其他文件时，它自动关闭，减少多次按:q!
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
" 显示行号
let NERDTreeShowLineNumbers=1
let NERDTreeAutoCenter=1
" 在终端启动vim时，共享NERDTree
let g:nerdtree_tabs_open_on_console_startup=1
```



操作方法：

```properties
?: 快速帮助文档
o: 打开一个目录或者打开文件，创建的是buffer，也可以用来打开书签
go: 打开一个文件，但是光标仍然留在NERDTree，创建的是buffer
t: 打开一个文件，创建的是Tab，对书签同样生效
T: 打开一个文件，但是光标仍然留在NERDTree，创建的是Tab，对书签同样生效
i: 水平分割创建文件的窗口，创建的是buffer
gi: 水平分割创建文件的窗口，但是光标仍然留在NERDTree
s: 垂直分割创建文件的窗口，创建的是buffer
gs: 和gi，go类似
x: 收起当前打开的目录
X: 收起所有打开的目录
e: 以文件管理的方式打开选中的目录
D: 删除书签
P: 大写，跳转到当前根路径
p: 小写，跳转到光标所在的上一级路径
K: 跳转到第一个子路径
J: 跳转到最后一个子路径
<C-j>和<C-k>: 在同级目录和文件间移动，忽略子目录和子文件
C: 将根路径设置为光标所在的目录
u: 设置上级目录为根路径
U: 设置上级目录为跟路径，但是维持原来目录打开的状态
r: 刷新光标所在的目录
R: 刷新当前根路径
I: 显示或者不显示隐藏文件
f: 打开和关闭文件过滤器
q: 关闭NERDTree
A: 全屏显示NERDTree，或者关闭全屏
```

##### vim-devicons

在NerdTree中显示文件类型图标，这个插件不光支持NerdTree，还支持vim-airline、CtrlP、powerline等。

配置：

```shell
set encoding=UTF-8
"Can be enabled or disabled
let g:webdevicons_enable_nerdtree = 1
"whether or not to show the nerdtree brackets around flags
let g:webdevicons_conceal_nerdtree_brackets = 1
"adding to vim-airline's tabline
let g:webdevicons_enable_airline_tabline = 1
"adding to vim-airline's statusline
let g:webdevicons_enable_airline_statusline = 1
```

#### vim-nerdtree-tabs

共享 NERDTree 状态。在编辑当前文件，切换到 NERDTree，选中其它文件按 t 打开新标签，保留 NERDTree 状态。

配置文件：

```shell
map <Leader>n <plug>NERDTreeTabsToggle<CR>
```

操作方法：

```properties
:NERDTreeTabsOpen switches NERDTree on for all tabs.
:NERDTreeTabsClose switches NERDTree off for all tabs.
:NERDTreeTabsToggle toggles NERDTree on/off for all tabs.
:NERDTreeTabsFind find currently opened file and select it
:NERDTreeMirrorOpen acts as :NERDTreeMirror, but smarter: When opening, it first tries to use an existing tree (i.e. previously closed in this tab or perform a mirror of another tab's tree). If all this fails, a new tree is created. It is recommended that you use this command instead of :NERDTreeMirror.
:NERDTreeMirrorToggle toggles NERDTree on/off in current tab, using the same behavior as :NERDTreeMirrorOpen.
:NERDTreeSteppedOpen focuses the NERDTree, opening one first if none is present.
:NERDTreeSteppedClose unfocuses the NERDTree, or closes/hides it if it was not focused.
:NERDTreeFocusToggle focus the NERDTree or create it if focus is on a file, unfocus NERDTree if focus is on NERDTree
```

#### nerdtree-git-plugin

能够在 NERDTree 文件栏中显示 Git 状态。

```shell
" 开发的过程中，我们希望git信息直接在NERDTree中显示出来， 
" 和Eclipse一样，修改的文件和增加的文件都给出相应的标注， 
" 这时需要安装的插件就是 nerdtree-git-plugin,配置信息如下
let g:NERDTreeGitStatusIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",s
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ "Unknown"   : "?"
    \ }
```

#### vim-gitgutter

让 vim 对新增、删除、修改等操作时，都有颜色标记提醒。在sign列中显示git diff标记并进行阶段预览撤销块和部分块。

#### tagbar

类似SublimeText右侧属性列表展示效果。

配置:

```properties
nmap <F9> :TagbarToggle<CR>
let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds'     : [
        \ 'p:package',
        \ 'i:imports:1',
        \ 'c:constants',
        \ 'v:variables',
        \ 't:types',
        \ 'n:interfaces',
        \ 'w:fields',
        \ 'e:embedded',
        \ 'm:methods',
        \ 'r:constructor',
        \ 'f:functions'
    \ ],
    \ 'sro' : '.',
    \ 'kind2scope' : {
        \ 't' : 'ctype',
        \ 'n' : 'ntype'
    \ },
    \ 'scope2kind' : {
        \ 'ctype' : 't',
        \ 'ntype' : 'n'
    \ },
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
\ }
```

#### auto-pairs

在输入或删除左括号时，能自动补上或删除右括号。

```properties
" 去这里去选主题
" https://github.com/vim-airline/vim-airline-themes/tree/master/autoload/airline/themes
let g:airline_theme="luna" 

" 这个是安装字体后 必须设置此项
let g:airline_powerline_fonts = 1   
 
" 打开tabline功能,方便查看Buffer和切换，这个功能比较不错
" 我还省去了minibufexpl插件，因为我习惯在1个Tab下用多个buffer
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#buffer_nr_show = 1

" 设置切换Buffer快捷键
nnoremap <C-N> :bn<CR>
nnoremap <C-P> :bp<CR>

" 关闭状态显示空白符号计数,这个对我用处不大
let g:airline#extensions#whitespace#enabled = 0
let g:airline#extensions#whitespace#symbol = '!'

" 在Gvim中我设置了英文用Hermit， 中文使用 YaHei Mono
if has('win32')
    set guifont=Hermit:h13
    set guifontwide=Microsoft_YaHei_Mono:h12
endif
```

#### vim-youdao-translater

在普通模式下，按 `,y`， 会翻译当前光标下的单词。

配置文件：

```properties
vnoremap <silent> <C-T> :<C-u>Ydv<CR>
nnoremap <silent> <C-T> :<C-u>Ydc<CR>
noremap <leader>yd :<C-u>Yde<CR>
```

操作方法：

```properties
在 visual 模式下选中单词或语句，按 ,y，会翻译选择的单词或语句；
:Ydc 翻译当前光标下单词
:Ydv 翻译在 visual 模式下选中单词或语句
:Yde 手动输入单词
```

#### YouCompleteMe

一款非常强大的代码自动补全插件，用`TAB键`切换选择。

配置文件：

```properties
" 补全菜单的开启与关闭
set completeopt=longest,menu                    " 让Vim的补全菜单行为与一般IDE一致(参考VimTip1228)
let g:ycm_min_num_of_chars_for_completion=2             " 从第2个键入字符就开始罗列匹配项
let g:ycm_cache_omnifunc=0                      " 禁止缓存匹配项,每次都重新生成匹配项
let g:ycm_autoclose_preview_window_after_completion=1       " 智能关闭自动补全窗口
"autocmd InsertLeave * if pumvisible() == 0|pclose|endif         " 离开插入模式后自动关闭预览窗口

" 补全菜单中各项之间进行切换和选取：默认使用tab  s-tab进行上下切换，使用空格选取。可进行自定义设置：
"let g:ycm_key_list_select_completion=['<c-n>']
"let g:ycm_key_list_select_completion = ['<Down>']      " 通过上下键在补全菜单中进行切换
"let g:ycm_key_list_previous_completion=['<c-p>']
"let g:ycm_key_list_previous_completion = ['<Up>']
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>"    " 回车即选中补全菜单中的当前项

" 开启各种补全引擎
let g:ycm_collect_identifiers_from_tags_files=1             " 开启 YCM 基于标签引擎
let g:ycm_auto_trigger = 1                                  " 开启 YCM 基于标识符补全，默认为1
let g:ycm_seed_identifiers_with_syntax=1                    " 开启 YCM 基于语法关键字补全
let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全
let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全
let g:ycm_collect_identifiers_from_comments_and_strings = 0 " 注释和字符串中的文字也会被收入补全

" 重映射快捷键
"上下左右键的行为 会显示其他信息,inoremap由i 插入模式和noremap不重映射组成，只映射一层，不会映射到映射的映射
inoremap <expr> <Down>     pumvisible() ? "\<C-n>" : "\<Down>"
inoremap <expr> <Up>       pumvisible() ? "\<C-p>" : "\<Up>"
inoremap <expr> <PageDown> pumvisible() ? "\<PageDown>\<C-p>\<C-n>" : "\<PageDown>"
inoremap <expr> <PageUp>   pumvisible() ? "\<PageUp>\<C-p>\<C-n>" : "\<PageUp>"

"nnoremap <F5> :YcmForceCompileAndDiagnostics<CR>           " force recomile with syntastic
"nnoremap <leader>lo :lopen<CR>    "open locationlist
"nnoremap <leader>lc :lclose<CR>    "close locationlist
"inoremap <leader><leader> <C-x><C-o>

nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR> " 跳转到定义处
let g:ycm_confirm_extra_conf=0  " 关闭加载.ycm_extra_conf.py确认提示
```

#### indentline

代码缩进提示插件。

```properties
" 支持任意ASCII码，也可以使用特殊字符：¦, ┆, or │ ，但只在utf-8编码下有效
let g:indentLine_char='¦'
" 使indentline生效
let g:indentLine_enabled = 1
```

#### SimpylFold

代码快速折叠插件。

```properties
" 必须手动输入za来折叠（和取消折叠）
set foldmethod=indent                " 根据每行的缩进开启折叠
set foldlevel=99
" 使用空格键会是更好的选择,所以在你的配置文件中加上这一行命令吧：
nnoremap <space> za
" 希望看到折叠代码的文档字符串？
let g:SimpylFold_docstring_preview=1
```

#### ultisnips

Ultisnips 插件安装分两部分，一个是 ultisnips 插件本身，另外一个是代码片段仓库。一般来说把默认的代码片段仓库下载下来按需修改后上传到自己的 github 即可。

#### vim-snippets

自己的代码片段仓库，按需修改维护。配合 ultisnips 插件才能生效。可以下载 https://github.com/honza/vim-snippets这个 vim-snippets 库，按需修改后上传到自己的 GitHub 中。

#### mathjax-support-for-mkdp

Markdown 数学公式预览插件。

#### markdown-preview.vim

Markdown 浏览器预览插件。

配置文件：

```properties
" 设置 chrome 浏览器的路径（或是启动 chrome（或其他现代浏览器）的命令）
" 如果设置了该参数, g:mkdp_browserfunc 将被忽略
let g:mkdp_path_to_chrome = "chrome"
" vim 回调函数, 参数为要打开的 url
let g:mkdp_browserfunc = 'MKDP_browserfunc_default'
" 设置为 1 可以在打开 markdown 文件的时候自动打开浏览器预览，只在打开markdown 文件的时候打开一次
let g:mkdp_auto_start = 1
" 设置为 1 在编辑 markdown 的时候检查预览窗口是否已经打开，否则自动打开预览窗口
let g:mkdp_auto_open = 1
" 在切换 buffer 的时候自动关闭预览窗口，设置为 0 则在切换 buffer 的时候不自动关闭预览窗口
let g:mkdp_auto_close = 1
" 设置为 1 则只有在保存文件，或退出插入模式的时候更新预览，默认为 0，实时更新预览
let g:mkdp_refresh_slow = 0
" 设置为 1 则所有文件都可以使用 MarkdownPreview 进行预览，默认只有 markdown
let g:mkdp_command_for_global = 0
" 设置为 1, 在使用的网络中的其他计算机也能访问预览页面
" 默认只监听本地（127.0.0.1），其他计算机不能访问
let g:mkdp_open_to_the_world = 0
```

操作方法：

```properties
nmap <silent> <F8> <Plug>MarkdownPreview        	" 普通模式
imap <silent> <F8> <Plug>MarkdownPreview        	" 插入模式
nmap <silent> <C-F8> <Plug>StopMarkdownPreview    " 普通模式
imap <silent> <C-F8> <Plug>StopMarkdownPreview    " 插入模式
```











