---
title: 静态分析工具PMD使用说明
tags: PMD, 安全
notebook: wiki
---

### 静态分析工具PMD使用说明

目录

[静态分析工具PMD使用说明... 1](http://write.blog.csdn.net/postedit#_Toc208371499)

[目录... 2](http://write.blog.csdn.net/postedit#_Toc208371500)

[1.           编写目的... 3](http://write.blog.csdn.net/postedit#_Toc208371501)

[2.           PMD简介... 4](http://write.blog.csdn.net/postedit#_Toc208371502)

[3.           PMD的安装和运行... 4](http://write.blog.csdn.net/postedit#_Toc208371503)

[3.1安装并从命令行运行PMD. 4](http://write.blog.csdn.net/postedit#_Toc208371504)

[3.2在Eclipse中安装PMD插件运行方式... 6](http://write.blog.csdn.net/postedit#_Toc208371505)

[3.3 使用Ant进行调用... 8](http://write.blog.csdn.net/postedit#_Toc208371506)

[4.           关于PMD规则... 10](http://write.blog.csdn.net/postedit#_Toc208371507)

[5.           编写自定义的PMD规则... 15](http://write.blog.csdn.net/postedit#_Toc208371508)

[6.           结束语... 18](http://write.blog.csdn.net/postedit#_Toc208371509)

[7.    参考资料... 18](http://write.blog.csdn.net/postedit#_Toc208371510)

质量是衡量一个软件是否成功的关键要素。而对于商业软件系统，尤其是企业应用软件系统来说，除了软件运行质量、文档质量以外，代码的质量也是非常重要的。软件开发进行到编码阶段的时候，最大的风险就在于如何保证代码的易读性和一致性，从而使得软件的维护的代价不会很高。

在软件开发的过程中，以下几种情形随处可见：

1) 软件维护时间长，而且维护人员的积极性不高：

 做过软件维护的开发人员，尤其是在接手不是自己开发产品的源码的时候，即使有良好的文档说明，仍然会对代码中冗长、没有注释的段落“叹为观止”。理解尚且如此困难，何况要修改或者增加新的功能。因此，很多开发人员不愿意进行软件维护的工作。

2）新的开发人员融入团队的时间比较长：

　除了没有良好的培训、文档等有效的机制以外，每个人一套的编码风格，也容易造成新成员对于已有代码的理解不够，甚至出现偏差。

提高[代码](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=4744&sid=6235007045041793&click=1&url=http%3A//www.cnzz.cn&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=330290&k=%u4EE3%u7801 "新浪Sina/宏界Vogate提供")的质量，除了要提高逻辑上的[控制](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=5856&sid=6235007045041793&click=1&url=http%3A//www.021ln.com/ln906v.htm&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=470902&k=%u63A7%u5236 "新浪Sina/宏界Vogate提供")以及业务流程的理解外，代码本身也存在提高的[空间](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=6178&sid=6235007045041793&click=1&url=http%3A//www.dqzbbs.com&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=315908&k=%u7A7A%u95F4 "新浪Sina/宏界Vogate提供")，例如一些潜在的问题可以很早的就避免。类似于编码规范上的内容，如果全靠编码人员[进行](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10118&sid=6235007045041793&click=1&url=http%3A//www.mechr.com/topic/080801/&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=404484&k=%u8FDB%u884C "新浪Sina/宏界Vogate提供")自行检查，那么无疑[需要](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10119&sid=6235007045041793&click=1&url=http%3A//www.mechr.com/topic/080801/&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=546914&k=%u9700%u8981 "新浪Sina/宏界Vogate提供")很大的工作量，如果可以使用代码的静态检查[工具](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10186&sid=6235007045041793&click=1&url=http%3A//unionafa.allyes.com/main/adfclick%3Fdb%3Dunionafa%26bid%3D474%2C122%2C23%26cid%3D6601%2C309%2C1%26sid%3D319%26show%3Dignore%26url%3Dhttp%3A//buycar.chezhu.com.cn/price.html%3Fcid%3D15&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=291530&k=%u5DE5%u5177 "新浪Sina/宏界Vogate提供")进行检查的话，那么将大大的提高编码的效率。

项目组目前代码检查的工作基本上都是通过人工的方式，实行起来比较困难，检查的效果也不是很明显。PMD正是这样一种工具，可以直接使用它自带的规则（当然也可以使用自己的规则）对Java源程序进行分析找出程序存在的问题，可以很大程度上的减轻代码检查工作的繁琐，为项目组今后的维护和开发工作起到指导的作用。

本文主要介绍了如何使用pmd工具进行代码的自动化检查，以规避一些潜在的问题并找出代码的逻辑错误。

PMD是一种开源分析Java代码错误的工具。与其他分析工具不同的是，PMD通过静态分析获知代码错误。也就是说，在不运行Java程序的情况下报告错误。PMD附带了许多可以直接使用的规则，利用这些规则可以找出Java源程序的许多问题，例如：

® 潜在的bug：空的try/catch/finally/switch语句

® 未使用的代码：未使用的局部变量、参数、私有方法等

® 可选的代码：String/StringBuffer的滥用

® 复杂的表达式：不必须的if语句、可以使用while循环完成的for循环

® 重复的代码：拷贝/粘贴代码意味着拷贝/粘贴bugs

® 循环体创建新对象：尽量不要再for或while循环体内实例化一个新对象

@ 资源关闭：Connect，Result，Statement等使用之后确保关闭掉

此外，用户还可以自己定义规则，检查Java代码是否符合某些特定的编码规范。例如，你可以编写一个规则，要求PMD找出所有创建Thread和Socket对象的操作。

PMD的核心是JavaCC解析器生成器。PMD结合运用JavaCC和EBNF（扩展巴科斯-诺尔范式，Extended Backus-Naur Formal）语法，再加上JJTree，把Java源代码解析成抽象语法树（AST，Abstract Syntax Tree）。显然，这句话不那么好懂，且看下文具体说明。

　　从根本上看，Java源代码只是一些普通的文本。不过，为了让解析器承认这些普通的文本是合法的Java代码，它们必须符合某种特定的结构要求。这种结构可以用一种称为EBNF的句法元语言表示，通常称为“语法”（Grammar）。JavaCC根据语法要求生成解析器，这个解析器就可以用于解析用Java编程语言编写的程序。

　　不过实际运行中的PMD还要经过JJTree的一次转换。JJTree是一个JavaCC的插件，通过AST扩充JavaCC生成的解析器。AST是一个Java符号流之上的语义层。有了JJTree，语法分析的结果不再是“System, ., out, ., . println”之类的符号序列，而是一个由对象构成的树型层次结构。例如，下面是一段简单的Java代码以及与之对应的AST。

Java源代码：  
public class Foo {

public void bar() {

System.out.println("hello world");  
}  
}  
对应的抽象语法树  
CompilationUnit  
TypeDeclaration  
ClassDeclaration  
UnmodifiedClassDeclaration  
ClassBody  
ClassBodyDeclaration  
MethodDeclaration  
ResultType  
MethodDeclarator  
FormalParameters  
Block  
BlockStatement  
Statement  
StatementEXPression  
PrimaryExpression  
PrimaryPrefix  
Name  
PrimarySuffix  
Arguments  
ArgumentList  
Expression  
PrimaryExpression  
PrimaryPrefix  
Literal

### 4.1安装并从命令行运行PMD

你可以从PMD的网站下载PMD的二进制版本，或下载带源代码的版本，下载得到的都是ZIP文件。假设你下载了二进制版本，先把它解压缩到任意一个目录。接下来怎么做，就要看你准备怎么用它——最简单的，如果要在一个Java源代码目录中运行PMD，只需直接在命令行上运行下面的命令：

E:\\SoftWare\\pmd-bin-4.2.1\\pmd-4.2.1\\bin>java -jar ..\\lib\\pmd-4.2.1.jar D:\\ebsser

vice\\ebsservice\\src text rulesets/unusedcode.xml

输出结果类如：

D:\\ebsservice\\ebsservice\\src\\com\\sinosoft\\service\\policy\\ebs\\SMPolicyInput.java:

51      Avoid unused private fields such as 'logger'.

D:\\ebsservice\\ebsservice\\src\\com\\sinosoft\\service\\policy\\ebs\\SMPolicyShow.java:2

5       Avoid unused private fields such as 'logger'.

D:\\ebsservice\\ebsservice\\src\\com\\sinosoft\\service\\policy\\ebs\\SMQueryPolicyByPoli

cyNo.java:32    Avoid unused local variables such as 'visaStatus'.

D:\\ebsservice\\ebsservice\\src\\com\\sinosoft\\service\\policy\\ebs\\SMQueryPolicyByPoli

cyNo.java:44    Avoid unused local variables such as 'temp'.

D:\\ebsservice\\ebsservice\\src\\com\\sinosoft\\service\\policy\\ebs\\erisk\\ESMPolicyInpu

t.java:28       Avoid unused private fields such as 'logger'.

D:\\ebsservice\\ebsservice\\src\\com\\sinosoft\\service\\policy\\ebs\\jrisk\\JSMPolicyInpu

t.java:22       Avoid unused private fields such as 'logger'.

一些可以加载必须参数前面或者后面的可选参数如下：

\-debug: 打印debug日志信息

\-targetjdk: 指定目标源代码的版本- 1.3, 1.4, 1.5, 1.6 or 1.7;

默认是1.5

\-cpus: 指定创建的线程数

\-encoding: 指定PMD检查的代码的编码方式

\-excludemarker: 指定PMD需要忽略的行的标记，默认为NOPMD

\-shortnames: 在报告中显示缩短的文件名

\-linkprefix: HTML源文件的路径，只是为了HTML显示

\-lineprefix: 自定义的锚，用于影响源文件中的行，只是用于HTML显示

\-minimumpriority: 规则的优先级限制，低于优先级的规则将不被使用

\-nojava: 不检查java文件，默认是检查java文件

\-jsp: 检查JSP/JSF文件，默认不检查

\-reportfile: 将报告输出到文件，默认是打印在控制台

\-benchmark: 输出一个基准清单，默认输出到控制台

\-xslt: 覆盖默认的xslt

\-auxclasspath: 指定源代码文件使用的类路径

例如在windows系统中，例子如下：

c:\\> java -jar pmd-4.2.1.jar c:\\my\\source\\code text unusedcode,imports -targetjd

k 1.5 -debug

c:\\> java -jar pmd-4.2.1.jar c:\\my\\source\\code xml basic,design -encoding UTF-8

c:\\> java -jar pmd-4.2.1.jar c:\\my\\source\\code html typeresolution -auxclasspath

 commons-collections.jar;derby.jar

### 4.2在Eclipse中[安装](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10139&sid=6235007045041793&click=1&url=http%3A//www.mechr.com/topic/080801/&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=236570&k=%u5B89%u88C5 "新浪Sina/宏界Vogate提供")PMD插件运行方式

PMD可以作为插件集成到很多[流行](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10190&sid=6235007045041793&click=1&url=http%3A//unionafa.allyes.com/main/adfclick%3Fdb%3Dunionafa%26bid%3D493%2C141%2C23%26cid%3D6601%2C309%2C1%26sid%3D362%26show%3Dignore%26url%3Dhttp%3A//buycar.chezhu.com.cn/promotion_0318.php%3Fcid%3D15&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=881511&k=%u6D41%u884C "新浪Sina/宏界Vogate提供")的IDE中，很多的插件中都包含了PMD的jar文件，这个jar文件中包含了规则集。所以虽然一些插件中使用rulesets/unusedcode.xml来作为参数引用规则集，但是实际上是使用getResourceAsStream()方法来从PMD的jar文件中加载。

由于Eclipse是比较流行的开源Java/J2EE开发IDE，所以本文主要介绍如何在Eclipse中使用PMD工具进行代码的检查。

#### 4.2.1 安装基于[Eclipse](http://gocom.primeton.com/modules/gSpace/about_onetag.php?tagid=103&tagname=Eclipse&PHPSESSID=23ba501255f28f67dceea4e438726cba) IDE的插件

安装Eclipse的PMD插件的过程如下：

® 启动Eclipse

® 选择Help-->Software Updates-->Find and Install

® 选择Next,选择New remote site

® 在Name框中输入PMD，URL框中输入[http://pmd.sf.net/eclipse](http://pmd.sf.net/eclipse)

® 在之后的对话框中一直点击下一步或者接受协议，完成Eclipse的PMD插件的安装

也可以通过下载[最新](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10185&sid=6235007045041793&click=1&url=http%3A//unionafa.allyes.com/main/adfclick%3Fdb%3Dunionafa%26bid%3D494%2C142%2C23%26cid%3D6601%2C309%2C1%26sid%3D364%26show%3Dignore%26url%3Dhttp%3A//buycar.chezhu.com.cn/promotion_0318.php%3Fcid%3D16&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=727130&k=%u6700%u65B0 "新浪Sina/宏界Vogate提供")的zip文件按，然后执行上述过程，只是使用New locale site来代替New remote site，并使用下载的zip文件。

可以通过Windows-->Preferences来配置PMD。

通过右键一个[项目](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=5931&sid=6235007045041793&click=1&url=http%3A//www.cnmty.com&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=149675&k=%u9879%u76EE "新浪Sina/宏界Vogate提供")，然后选择PMD-->Check node with PMD，即可使用PMD工具检查代码。如果要进行重复代码检测，那么右键一个项目后，选择PMD-->Find suspect cut and paste。检查结果会放在reports目录下，文件名为cpd-report.txt。

可以通过使用Eclipse的帮助系统来查看PMD插件的文档。

在安装完更新后，如果发生了一个异常，例如”java.lang.RuntimeException: Could not find that class xxxx”，这时试着删除workspace中的.metadata/plugins/net.sourceforge.pmd.eclipse目录下的ruleset.xml文件。

#### 4.2.2 使用PMD

1、启动Eclipse IDE，打开工程，选择 "Windows"->"Preferences"下的PMD项，其中Rules Configuration 项目可以配置PMD的检查规则，自定义检查规则也可以在此通过Import的方式导入到PMD中

2、配置好后，鼠标右键点击工程中需要检查的JavaSource，选择"PMD"->"Check Code With PMD" ,之后PMD就会通过规则检查你的JavaSource了并且将信息显示在PMD自己的视图上

3、示例

import java.util.\*;

public class Test {

        public static void main(String\[\] args) {

             try{

                if(true) {}

                System.out.println("Hello World!");

            } catch(Exception e) {

            }

       }

}

以上代码PMD会检查出：catch块中没有内容、if判断块中没有内容、代码中出现System.out.println等警告描述

### 4.3 使用Ant进行调用

下面是主要的Ant配置信息

```html
<path id="pmd.path">    

    <fileset dir="${lib.dir}/pmd-3.8">

        <include name="\*\*/\*.jar" />

    </fileset>

</path>

<taskdef name="pmd" classname="net.sourceforge.pmd.ant.PMDTask" classpathref="pmd.path"/>

<taskdef name="cpd" classname="net.sourceforge.pmd.cpd.CPDTask" classpathref="pmd.path"/>

    <target name="pmd">

        <pmd shortFilenames="true">

            <ruleset>rulesets/favorites.xml</ruleset>            

            <formatter type="html" toFile="d:\\foo.html" toConsole="false"/>

            <fileset dir="${src.dir}">

                <include name="\*\*/\*.java"/>

            </fileset>

        </pmd>

    </target>

<target name="cpd">        

        <cpd minimumTokenCount="100" outputFile="d:/cpd.txt">

            <fileset dir="${src.dir}">

                <include name="\*\*/\*.java"/>

            </fileset>

        </cpd>

    </target>
```

用Ant命令运行build.xml，PMD就会按照你设定好的规则自动执行代码检查了。

**选择合适的规则**

运行所有的规则集中的规则会产生非常多的冲突，这些冲突中的很多是不重要的。在这么多的冲突中寻找你关心的部分结果就没有什么效率可言了。

所以需要从明显的规则集，也就是说必须要改的[地方](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10382&sid=6235007045041793&click=1&url=&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=809726&k=%u5730%u65B9 "新浪Sina/宏界Vogate提供")开始是比较好的一个选择，例如只是运行unusedcode检查，然后修改没有使用的局部变量和成员变量。然后运行基本的检查，修改所有的空语句，例如if语句等。最后可以执行与设计[相关](http://action.vogate.com/c/c.php?r=http%3A//www.google.cn/search%3Fcomplete%3D1%26hl%3Dzh-CN%26newwindow%3D1%26q%3Dpmd%25E4%25BD%25BF%25E7%2594%25A8%26start%3D20%26sa%3DN&aid=10116&sid=6235007045041793&click=1&url=http%3A//www.mechr.com/topic/080801/&v=0&s=http%3A//www.diybl.com/course/3_program/java/javajs/2008419/110532.html&rn=430217&k=%u76F8%u5173 "新浪Sina/宏界Vogate提供")的或者存在一定争议的规则集，或者自定义的规则集。

**自带规则的介绍： （PMD插件分析代码规则（中文）.xls）**

**PMD 自带了很多规则集合，并且分类写入不同的 ruleset 文件，如**

**Basic 包含每人都必须遵守的代码最佳实践，如EmptyCatchBlock**

**Braces 关于条件分支的规则，如IfStmtsMustUseBraces**

**Code Size 关于代码大小的规则，如方法的长度，参数的长度，属性的个数等**

**Clone 克隆实现的规则，如是否有super.clone()**

**Controversial 一些有争议的规则，如UnnecessaryConstructor不必要的构造器**

**Coupling 对象连接有关的规则**

**Design 可以检查有问题的设计，如SwitchStmtsShouldHaveDefault**

**Finalizers 使用finalizers时需遵循的规则，如FinalizeOnlyCallsSuperFinalize**

**Import Statements 和import有关的规则，如DuplicateImports重复import**

**J2EE 唯一规则UseProperClassLoader，class.getClassLoader()可能不正确，用**

**Thread.currentThread().getContextClassLoader() 代替**

**Javabeans 和javabean规范有关的规则，有BeanMembersShouldSerialize属性必须**

**序列化和MissingSerialVersionUID缺少序列化ID**

**JUnit Tests 和JUnit测试有关的，如JUnitSpelling拼写检查等**

**Logging (Java) 检查Logger的一些错误用法，如MoreThanOneLogger多个Logger**

**Logging (Jakarta) 使用Jakarta Logger的一些规则，有UseCorrectExceptionLogging**

**异常处理不当和ProperLogger是否正确定义Logger**

**Migrating JDK 版本移植的规则，如ReplaceVectorWithList用List代替Vector**

**Naming 和命名有关的规则，名称太短或太长，命名的约定等**

**Optimizations 优化性能的一些规则，如LocalVariableCouldBeFinal本地变量如果**

**只赋值一次，则应该声明为final**

**Strict Exceptions 比较严格的异常处理方针，如AvoidCatchingThrowable**

**Strings 使用String和StringBuffer时应遵守的规则，如StringToString**

**Sun Security 编写安全的代码，有MethodReturnsInternalArray直接返回内部的数组，**

**更安全的做法是返回一个拷贝和ArrayIsStoredDirectly**

**Unused Code 检查未使用的代码，如UnusedPrivateField未使用的私有属性**

**Java Server Pages 编写jsp的一些方针，如NoLongScripts**

**Java Server Faces 编写jsf的一些方针，有DontNestJsfInJstlIteration，在Jsf**

**里使用jstl的标签**

**自定义规则：**

**有两个办法来自定义规则，可以编写java类和编写XPath，编写java类的一般步骤是，先确定要查找的代码形式，利用PMD自带的designer.bat工具查看AST（抽象语法树），然后编写规则类（继承net.sourceforge.pmd.AbstractRule），然后编写一个ruleset的XML文件，最后就可以运行PMD进行检查。编写XPath比编写java类要容易些，但也需要掌握AST的含义，利用designer.bat工具可以查看AST，比如 //ClassBody \[count(//VariableDeclarator\[../Type/Name\[@Image='Logger'\]\])>1\] ，这个表达式就是查找类的代码里是否声明了多个 Logger ，然后编写一个 ruleset 的 XML 文件，最后运行 PMD 进行检查。[**这里**](http://www.onjava.com/pub/a/onjava/2003/04/09/pmd_rules.html)是一个 ruleset 的 XML 文件的例子。**

**自定义规则集合：**

**PMD 自带了很多代码规范的规则，还可以自定义规则，我们可以把这些规则整合到一起，按照我们的需求进行代码检查。**

<!-- 使用整个strings规则集 -->

`<rule ref="rulesets/strings.xml"/>`

<!-- 使用某个规则集里的某个规则 -->

`<rule ref="rulesets/unusedcode.xml/UnusedLocalVariable"/>`

  <!-- 指定某个规则集里的某个规则的优先级 -->

`<rule ref="rulesets/basic.xml/EmptyCatchBlock" message="Must handle exceptions">

    <priority>2</priority>

    </rule>

          <!-- 去除某个规则集里的某个规则 -->

<rule ref="rulesets/braces.xml">

    <exclude name="WhileLoopsMustUseBracesRule"/>

  </rule>

**最后，我们运行PMD的时候就可以指定这个 ruleset 文件。**

**PMD 里面还有一个写好的ruleset文件，在pmd-3.8.jar里面的rulesets文件夹下，名称是favorites.xml，以下是主要部分：**

< rule  ref ="rulesets/basic.xml" />

< rule  ref ="rulesets/basic.xml/EmptyCatchBlock"  message ="Must handle exceptions" >

         < priority > 2 </ priority >

</ rule >

     < rule  ref ="rulesets/unusedcode.xml" />

< rule  ref ="rulesets/braces.xml/WhileLoopsMustUseBraces" />

< rule  ref ="rulesets/braces.xml/ForLoopsMustUseBraces" />

< rule  ref ="rulesets/design.xml/SimplifyBooleanReturns" />

< rule  ref ="rulesets/design.xml/SwitchStmtsShouldHaveDefault" />

< rule  ref ="rulesets/strings.xml/StringToString" />

< rule  ref ="rulesets/strings.xml/StringInstantiation" />

< rule  ref ="rulesets/controversial.xml/UnnecessaryConstructor" />

< rule  ref ="rulesets/controversial.xml/NullAssignment" />

< rule  ref ="rulesets/controversial.xml/UnusedModifier" />

< rule  ref ="rulesets/codesize.xml/CyclomaticComplexity" >

     < properties >< property  name ="reportLevel"  value ="5" /></ properties >

</ rule >

**其它事项：**

**1.     可以使用JDK1.5的声明 @SuppressWarnings("")，禁止PMD的警告。**

**2.     可以使用//NOPMD来标记行或块代码，禁止PMD警告。**

**3.     有两种方法自定义Rule，编写java类和编写XPath。**

前提：

1.了解XPath：<http://www.w3.org/TR/xpath>

2\. 对PMD 的实现原理有一定的了解

实现过程：

首先传一个文件名或者Ruleset给pmd

Pmd把该文件流传给自己生成的javaCC分析器

分析完毕后，pmd获得了分析生成的AST的一个引用

PMD把AST处理成一个符号表，你可以在符号表里面查询一些有用的信息

每个pmd规则都会遍历整个AST并检验是否发生了错误

接着pmd产生一个报表，上面说明了有哪些地方违反了pmd规则

编写pmd规则有两种方法：

一．用java code，需要了解pmd的api，需要进行深入研究，也常常用于一些比较复杂的pmd规则

二．用xpath，对着产生的AST树，写就行了，上手比较快，写起来也比较简单

下面举一个用XPath实现的一个PMD规则：

在项目中，我们不希望Application的开发人员手动的调用Toplink UnitOfWork的commit,

commitAndResume, commitAndResumeOnFailure'方法，因为每次提交都会映像performa，我们的提交是放在自己编写的framework里面，

在指定的位置提交。所以我们把规则的优先级设置为3. 在eclipse的pmd plugin中，优先级为3会产生一个警告。

1首先将D:"local\_lib"pmd-bin-4.2"bin 加到系统环境变量的path中

2打开cmd 运行 designer 分析器

3左上角source code可以把你写好的java source copy过来主要就在这个java source code基础上不断修正你的pmd规则。

4xpath query：用来编写自定义的xpath expression（先不忙写xpath expression）

5点击go，就会在左下角的Abstract syntax Tree中产生AST，你可以选择AST上的某个节点，左下角的下面一个框中就会出现该节点的一些信息。

是在符号表中查询得到的。

6．DFA是pmd4的新功能，用于编写更复杂的pmd规则，不光是某个source code级别了，pmd4使用了asm读取字节码，并作分析，处理类文件之间的依赖性。

在实际使用中，特别是在特定应用中，这个功能是相当有用的。还可以用来简化一些现有的规则。

7根据生成的AST编写xpath expression。对于上文提到的source检查规则编写了一个xpath

Expression，在编写xpath expression的过程中需要反复的修改源代码并且反复的修改xpath expression这样才能满足所有的需要，反复的点击go。

最后写好的规则大致如下：

//PrimaryExpression\[(PrimaryPrefix/Name\[ends-with(@Image,'commit') or ends-with(@Image, 'commitAndResume') or ends-with(@Image, 'commitAndResumeOnFailure')\] and substring-before(PrimaryPrefix/Name/@Image, '.') =

//VariableDeclaratorId\[../..//ClassOrInterfaceType\[@Image =

'UnitOfWork'\]\]/@Image) or (PrimarySuffix\[ends-with(@Image, 'commit') or

ends-with(@Image,'commitAndResume') or ends-with(@Image, 'commitAndResumeOnFailure')\] and (PrimarySuffix\[ends-with(@Image,

'getActiveUnitOfWork')\] or PrimarySuffix\[ends-with(@Image, 'acquireUnitOfWork')\]))

and //ImportDeclaration/Name\[contains(@Image,'oracle.toplink.sessions.UnitOfWork') or contains(@Image, 'oracle.toplink.sessions')\]

\]

8．将写好的xpath expression转换成pmd rule。Designer可以自动生成点击菜单actions下面的create rule xml。

9．最后将生成的rule添加到ruleset中，并最好在大批量的代码中进行验证。

**即使只使用内置规则（内容相当全面），PMD 也可以找到您的代码中的一些真正问题。某些问题可能很小，但有些问题则可能很大。PMD 不可能找到每个 bug，您仍然需要做单元测试和接受测试，在查找已知 bug 时，即使是 PMD 也无法替代一个好的调试器。但是，PMD 确实可以帮助您发现未知的问题。这是一个便宜、简易、有意思的改进程序的方式。如果您以前从未用过 PMD，那么您以及您的客户应该试试它。**

PMD 官方文档（[http://pmd.sourceforge.net/](http://pmd.sourceforge.net/)）

**请参阅 Tom Copeland 撰写的文章“ [**Static Analysis with PMD**](http://www.onjava.com/pub/a/onjava/2003/02/12/static_analysis.html?page=1)”，这篇文章对 PMD 进行了介绍。**

**（**[http://www.onjava.com/pub/a/onjava/2003/02/12/static\_analysis.html?page=1](http://www.onjava.com/pub/a/onjava/2003/02/12/static_analysis.html?page=1)**）**

**从 Tom Copeland 撰写的文章“ [**Custom PMD Rules**](http://www.onjava.com/pub/a/onjava/2003/04/09/pmd_rules.html)”中学习如何编写定制 PMD 规则。**

**（**[http://www.onjava.com/pub/a/onjava/2003/04/09/pmd\_rules.html](http://www.onjava.com/pub/a/onjava/2003/04/09/pmd_rules.html)**）**

[XOM](http://www.xom.nu/) **在本文的示例中多次引用到，它是一个开源的、基于树的 API，用于处理 Java 中的 XML。** （[http://www.xom.nu/](http://www.xom.nu/)）

XPath语法：（[http://www.w3.org/TR/xpath](http://www.w3.org/TR/xpath)）
