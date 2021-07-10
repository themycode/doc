---
title: linux 常用30个命令技巧
tags: linux
notebook: 极客指南
---

- [01. Vim自动添加注释及智能换行](#01-vim自动添加注释及智能换行)
- [02. 查询并删除/data这个目录7天前建立的文件](#02-查询并删除data这个目录7天前建立的文件)
- [03. tar命令压缩排出某个目录](#03-tar命令压缩排出某个目录)
- [04. 查看tar包存档文件，不解压](#04-查看tar包存档文件不解压)
- [05. 使用stat命令检视一个档案的属性](#05-使用stat命令检视一个档案的属性)
- [06. 批量解压tar.gz](#06-批量解压targz)
- [07. 筛除文件中的注释和空格](#07-筛除文件中的注释和空格)
- [08. *筛选/etc/passwd目录中所有的使用者*](#08-筛选etcpasswd目录中所有的使用者)
- [09. iptables 将本机80转发到本地8080](#09-iptables-将本机80转发到本地8080)
- [10. find命令查找档案并复制到/opt目录](#10-find命令查找档案并复制到opt目录)
- [11. 查找目录下大于1G的档案](#11-查找目录下大于1g的档案)
- [12. 插入一行到391航，包括特殊符号“/”](#12-插入一行到391航包括特殊符号)
- [13. 列出nginx日志访问最多的10个IP](#13-列出nginx日志访问最多的10个ip)
- [14. 显示nginx日志一天访问量最多的前10位IP](#14-显示nginx日志一天访问量最多的前10位ip)
- [15. 找出1-255之间的整数](#15-找出1-255之间的整数)
- [16. 找出ip地址](#16-找出ip地址)
- [17. 检查网络状态命令](#17-检查网络状态命令)
- [18. 删除换行符并将空格替换别的字符](#18-删除换行符并将空格替换别的字符)


 
#### 01. Vim自动添加注释及智能换行


``` sh
# vi ~/.vimrc
set autoindent
set tabstop=4
set shiftwidth=4
function AddTitle()
call setline(1,"#!/bin/bash")
call append(1,"#====================================================")
call append(2,"# Author: lizhenliang")
call append(3,"# Create Date: " . strftime("%Y-%m-%d"))
call append(4,"# Description: ")
call append(5,"#====================================================")
endf
map <F4> :call AddTitle()<cr>
```

打开文本后，按F4就会自动添加注释

#### 02. 查询并删除/data这个目录7天前建立的文件  

 `# find /data -ctime +7 -exec rm -rf {} \;`

 `# find /data -ctime +7 | xargs rm -rf `

#### 03. tar命令压缩排出某个目录  

 `tar zcvf data.tar.gz /data --exclude=tmp  # --exclude不包含某个目录活着文件`

#### 04. 查看tar包存档文件，不解压  

 `# tar tr data.tar.gz #t是列出存档档案目录，f是制定存档档案`

#### 05. 使用stat命令检视一个档案的属性  

 `stat index.php`

访问时间（Access）、修改时间（modify）、状态改变时间(Change)  

#### 06. 批量解压tar.gz  

> 方法1: # find . name "*.tar.gz" -exec tar zxf {} \; 

方法2: # for tar in *.tar.gz; do tar zxvf $tar; done  
方法3: # ls *.tar.gz | xargs -i tar zxvf {}

#### 07. 筛除文件中的注释和空格  

> 方法1: # grep -v "^#" httpd.conf | grep -v "^$"  

方法2: # sed -e `/^$/d' -e ` /^$/d' httpd.conf > http.conf  
方法3: # sed -e '/^#/d; /^$/d' # -e 执行多条sed命令  
方法4: # awk '/^[^#]/|/"^$"' httpd.conf  

#### 08. *筛选/etc/passwd目录中所有的使用者*

> 方法1: # cat /etc/passwd | cut -d: -f1
> 方法2: # awk -F ":" '{print $1}' /etc/passwd

#### 09. iptables 将本机80转发到本地8080  

> #iptables -t ant -A PREROUTING -p tcp --dport 80 -j PEDIRECT --to-ports 8080

#### 10. find命令查找档案并复制到/opt目录  

> 方法1 # find /ect -name httpd.conf -exec cp -rf {} /opt/ \; :  #-exec執行後面命令，{}代表前面輸出的結果，\; 結束命令  

方法2: # find /etc -name httpd.conf |xargs -i cp {} /opt   #-i表示輸出的結果由{}代替  

#### 11. 查找目录下大于1G的档案  

#find / -size +1024M  

#### 12. 插入一行到391航，包括特殊符号“/”

> #sed -i "391 s/^/AddType application\/x-httpd-php .php .html/" httpd.conf

#### 13. 列出nginx日志访问最多的10个IP

> 方法1: # awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -n 10  

sort : 排序  
uniq -c : 合并重复行，并记录重复次数  
sort -nr: 按照数字进行降序排序  

#### 14. 显示nginx日志一天访问量最多的前10位IP

> #awk '$4>="[16/May/2017:00:00:01" && $4<="[16/May/2017:23:59:59"' access_test.log |sort |uniq -c |sort-nr |head -n 10   

#awk '$4>="[16/Oct/2017:00:00:01" && $4<="[16/Oct/2017:23:59:59"{a[$1]++}END{for(i in a){print a[i], i|"sort -k1 -nr |head -n 10"}}' access.log

#### 15. 找出1-255之间的整数  

方法1: # ifconfig | grep  -o '[0-9]\+'  # +号匹配前一个字元一次或多次  
多方2: # ifconfig |egrep -o '\<([1-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\>  

#### 16. 找出ip地址

> #ifconfig |grep -o '[0-9]\{1, 3\}\.[0-9]\{1, 3\}\.[0-9]\{1, 3\}\.[0-9]\{1, 3\}' #-o只顯示匹配字元

#### 17. 检查网络状态命令

> netstat -antp  # 检查所有网络链接  

netstat -lntp  # 只检查监听的链接  
lsof -p pid # 检查程序开启的文件控制代码  
lsof -i: 80 # 检查端口呗那个程序占用

#### 18. 删除换行符并将空格替换别的字符

> cat a.txt | xargs echo -n | sed 's/[]/|/g' # -n 不换行

cat a.txt | tr -d '\n' # 删除换行符
