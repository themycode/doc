---
title: python 常用包推荐
tags: python
notebook: Python
---

#### 爬虫登陆神器

工具名称：DecryptLogin
GitHub地址：<https://github.com/CharlesPikachu/DecryptLogin>
功能特点：一键式登陆各大网站的工具，所有的登陆代码只需要3行

```Python
from DecryptLogin import login

lg = login.Login()
infos_return, session = lg.weibo()
print(infos_return)
```

#### rich 一款美化终端富文本内容的命令行工具

在终端下使用Python过程中，内容美化工具
