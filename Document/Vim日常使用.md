---
title: vim 个人快捷键记录 
tags: 规则文档
notebook: wiki
---

## 移动操作

> w 向右移动到文字结尾  
e 移动到符号结尾  
b 当前行后退  
control + f 向下翻页  
control + n 向前翻页  
control + ] 跳转到对应的定义位置  
control + o 会退到原来的位置  
`t,`跳转到下一个逗号的位置  
`fa` 跳转到a的位置  
gd 跳转到这个局部变量的定义处  
dw 移动贯标到单词头，可以删除该单词  
d$ 删除光标到行尾的字符  
d2w 光标移动到2个大写单词，输入d2w删除单词  
dd 删除当前行后，输入p粘贴到光标下  

# vim插件操作

- NERDTree 打开导航栏
- NERDTreeClose 关闭导航栏
- CommandT 快速搜索文件命令

# 状态栏插件安装
>Airline 可以在 vim 中显示更友好的状态栏，再配以漂亮的配色
Airline https://github.com/vim-airline/vim-airline
Airline Themes https://github.com/vim-airline/vim-airline-themes
添加 .vimrc 配置
Bundle 'vim-airline/vim-airline'
Bundle 'vim-airline/vim-airline-themes'
执行 :PlugInstall
基本配置:
set laststatus=2    " 始终显示状态栏
" Airline
let g:airline#extensions#tabline#enabled=1    " 开启 tab 栏
执行 :AirlineTheme simple 应用 simple 样式，或者修改 .vimrc 文件，添加如