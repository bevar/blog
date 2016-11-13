Title: vim plugin vim template
Date: 2016-07-21 00:00
Category: 软件
Slug: vim-plugin-vim-template
Summary: vim的模板插件的安装配置指南

# vim模板插件vim-template的使用

之前使用IDE编程，模板是最基本的功能，现在切换到vim，用惯了模板的我，对于每次写代码都来上
```
# -*- coding: utf-8 -*-
#
# 日期 作者
#
# License
```

这么一段，感觉有点恐怖，Google了一下，vim竟然可以实现比IDE更强大的功能，真是不得不佩服vim的强大！


## 安装

由于不同人的vim配置不一样，安装方法也不尽相同，请参考[官方的安装方法](https://github.com/aperezdc/vim-template)来安装。
对于使用spf13-vim的同学，在主目录下的.vimrc.bundles.local添加一行:
```
Bundle 'git@github.com:aperezdc/vim-template.git'

# 不知道"Bundle 'vim-template'"为什么老要我输入用户名密码，
# 输入之后也安装不成功，知道的同学可以留言告知我。
```

然后在shell里面运行：
```shell
vim +BundleInstall
```

或者打开vim，运行:
```
:BundleInstall
```

都行。

## 基本使用

如果你对自带的模板没有很多意见，你甚至什么都不用配置，你用vim打开一个新的python脚本，vim-template会自动检测
文件后缀，然后自己根据模板填充文件，一切都那么理所当然。你甚至可以打开没有后缀的文件，然后运行:Template &lt;pattern&gt;(比如:Template \*.py)在文件
开头插入模板，:TemplateHere &lt;pattern&gt;(比如: TemplateHere \*.py)在光标所在的位置插入模板。


## 配置

安装好vim-template之后，可以根据自己的需要来配置：

### 1. 自定义模板变量

vim-template可以自定义模板变量, 在主目录的.vimrc(如果你使用spf13-vim的话就修改.vimrc.local)中添加:

```
let g:templates_user_variables = [['EMAIL', 'GetEmail'],]

function GetEamil()
    return 'pylego@hotmail.com'
endfunction
```
> 注意：上面都是VimScript的语法，有兴趣的可以学习下。

这样你就可以在模板里面写%EMAIL%，然后模板引擎会自动调用GetEmail函数，将返回值代替之。


### 2. 自定义模板

有时候你想根据自己的需要定义模板，在vim-template里面是很容易实现的

#### 1. 修改g:templates_directory到你自定义模板所在的文件夹

```
# 在.vimrc(如果你使用spf13-vim的话就是.vimrc.local)
let g:templates_directory = '/home/pylego/.vim/templates'
```

#### 2. 在/home/pylego/.vim/templates中创建模板文件

文件的命名模式是"=template=&lt;pattern&gt;", 比如我想让每个以py结尾的文件都以这个模板开始就可以这样:

创建一个名为"=template=.py"的文件，向文件里面写入模板。

注意这个"=template="前缀是可以改动的，具体不在本文讨论范围内。


## 我的配置
> spf13-vim

.vimrc.local

```
" settings for vim-template bundle
let g:templates_directory = ['/home/pylego/.vim/templates',]
let g:templates_user_variables = [['EMAIL', 'GetMail'], ['FULLPATH', 'GetFullPath']]

function GetMail()
    return 'pylego@hotmail.com'
endfunction

function GetFullPath()
    return expand('%:p')
endfunction
" vim-template settings end
```

/home/pylego/.vim/templates/=template=.py
```
# -*- coding: utf-8 -*-
#
# Copyright © PyleGo.
#
# %DATE% %TIME% <%EMAIL%>
#
# Distributed under terms of the %LICENSE% license.
```
