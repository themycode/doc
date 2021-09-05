---
title: vim 个人快捷键记录 
tags: 规则文档
notebook: wiki
---
## 编辑操作 可视模式 （选取功能）

如果编辑代码或需要行内容的编辑，需要编辑引号中文本或者是替换

**【vaw】指令 选中第一个单词**

- v 是 选择模式
- a 是 [a]一个
- w 是 word单词
vaw 适用的场景并不多，但是可以将va和vi当成一组独立的指令

- 命令【vi"c】 选中双引号中间的文本，c在进行替换
- 命令【va)】选中括号中的内容
system.out.print("hello world");
system.out.print("hello world");
system.out.print("hello world");

- 命令 【v2i)】 选择第二个小括号的范围

- 命令 【vat、vit】指令
`t 代表的是 tag`
网页html文件中的xml标签格式
【vit】 -> 两个标签中的文字
【vat】 -> 包含标签的整个元素

```html
<HTML>
<body>
    // vit vat 快速选择tag
    <a>hello world</a>
</body>
</HTML>
```

选择模式的三种状态：
小写v指令：选取以当前行为单位
大写V指令：以行为单位，左下方出现VISUAL LINE，选取方式，仅可以使用垂直的移动指令
control + v指令：垂直的方向进行选取，多行编辑功能

增删该操作：
c 指令 替换
d 指令 删除
y 指令 复制
p 指令 粘贴
y$ 拷贝到行尾的内容

## 标签书签指令

m 代表的是mark ，然后在加任意 字母 或者数字

例如 【 mq 指令】 当前光标的位置，记录在q的寄存器中
移动到其他的位置，想要快速的跳回刚才的地方
【`q 指令】 跳回到刚才的位置 ，使用波浪号这个键

## 移动操作

> d 向右移动到文字结尾  
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

## vim插件操作

- NERDTree 打开导航栏
- NERDTreeClose 关闭导航栏
- CommandT 快速搜索文件命令

## 状态栏插件安装

>Airline 可以在 vim 中显示更友好的状态栏，再配以漂亮的配色
Airline <https://github.com/vim-airline/vim-airline>
Airline Themes <https://github.com/vim-airline/vim-airline-themes>
添加 .vimrc 配置
Bundle 'vim-airline/vim-airline'
Bundle 'vim-airline/vim-airline-themes'
执行 :PlugInstall
基本配置:
set laststatus=2    " 始终显示状态栏
" Airline
let g:airline#extensions#tabline#enabled=1    " 开启 tab 栏
执行 :AirlineTheme simple 应用 simple 样式，或者修改 .vimrc 文件，添加如

## 寄存器指令

例如：输入【qq】 下方出现 recording @q 代表开始记录指令的执行动作

在录制时，开头和结尾的位置是特别需要注意的：
开头 数字0指令 移动到最前，预先做一个位置校正的动作
结果0j 指令 移动到下一行的开头，为了下一个指令的执行
完成指令以后，在输入 【q】 结束集合指令的录制

执行寄存器的集合指令
【@q 指令】 可以执行一次
【@@ 指令】 重复上一次的集合指令动作

追加指令录制
使用按键q指令加上大写Q 就可以不上缺少的指令
qQ 指令 追加代号q的集合指令

## 其他命令操作

- json格式化操作
命令： %！python -m json.tool 就可以格式化json字符串
vim 内部也有json格式化命令：：JSONForm
