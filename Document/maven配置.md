---
title: maven配置
tags: work
notebook: 极客指南
---
```git
git_version = $(git branch 2>/dev/null | sed -e '/^[^*]/d'-e's/* \(.*\)/\1/') 
npm_bin= $(npm bin)
all: test 
test-desktop: server 
    mvn -s settings.xml clean install 
    mvn test -Dtest=DesktopSampleTest 
test-android: server 
    mvn -s settings.xml clean install 
    mvn test -Dtest=AndroidSampleTest 
test-ios: server 
    mvn -s settings.xml clean install 
    mvn test -Dtest=IOSSampleTest 
test-h5: server 
    mvn -s settings.xml clean install 
    mvn test -Dtest=H5SampleTest 
server: 
    @${npm_bin}/macaca server --verbose & 
.PHONY: test
1、先安装homebrew，安装后使用brew install maven安装，不要手动下载安装包，配置环境变量容易出错

2、自动安装好后再/usr/local/Cellar/maven/3.3.9/目录下，设置环境变量
设置环境变量有两种：1、设置系统环境变量在/erc/profile里面2、在～/.bash_profile新建一个，然后添加环境变量

3、直接在系统环境变量添加mvn环境变量，先添加java环境变量，查看java环境变量使用 /usr/libexec/java_home查看java路径/Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home

4、打开配置文件sudo su ,vi /etc/profile 在设置JAVA_HOME
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/"
CLASS_PATH="$JAVA_HOME/lib"
PATH=".;$PATH:$JAVA_HOME/bin"
然后设置mvn变量
M3_HOME="/usr/local/Cellar/maven/3.3.9"
export M3_HOME
M3="$M3_HOME/bin"
export PATH=$M3:$PATH
设置完后，source /etc/profile
在输入mvn -v检查一下是否安装成功
在intellilj idea中配置maven，preferecnces-build-build tools—maven选择
maven home directory选择maven版本目录，选择settings.xml文件
把settings.xml放在mvn的conf目录下
ssh shenwandong@relay.sankuai.com ssh登陆
