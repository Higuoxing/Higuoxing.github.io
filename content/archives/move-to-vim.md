---
title: 迁移到 [NeoVim]
date: 2018-06-23 15:16:46
tags: ["Vim"]
draft: true
---

很早就想尝试一下 `NeoVim` 了，因为每次配置 Vim 有点太烦了，而且这次直接换用了 Vim-Plug 的包管理，比起原来配置起来更方便了，而且更快了。初次尝试还是不错的，尤其是 NeoVim 有一个 `:terminal` 的命令，可以调出一个 Buffer 来容纳一个 Terminal Emulator ，这样调试程序就很方便了。但是在使用过程有个小小的不舒服的地方就是，当输入 exit 退出当前 Terminal 的时候，整个 NeoVim 都会退出，Google 了之后貌似是 Terminal 的Buffer 和 普通的 Buffer 不太一样，所以导致了这个奇怪的问题。

<!--more-->

### Solution

一个解决方法是绑定一个打开 Terminal 的快捷键以及从 Terminal Mode 退出到 Normal Mode 的快捷键，这样有两个好处，一是能够利用自己舒服的键位打开命令行，二是直接退出到 Normal Mode 之后就可以按照正常的键位(`<ESC>` 退出 Terminal Mode，`<\>o` 打开 Terminal)操作了，不是很麻烦。而且这样就不需要额外的安装使用 Terminal 的插件了，调试起来程序也很方便了！

```vim
" Terminal Config {
     tnoremap <Esc> <C-\><C-n>
     nnoremap <leader>o :below 10sp term://$SHELL<cr>i
" }
```

其它方面的东西可以在 NeoVim 内输入 `:help terminal-emulator` 进行查看。我的 NeoVim 配置在[这里](https://github.com/Higuoxing/dot-file/blob/master/.nvim/init.vim)

### Plugins

```vim
"===--------------------------------------------
" Tool Plugins
"===--------------------------------------------
Plug 'vim-airline/vim-airline'                          " airline 状态栏
Plug 'scrooloose/nerdtree'                              " 左侧栏的文件树，很好用
Plug 'yggdroot/indentline'                              " 就是图中的缩进提示线，可以替换成自己喜欢的字符
Plug 'majutsushi/tagbar', { 'on': 'TagbarToggle' }      " 标签栏，不多说了，用 Vim 都知道
Plug 'jiangmiao/auto-pairs'                             " 自动补齐括号，引号等符号的小插件
Plug 'godlygeek/tabular'                                " 对齐插件

"===--------------------------------------------
" Apperance Plugins
"===--------------------------------------------
Plug 'dracula/vim', { 'as': 'dracula' }  " dracula 主题，我最喜欢的暗色主题
Plug 'vim-airline/vim-airline-themes'    " 导航栏的主题，可有可无
Plug 'kien/rainbow_parentheses.vim'      " 🌈 括号，多括号的时候比较直观的看出来括号对(不过通常情况下我是禁用的)
```

上面就是我常用的一些插件了，一些顺手的小工具可以让工作效率提高很多，比如说，经常写 `Verilog`，那么有强迫症的人，可能比较喜欢将一些关键字，或者符号对齐，这样会看起来比较顺眼。写 `C++/C` 时候，又会需要写很多的括号，所以需要一个自动匹配括号的小插件就比较舒服了。

### Tabular

Tabular 插件是我在裕浩哥[那里](https://corvo.myseu.cn/2018/07/31/2018-07-31-Linux%E5%B0%8F%E6%8A%80%E5%B7%A7(%E4%B8%89)/)看来的，超级方便！

```verilog
assign var1 = 4'b0000;
assign var11 = 4'b0001;
assign var111 = 4'b0010;
assign var1111 = 4'b0011;
assign var11111 = 4'b0100;
```

这里的 Verilog 看起来就像 js 三角形的 callback hell 一样，不是很爽，所以利用 tabular 这个小插件来做一些有趣的事情。tabular 的语法很简单 `:Tab /<pattern>` 当我对上面的文本在 Visual Mode 输入 `:Tab /=` 就会变成

```verilog
assign var1     = 4'b0000;
assign var11    = 4'b0001;
assign var111   = 4'b0010;
assign var1111  = 4'b0011;
assign var11111 = 4'b0100;
```

怎么样，很方便吧！在写 Markdown 表格的时候也会十分方便，比如

```markdown
| item0 | item1 | item2 |
| a | b | c |
```

向上面一样，输入 `:Tab /|` 就可以了！当然，绑定一个快捷键会更舒服！

```markdown
| item0 | item1 | item2 |
| a     | b     | c     |
```

