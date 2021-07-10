---
title: vim 一键安装脚本使用 
tags: vim
notebook: wiki 
---
- [](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E5%89%8D%E8%A8%80)前言
- [](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#vim%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC)Vim一键安装脚本
- [](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85zope)手动安装zope
- [](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E5%90%8E%E8%AE%B0)后记
  - [编写python程序](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E7%BC%96%E5%86%99python%E7%A8%8B%E5%BA%8F)
  - [多窗口操作](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E5%A4%9A%E7%AA%97%E5%8F%A3%E6%93%8D%E4%BD%9C)
  - [编写markdown文件](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E7%BC%96%E5%86%99markdown%E6%96%87%E4%BB%B6)
  - [快速注释](https://ketao1989.github.io/2014/08/26/Vim-Plugin-On-Mac/#%E5%BF%AB%E9%80%9F%E6%B3%A8%E9%87%8A)

## 前言

vim工具，对于一个程序猿来讲，是使用非常普遍的。以前在Ubuntu上安装`ma6174`大神提供的安装脚本，一键安装一些非常好用的插件，但是对于Mac来说，一直不能很好地执行一件安装。

这里，记录一下，使用大神提供的安装脚本出现问题的解决方法。

## Vim一键安装脚本

`ma6174`大神提供的安装脚本，不能直接在Mac 上运行，直接copy到本地，脚本为：

```

#!/bin/bash

brew install vim ctags git astyle
sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future easy_install twisted

sudo easy_install -ZU autopep8 twisted
sudo ln -s /usr/bin/ctags /usr/local/bin/ctags
mv -f ~/vim ~/vim_old
cd ~/ && git clone https://github.com/ma6174/vim.git
mv -f ~/.vim ~/.vim_old
mv -f ~/vim ~/.vim
mv -f ~/.vimrc ~/.vimrc_old
mv -f ~/.vim/.vimrc ~/
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
vim vim_mac -c "BundleInstall" -c "q" -c "q"
rm vim_mac
echo "安装完成"
            
```

> Note：上面的脚本是直接从一键安装命令中的build脚本部分拷贝的，因为我的Mac无法直接执行一键安装命令。此外，Mac必须安装HomeBrew工具，关于该工具的安装使用，请参考 [http://brew.sh/index\_zh-cn.html](http://brew.sh/index_zh-cn.html)

## 手动安装zope

在上面的脚本安装完成之后，控制台执行`vim`命令，会报错：

```

Error detected while processing /Users/rum/.vim/plugin/client.vim:
line 314:
Traceback (most recent call last):
File "", line 2, in 
File "/Library/Python/2.7/site-packages/Twisted-14.0.0-py2.7-macosx-10.9-intel.egg/twisted/init.py", line 53, in 
checkRequirements()
File "/Library/Python/2.7/site-packages/Twisted-14.0.0-py2.7-macosx-10.9-intel.egg/twisted/__init_.py", line 37, in _checkRequirements
raise ImportError(required + ": no module named zope.interface.")
ImportError: Twisted requires zope.interface 3.6.0 or later: no module named zope.interface.

```

然后，Google搜了各种方法都不生效，无奈执行，只能源码安装了。安装起来很简单，直接执行就好了。

1. 源码下载地址：[https://pypi.python.org/packages/source/z/zope.interface/zope.interface-4.1.0.tar.gz#md5=ac63de1784ea0327db876c908af07a94](https://pypi.python.org/packages/source/z/zope.interface/zope.interface-4.1.0.tar.gz#md5=ac63de1784ea0327db876c908af07a94)
2. 解压缩之后，执行安装命令:`sudo python setup.py install`。

完成之后，在执行 `vim`命令，就不会报错了。

## 后记

安装完插件之后，果然用起来十分爽哇！！此外，`ma6174`大神还提供了一些使用`Tips`。

### 编写python程序

1. 自动插入头信息：
    - `#!/usr/bin/env python`
    - `# coding=utf-8` - 输入`.`或按`TAB`键会触发代码补全功能 - `:w`保存代码之后会自动检查代码错误与规范 - 按`F6`可以按`pep8`格式对代码格式优化 - 按`F5`可以一键执行代码

### 多窗口操作

1. 使用`:sp + 文件名`可以水平分割窗口
    - 使用`:vs + 文件名`可以垂直分割窗口
    - 使用`Ctrl + w`可以快速在窗口间切换

### 编写markdown文件

1. 编写markdown文件(`*.md`)的时候，在normal模式下按 `md` 即可在当前目录下生成相应的`html`文件
    - 生成之后还是在normal模式按`fi`可以使用firefox打开相应的`html`文件预览
    - 当然也可以使用万能的`F5`键来一键转换并打开预览
    - 如果打开过程中屏幕出现一些混乱信息，可以按`Ctrl + l`来恢复

### 快速注释

- 按 `\` 可以根据文件类型自动注释

___
