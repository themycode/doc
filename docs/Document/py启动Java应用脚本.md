---
title: py启动Java应用脚本
tags: 脚本
notebook: Python
---

最近部署了 java 程序，刚开始用 shell 来启动，启动过程不友好也不方便，也没有守护进程。今年在整理资料的时候，发现了珍藏一套启动脚本。

#### 描述内容

这是我的 java 程序结构 ，一个 jar 文件，一个 config 配置文件 一个 lib 依赖包。
![2021-01-09-16-05-54-202119](http://upload-img.thedev.cn/2021-01-09-16-05-54-202119.png)


#### 脚本代码

重点贴出 appStoreServer.py 脚本代码

```python
#!/usr/bin/python
import os, sys, time, commands

_program = 'appStoreServer.jar'
_daemon = 'appStoreServer.py'

def getProgramPid():
    result = commands.getoutput("ps aux | grep java \
        | grep %s | grep -v grep | awk '{print $2}'" % _program)
    return result

def getDaemonPid():
    result = commands.getoutput("ps aux | grep python \
        | grep '%s monitor' | grep -v grep | awk '{print $2}'" % _daemon)
    return result

def startProgram():
    p_pid = getProgramPid()
    if p_pid != '':
        print('It seems this program is already running...')
    else:
        print('Starting program...')
        if os.system('nohup java -Dfile.encoding=UTF-8 -server -Xms1024m -Xmx1024m -Xmn256m \
-cp %s:config/:lib/* com.xxxx.Bootstrap >> log/stdio.log 2>&1 &' % _program) == 0:
            print('start program successfully and pid is ' + getProgramPid())

def startDaemon():
    d_pid = getDaemonPid()
    if d_pid != '':
        print('It seems this daemon is already running...')
    else:
        print('Starting daemon...')
        if os.system('nohup python %s monitor >> log/daemon.log 2>&1 &' % _daemon) == 0:
            print('start daemon successfully and pid is ' + getDaemonPid())

def stopProgram():
    p_pid = getProgramPid()
    if p_pid == '':
        print('It seems this program is not running...')
    else:
        os.system('kill ' + p_pid)
        print('program stopped')

def stopDaemon():
    d_pid = getDaemonPid()
    if d_pid == '':
        print('It seems daemon is not running...')
    else:
        os.system('kill ' + d_pid)
        print('daemon stopped')

def monitor():
    while 1:
        time.sleep(10)
        p_pid = getProgramPid()
        if p_pid == '':
            print('It seems this program is not running. Start it now!')
            startProgram()

if __name__ == '__main__':
    if(len(sys.argv) == 2):
        args = sys.argv[1]
    else:
        args = raw_input('Enter args: ')

    logPath = os.path.curdir + os.sep + 'log'
    if not os.path.exists(logPath):
        os.mkdir(logPath)

    if args == 'start':
        startProgram()
        startDaemon()
    elif args == 'stop':
        stopDaemon()
        stopProgram()
    elif args == 'restart':
        stopDaemon()
        stopProgram()
        time.sleep(3)
        startProgram()
        startDaemon()
    elif args == 'monitor':
        monitor()
    else:
        print('nothing to do')
```

只需要修改你的 jar 和 main class 入口(这就是文中 com.xxx.bootstrap 部分)。python 调用了 nuhop 命令，退出终端也可以后台运行，挺方便的，重点的是还有 daemo 进程
修改了，开始启动 java 程序
![2021-01-09-16-05-28-202119](http://upload-img.thedev.cn/2021-01-09-16-05-28-202119.png)
