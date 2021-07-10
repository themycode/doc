---
title: 静态代码扫描 PMD自定义规则
tags: 安全
notebook: wiki
---

> 阅读该文章前，最好已经对 PMD 有了初步的认识和了解，可参考[静态分析工具 PMD 使用说明](http://blog.csdn.net/ml5271169588/article/details/6975690)

## 准备工作

首先在[PMD 官网](https://pmd.github.io/)下载最新版本的文件，目前最新版本是 5.4.1。  
下载 pmd-bin-5.4.1.zip 和 pmd-src-5.4.1.zip 之后解压备用。  
pmd-src-5.4.1 是 PMD 源码包，是无法直接执行的。  
pmd-bin-5.4.1 是 PMD 的可执行包。

## 目录简介

* pmd-bin-5.4.1【PMD 可执行版本】
  * bin
    * designer.bat【界面工具，能将 java 源代码转化为 AST（抽象语法树），个人推荐使用】
    * bgastviewer.bat【界面工具，与 designer.bat 功能相似】  

    * cpd.bat【用来查找重复代码的工具，命令行版】
    * cpdgui.bat【用来查找重复代码的工具，GUI 版】
    * pmd.bat【Window 平台下运行 PMD 需要使用的文件】
    * run.sh【Linux 平台下运行 PMD 需要使用的文件】
  * lib【该目录存放 PMD 运行依赖的 jar 包，包括第三方 jar 包和各种语言的模块 jar 包】

___

* pmd-src-5.4.1【PMD 源代码版本】
  * pmd-core【PMD 的核心执行调度模块】
  * pmd-java【针对 java 语言的检测模块】
    * src ->main  
      * java -> net -> sourceforge -> pmd -> lang->java【目录太深，在此处聚合】
      * rule【该目录下存放已经编写好的 java 规则文件】
        * basic【基础类规则】
        * AvoidBranchingStatementAsLastInLoopRule.java【避免在循环的最后使用分支语句】
        * AvoidMultipleUnaryOperatorsRule.java【避免一元运算符的多重使用】
        * ...【其他基础类的规则文件】
        * codesize【代码体积类规则】
        * ...【各种规则类别的目录，包含该类别的 java 编写的规则文件】
    * resources
      * rulesets【java 规则对应的 xml 文件】
        * java
        * android.xml【PMD 运行时使用该文件会调用安卓类规则进行扫描】
        * basic.xml【PMD 运行时使用该文件会调用基础类规则进行扫描】
        * ...【其他类别的规则 xml 文件】
    * etc
      * grammar
        * Java.jjt【AST 抽象语法树生成所需的语法文件】
  * pmd-java8【新增对 java1.8 版本的支持模块】
  * pmd-javascript【针对 javascript 语言的检测模块】
  * pmd-jsp【针对 jsp 语言的检测模块】
  * ...【其余的主要是针对不同语言实现的独立的检测模块】

## 自定义规则实现思路

1. 明确想要自定义的规则。
2. 列举会触犯这种规则的所有不同的写法。
3. 使用 designer.bat 分析所有写法的抽象语法树的特点。
4. 编写规则代码捕捉这种特点。
5. 创建自己的 xml 规则文件，内容包括规则的相关信息。
6. 运行 PMD 扫描错误代码，验证是否能触发自定义规则。

**下面以一个比较简单的规则举例，详细的阐述一下实现这个规则的具体步骤，帮助大家快速上手。**  
目前 PMD 支持两种编写规则的方法：

1. 使用 Java 进行编写
2. 使用 XPath 表达式 我首先选择第一种 Java 编写方式进行讲解。 \*\*\*\*

### 1\. 明确想要自定义的规则

需要自定义的规则：While 循环必须使用括号，While 循环没有括号很容易困惑代码结构。所以下面以 “While 循环必须使用括号” 这条规则为例。

### 2\. 列举会触犯这种规则的所有不同的写法

写出问题样例的代码写法。

```
class Example {
 void bar() {
  while (baz)
   buz.doSomething();
 }
}
```

弄清楚样例代码是什么样子的，就成功了一半。

### 3\. 使用 designer.bat 分析所有写法的抽象语法树的特点

PMD 扫描时并不是直接使用源码；它使用 `JavaCC` 生成解析器来解析源代码并生成 AST(抽象语法树)。你可以使用 PMD 自带的 designer 工具进行解析代码。  
该工具所在目录：pmd-bin-5.4.1/bin/designer.bat  
双击 designer.bat 后出现一个界面，在 Source code 中填入源代码，点击 Go 按钮：  
[](http://img.blog.csdn.net/20160509180317573)

[![这里写图片描述](http://img.blog.csdn.net/20160509180317573)](http://img.blog.csdn.net/20160509180317573)

以上样例代码解析成抽象语法树后如下：

```
CompilationUnit
 TypeDeclaration
  ClassDeclaration:(package private)
   UnmodifiedClassDeclaration(Example)
    ClassBody
     ClassBodyDeclaration
      MethodDeclaration:(package private)
       ResultType
       MethodDeclarator(bar)
        FormalParameters
       Block
        BlockStatement
         Statement
          WhileStatement
           Expression
            PrimaryExpression
             PrimaryPrefix
              Name:baz
           Statement
            StatementExpression:null
             PrimaryExpression
              PrimaryPrefix
               Name:buz.doSomething
              PrimarySuffix
               Arguments
```

图片中 Abstract Syntax Tree/XPath/Symbol Table 的位置就是抽象后的树形结构，这个树形结构和源代码是有对应关系的。  
其中我们需要重点关注的 `WhileStatement` 的抽象树结构如下：

```
WhileStatement
 Expression
 Statement
  StatementExpression
```

这个是错误的代码示例的抽象树结构，如果 While 循环加上了括号，抽象树的结构就会变成：

```
WhileStatement
 Expression
 Statement
  Block
   BlockStatement
    Statement
     StatementExpression
```

这下能明显的看到了比之前多处了 `Block` 和 `BlockStatement` 这两个节点。  
这样我们只需要写一个规则检查 `WhileStatement` 下有没有 `Block` 节点，如果没有 `Block` 节点，那就说明 While 语句后面没有大括号，就可以报警告知这里是有问题的。  
顺便提一句，所有的结构信息，比如一个 `Statement` 节点后面可能跟着一个 `Block` 节点，这些都是在[EBNF grammar](https://github.com/pmd/pmd/blob/master/pmd-java/etc/grammar/Java.jjt)中定义的。比如在这个语法定义中，一个 `Statement` 的定义是这样的：

```
void Statement() :
{}
{
  LOOKAHEAD( { isNextTokenAnAssert() } ) AssertStatement()
| LOOKAHEAD(2) LabeledStatement()
| Block()
| EmptyStatement()
| StatementExpression() ";"
| SwitchStatement()
| IfStatement()
| WhileStatement()
| DoStatement()
| ForStatement()
| BreakStatement()
| ContinueStatement()
| ReturnStatement()
| ThrowStatement()
| SynchronizedStatement()
| TryStatement()
}
```

以上代码列出了一个 `Statement` 节点后面所有的可能的节点类型。

### 4\. 编写规则代码捕捉这种特点

我们需要新建一个规则文件，也就是一个 Java 类，名称为 WhileLoopsMustUseBracesRule.java。  
新建的位置也是有要求的，以我在上文中介绍的目录结构为例：  
新的规则类位置：pmd-src-5.4.1\\pmd-java\\src\\main\\java\\net\\sourceforge\\pmd\\lang\\java\\rule\\这个目录下即可。  
截图如下，标红处就是汇聚了所有规则文件的 rule 目录：  
[](https://testerhome.com/uploads/photo/2017/9f44203d-1106-4556-bcc8-19bd1a583e24.png!large)

[![](https://testerhome.com/uploads/photo/2017/9f44203d-1106-4556-bcc8-19bd1a583e24.png!large)](https://testerhome.com/uploads/photo/2017/9f44203d-1106-4556-bcc8-19bd1a583e24.png!large)

该类必须继承 `net.sourceforge.pmd.lang.java.rule. AbstractJavaRule` 。

```
import net.sourceforge.pmd.lang.java.rule.*;
public class WhileLoopsMustUseBracesRule extends AbstractJavaRule {
}
```

PMD 工作原理就是在生成的抽象语法树中递归的遍历，直到找出要找的目标，然后返回结果。  
接下来我们的目标就是在抽象语法树中找出 `WhileStatement` 节点下不存在 `Statement/Block` 这种结构的情况。

```
import net.sourceforge.pmd.lang.ast.*;
import net.sourceforge.pmd.lang.java.ast.*;
import net.sourceforge.pmd.lang.java.rule.*;

public class WhileLoopsMustUseBracesRule extends AbstractJavaRule {
    public Object visit(ASTWhileStatement node, Object data) {
        Node firstStmt = node.jjtGetChild(1);
        if (!hasBlockAsFirstChild(firstStmt)) {
            addViolation(data, node);
        }
        return super.visit(node,data);
    }
    private boolean hasBlockAsFirstChild(Node node) {
        return (node.jjtGetNumChildren() != 0 && (node.jjtGetChild(0) instanceof ASTBlock));
    }
}
```

这段代码的主要意思：

1. 访问文件中的`ASTWhileStatement`节点
2. 获取`ASTWhileStatement`节点下第二个子节点
3. 判断第二个子节点的第一个子节点是不是 ASTBlock 节点
4. 如果不是 ASTBlock 节点，说明我们的目标实现，触犯规则
5. 使用`addViolation(data, node);`语句记录触犯该规则的节点相关数据

### 5\. 创建自己的 xml 规则文件，内容包括规则的相关信息

现在规则已经写完了，我们需要告诉 PMD 运行时执行这条规则，就得将这个规则文件的相关信息放在 XML 规则集文件中。例如： `pmd-java/src/main/resources/rulesets/java/basic.xml` ；这里面有很多规则的定义，复制粘贴一下，改成一个新的规则集文件，名字自己随便取： `mycustomrules.xml` ，自己填充一下元素和属性。  
name - WhileLoopsMustUseBracesRule  
message - Use braces for while loops  
class - 放哪都行. 注意，没有必要放在 `net.sourceforge.pmd` 目录下，可以放在 `com.yourcompany.util.pmd`

description - Use braces for while loops  
example - 通过代码片段展示违反的规则样例

```
<?xml version="1.0"?>
<ruleset name="My custom rules"
    xmlns="http://pmd.sourceforge.net/ruleset/2.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 http://pmd.sourceforge.net/ruleset_2_0_0.xsd">
    <rule name="WhileLoopsMustUseBracesRule"
          message="Avoid using 'while' statements without curly braces"
          class="WhileLoopsMustUseBracesRule">
      <description>
      Avoid using 'while' statements without using curly braces
      </description>
        <priority>3</priority>

      <example>
<![CDATA[
    public void doSomething() {
      while (true)
          x++;
    }
]]>
      </example>
    </rule>
</ruleset>
```

### 6\. 运行 PMD 扫描错误代码，验证是否能触发自定义规则

在执行前需要把你修改后的 pmd-java 重新打包：  
在命令行中进入 `pmd-src-5.4.1\pmd-java` 目录中，执行 `mvn clean package` 。  
打包成功后，将 `pmd-src-5.4.1\pmd-java\target` 中的 pmd-java-5.4.1.jar 替换 `pmd-bin-5.4.1\lib` 目录中对应的 jar 包。  
最后在 `pmd-bin-5.4.1\bin` 目录中执行  
 `pmd.bat -d c:\path\to\my\src -f xml -R c:\path\to\mycustomrules.xml`

可在命令行界面中查看结果。  
[](http://img.blog.csdn.net/20160513150321461)

[![这里写图片描述](http://img.blog.csdn.net/20160513150321461)](http://img.blog.csdn.net/20160513150321461)

成功！使用 Java 编写的自定义规则完成！

## 使用 XPath 表达式编写该规则

PMD 是支持 XPath 引擎的，这条 “While 循环必须使用括号” 也可以使用 XPath 表达式实现。  
 `//WhileStatement[not(Statement/Block)]`

意思是匹配查找整个抽象语法树中 `WhileStatement` 节点下不存在 `Statement/Block` 这种结构的情况。  
XPath 表达式完成后，不需要写 java 代码，只写一个 xml 规则文件就行了。

```
<?xml version="1.0"?>
<ruleset name="My XPathRule rules"
    xmlns="http://pmd.sourceforge.net/ruleset/2.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 http://pmd.sourceforge.net/ruleset_2_0_0.xsd">
    <rule  name="WhileLoopsMustUseBracesRule"
    language="java"
  message="Avoid using 'while' statements without curly braces"
  class="net.sourceforge.pmd.lang.rule.XPathRule">
  <description>
  Avoid using 'while' statements without using curly braces
  </description>
  <properties>
    <property name="xpath">
    <value>
<![CDATA[
//WhileStatement[not(Statement/Block)]
]]>
    </value>
    </property>
  </properties>
  <priority>3</priority>
  <example>
<![CDATA[
class Example {
 void bar() {
  while (baz)
   buz.doSomething();
 }
}
]]>
  </example>
</rule>
</ruleset>
```

运行时指向这个新编写的 xml，查看结果：  
[](http://img.blog.csdn.net/20160513164942330)

[![这里写图片描述](http://img.blog.csdn.net/20160513164942330)](http://img.blog.csdn.net/20160513164942330)

成功！  
小技巧：PMD 自带的 designer.bat 工具可以快速生成一个 xpath rule xml。

1. 打开 designer 界面工具，输入源代码，输入 XPath 表达式，点击 Go 按钮，确认右下方的结果输出正确。
2. 点击左上方 Actions->Create rule XML
3. 在新的页面输入 Rule name,Rule msg,Rule desc 后，点击 Create rule XML 按钮，查看输出的结果。 注意：目前版本的 designer 有一个小 BUG，需要自己在 XML 中的 rule 标签中指定被测代码的属性 language="java"

这篇文章只是教大家快速的上手自定义 PMD 规则，下一篇文章将重点分析复杂规则如何编写，敬请期待。

## 参考文献

> PMD site. [How to write a PMD rule](https://pmd.github.io/pmd-5.4.1/customizing/howtowritearule.html)  
> ONJava.com.[Custom PMD Rules](http://www.onjava.com/pub/a/onjava/2003/04/09/pmd_rules.html)  
> CSDN.[静态分析工具 PMD 使用说明](http://blog.csdn.net/ml5271169588/article/details/6975690)

## 360Qtest 团队公众号

关注公众号，第一时间收到我们推送的新文章~  
[](https://testerhome.com/photo/2016/ae8a6fd86ac7867d4eda696b39963d20.jpg)

[![](https://testerhome.com/photo/2016/ae8a6fd86ac7867d4eda696b39963d20.jpg)](https://testerhome.com/photo/2016/ae8a6fd86ac7867d4eda696b39963d20.jpg)
