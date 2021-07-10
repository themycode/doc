---
title: Linux经典命令
tags: 
notebook: 极客指南
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->
- [记录linux操作命令](#记录linux操作命令)
  - [**查看当前目录下文件大小排序**](#查看当前目录下文件大小排序)
  - [**jenkins执行远程服务启动，并且打印出服务日志命令**](#jenkins执行远程服务启动并且打印出服务日志命令)
  - [**压缩**](#压缩)
  - [**解压缩**](#解压缩)
  - [**查看端口连接数**](#查看端口连接数)
  - [**查看端口被占用的**](#查看端口被占用的)
  - [**shell遍历创建目录 拷贝文件**](#shell遍历创建目录-拷贝文件)
  - [**在文件中搜索关键字**](#在文件中搜索关键字)
  - [**查看本机外网ip**](#查看本机外网ip)
  - [**linux 监控**](#linux-监控)
  - [**查询进程并杀死**](#查询进程并杀死)
  - [**awk匹配**](#awk匹配)
  - [**wc -l**](#wc--l)
  - [**grep awk**](#grep-awk)
  - [**mysql**](#mysql)
  - [**常用命令**](#常用命令)
  - [查看所有监听的端口](#查看所有监听的端口)
  - [scp](#scp)
  - [**sed替换文本内容**](#sed替换文本内容)
  - [**awk使用**](#awk使用)
  - [awk '{print}' /etc/passwd](#awk-print-etcpasswd)
  - [**使用外部脚本**](#使用外部脚本)
  - [**对拷文件夹 (包括文件夹本身)**](#对拷文件夹-包括文件夹本身)
  - [**对拷文件夹下所有文件 (不包括文件夹本身)**](#对拷文件夹下所有文件-不包括文件夹本身)
  - [**对拷文件并重命名**](#对拷文件并重命名)
  - [**cat远程文件内容追加**](#cat远程文件内容追加)

## 记录linux操作命令

### **查看当前目录下文件大小排序**

`du -sh * | sort -rh`

### **jenkins执行远程服务启动，并且打印出服务日志命令**

 `nohup java $JAVA_OPTS  -Dfile.encoding=UTF-8 -Djava.security.egd=file:/dev/./urandom -Dspring.profiles.active=test -jar classroom.jar --server.port=$port> nohup.out 2>&1 &`

`tips!` : 两个服务器之间拷贝文件，如果a的服务要往b服务上拷贝文件，把a服务的ssh生成的公钥放在b的~/.ssh/authorized_keys中，这样就可以减少服务认真登陆输入root密码的问题

`grep -E -v '^$|#'` 过滤掉空白符和#号的内容，如两个地址  
 `#l-yace-rouji6.cls.beta.ali.dm 52722`

 `l-yace-rouji7.cls.beta.ali.dm 52722`

### **压缩**

 `tar -czvf ***.tar.gz`

 `tar -cjvf ***.tar.bz2`

### **解压缩**

 `tar -xzvf ***.tar.gz`

 `tar -xjvf ***.tar.bz2`

### **查看端口连接数**

 `netstat -anlp | grep 8787 | grep ESTABLISHED | wc -l`

### **查看端口被占用的**

 `lsof -i tcp:8080`

### **shell遍历创建目录 拷贝文件**

1. 遍历拷贝文件 `for ((i=3;i<10;i++)); do cp pm2.json a$i-abc;done`
2. 遍历拷贝文件 `for ((i=3;i<10;i++)); do cp pm2.json a$i-abc;done`  

> 设置别名：alias 命令 定义命令别名，格式为“命令别名=‘实际命令’  

暂时设置别名为python `alias python='/usr/local/bin/python2.7'`

> 软连接方式：ln -s /usr/local/bin/python python, 在目录下创建python的软连接

### **在文件中搜索关键字**

>grep -c 'error' log.txt 显示行号  
grep -o 'error' log.txt 显示关键字  
grep 'error' log.txt 显示关键字包含的内容  
grep -C 5 'false' catalina.out 显示文件里匹配false那行以及上下5行  
grep -B 5 'false' catalina.out 显示false及前5行  
grep -A 5 'false' catalina.out 显示false及后5行
查看安装目录 which node

### **查看本机外网ip**

curl ifconfig.me 就会显示外网ip  
curl cip.cc 查询本机ip 归属地

### **linux 监控**

`iostat -d -k 2` io监控 -d表示磁盘，-k表示转换成千字节，每两秒检查一次  
 `sudo netstat -antp | grep 10.103.132.53`

`dstat -N total` 监控系统  
`perf top` 系统性能工具

### **查询进程并杀死**

 `pgrep firefox | xargs kill -s 9`

`ps -ef | grep firefox | awk '{print $2}' | xargs kill -9`  `pkill speed` 杀死进程  
`pgrep java` 查询Java的进程  
`netstat -lnpt` 查看活跃的端口链接

### **awk匹配**

 `awk -F "|" '{print $2}' stu.txt | awk -F "}" '{print $1}'`

匹配以|开始以}结束的字段  
 `awk -F 'Json:' '{print $2}' stu.txt`

以Json分隔，把Json前面和后面的分为两块，如果$1是取Json前面的数据，$2取Json后面的数据。匹配Json之后的所有文件!（非常牛逼啊！！）

### **wc -l**

> 统计文件 根据文件里的关键字出现的次数统计出现多少次  
> `cat signal-kp.log_test.2017-08-07| grep "Client Json" |wc -l`

### **grep awk**

> pan= `casperjs test get_token.js|grep -E " http:|FAIL" |awk -F '0m' '{print $0}'`
> 精确匹配speed进程`ps -ef | grep speed | grep -v grep`
> 使用grep -E 可以筛选多个条件来查询

### **mysql**

新增一个字段，并设置默认值  

``` sql
alter table panamera.order add discount_price decimal(10,2) Default 0.00;  
alter table order_sayabc.order add convert_lesson_count SMALLINT NOT NULL DEFAULT '0';  
alter table order_sayabc.product add is_normal_course TINYINT NOT NULL DEFAULT '0' COMMENT '是否是常规课 0.常规课 1.非常规课';
```

查看表中的字段  
desc order；

### **常用命令**

`alias` 用来设置指令的别名  
`alias l='ls -lsh'` alias 新的命令='原命令 -选项/参数'  
要删除别名可以使用 `unalias l`

 `chmod -R 777 /usr/local/bin/node`

 `chmod 644 * -R`

 `sudo su -切换`

 `sudo su -切换到root权限下`

linux查看指定端口状态： `netstat -antlp|grep 8080`

`find / -name 'docker'` 从系统根目录开始查找  
`find . -name 'docker'` 从当前目录开始查找

linux下所有安装的软件都会在/usr/local/bin/下来执行，所以要做一个软连接  
 `ln -s /home/shenwandong/node-v8.1.3-linux-x64/bin/node /usr/local/bin/node`

让nodejs在后台执行，可以这样执行  
 `nohup node app.js &`

forever让nodejs应用后台执行  
`$ sudo npm install forever -g` #安装  
`$ forever start app.js` #启动  
`$ forever stop app.js` #关闭  
`$ forever start -l forever.log -o out.log -e err.log app.js` #输出日志和错误

### 查看所有监听的端口  

 `netstat -a | grep LISTEN`

> 命令 lsof -i tcp:port （port替换成端口号，比如6379）可以查看该端口被什么程序占用，并显示PID，方便KILL

查看端口是否存在 `netstat -antlp | grep 8061`

通过pid查看端口 `netstat -antlp | grep 1477`

今天发现服务器上Tomcat 8080端口起不来，老提示端口已经被占用。  
使用命令：  
ps -aux | grep tomcat发现并没有8080端口的Tomcat进程。
使用命令：netstat –nat查看所有的进程和端口使用情况。发现下面的进程列表，其中最后一栏是PID/Program name发现8080端口被PID为9658的Java进程占用。

进一步使用命令：ps -aux | grep java，或者直接：ps -aux | grep pid 查看

查看ssh端口：vi /etc/ssh/sshd_config

就可以明确知道8080端口是被哪个程序占用了！然后判断是否使用KILL命令干掉！

方法二：直接使用 netstat -nat | grep portno  
即：netstat –apn | grep 8080

### scp

>拷贝远程目录到本地  
scp -r test@192.168.23.113:/Users/vipkid/Documents/webserver /User/shen  
拷贝远程文件到本地  
scp test@192.168.23.113:/home/oracle/test.sql /home/shen
指定端口拷贝
scp  -P 9989  /home/ligh/index.php   root@192.168.8.139:/root

### **sed替换文本内容**

1. 使用方法sed 参数-ig，如果想直接在原文件上进行修改，需要添加-i参数，否则sed是不会直接操作源文件的
2. 其中Code是文本原来内容，Hello是需要替换为新的内容,/g是替换每一行的内容

> 注：sed命令在mac和Linux中使用还是有一些小的区别，在mac下要用-ig参数，在Linux下-i 参数就可以
    sed -i 's/原内容/需要替换的内容/g' test.txt  
    sed -ig 's/Code/Hello/g' test.txt
    // 其他常用命令 ^表示行首，$表示行尾
    sed -i 's/^/添加的头部&/g' // 在文件行首添加
    sed -ig 's/^/super linux&/g' test.txt
    sed -i 's/$/&添加的尾部/g' // 在所有行尾添加
    sed -ig 's/$/&over/g' text.txt
    sed -i '2s/原来字符串/替换字符串/g' //替换第二行
    sed -i '2,5s/原来字符串/替换字符串/g' //替换2-5行
    sed -i '2,$s/原来字符串/替换字符串/g' //替换2到最后一行

### **awk使用**

* 第一个awk  

### awk '{print}' /etc/passwd

> 解释：将输出/etc/passwd文件的内容输出到屏幕。调用awk时，我们指定/etc/passwd作为输出文件，执行awk时，它依次对/etc/passwd中的每一行执行print命令。  
在awk中，花括号用于将几块代码组合到一起，如果只出现print命令，那么将打印当前行的全部内容  
**awk '{print $0}' /etc/passwd  
$0变量表示整个当前行，所以print和print $0作用一样

获取进程的pid  
`**ps x | grep iterm | awk '{print $1}'**  
用到ps，grep，awk三个命令

 `awk -F ":" '{print $1}' /etc/passwd`

使用-F选项来指定":"作为字段分隔符。awk处理print $1命令时。它会打印出输入文件中每一行中出现的第一个字段  
awk -F ":" '{print "username:" $1 "/t/tuid:"$3}' /etc/passwd  
将$1和$3链接，创建可读的输出，添加文本标签

### **使用外部脚本**

创建一个myscript.awk代码

```shell
BEGIN {
        FS =":"
    }
    {print $1}
```

然后执行awk -f myscript.awk /etc/passwd和上面输出一样

cp -rf 强制覆盖不提示

sql常用命令：  
创建库 `create database jedi;`

导入数据 `source /home/shenwandong/schema.sql`

shell脚本远程执行  
 `/root/tmp/redis-3.2.1/src/redis-cli -h 10.102.132.149 -p 6379 "flushall"`

登录远程执行命令  
 `ssh root@${hosts} "cd ${remote_dir}${PG} && pm2 start pm2.json"`

### **对拷文件夹 (包括文件夹本身)**

scp -r /home/wwwroot/www/charts/util root@192.168.1.65:/home/wwwroot/limesurvey_back/scp

### **对拷文件夹下所有文件 (不包括文件夹本身)**

scp /home/wwwroot/www/charts/util/* root@192.168.1.65:/home/wwwroot/limesurvey_back/scp  

### **对拷文件并重命名**

scp /home/wwwroot/www/charts/util/a.txt root@192.168.1.65:/home/wwwroot/limesurvey_back/scp/b.text

### **cat远程文件内容追加**

cat ~/.ssh/id_rsa.pub | ssh user@host “cat - >> ~/.ssh/authorized_keys”
