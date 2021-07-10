---
title: Maven test集成TestNG + ExtentReports
tags: maven
notebook: wiki
---

***本篇主要讲解在maven项目的单元测试中，如何直接使用mvn test命令来驱动测试代码。***

- [**maven中添加依赖**](#maven中添加依赖)
- [**添加build plugin**](#添加build-plugin)
- [**testNg.xml配置listener**](#testngxml配置listener)
- [**项目结构如下：**](#项目结构如下)
- [**POM**](#pom)
- [**ExtentTestNGIReporterListener**](#extenttestngireporterlistener)
- [**test.xml**](#testxml)
- [**testCase**](#testcase)

### **maven中添加依赖**

```xml
<dependency>
   <groupId>org.testng</groupId>
   <artifactId>testng</artifactId>
   <version>6.14.3</version>
   <scope>test</scope>
  </dependency>
  <dependency>
   <groupId>org.uncommons</groupId>
   <artifactId>reportng</artifactId>
   <version>1.1.4</version>
   <scope>test</scope>
   <exclusions>
    <exclusion>
     <groupId>org.testng</groupId>
     <artifactId>testng</artifactId>
    </exclusion>
   </exclusions>
  </dependency>
  <dependency>
   <groupId>com.google.inject</groupId>
   <artifactId>guice</artifactId>
   <version>4.0</version>
   <scope>test</scope>
  </dependency>
  <dependency>
   <groupId>com.aventstack</groupId>
   <artifactId>extentreports</artifactId>
   <version>3.0.6</version>
  </dependency>
```

### **添加build plugin**

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.7.2</version>
    <configuration>
     <suiteXmlFiles>testNg.xml</suiteXmlFiles>
     <properties>
      <property>
       <name>listener</name>
       <value>com.shuidihuzhu.sdb.consumer.config.ExtentTestNGIReporterListener</value>
      </property>
     </properties>
    </configuration>

   </plugin>
```

### **testNg.xml配置listener**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="TestSuite">
    <listeners>
        <listener class-name="org.uncommons.reportng.HTMLReporter"/>
        <listener class-name="org.uncommons.reportng.JUnitXMLReporter"/>
    </listeners>
    <test name="sdb-consumer">
        <classes>
            <class name="com.shuidihuzhu.sdb.consumer.consumer.msg.AdvertiseFreeApplyInterruptConsumerTest"></class>
<!--            <class name="com.shuidihuzhu.sdb.consumer.consumer.artificialSale.ArtificialSaleEntranceConsumerTest"></class>-->
<!--            <class name="com.shuidihuzhu.sdb.consumer.consumer.artificialSale.ArtificialSaleOrderNotPayDelayConsumerTest"></class>-->
        </classes>
    </test>
</suite>
```

### **项目结构如下：**

现在要做的是，使用 mvn test命令时，可以自动执行test.xml和test2.xml文件两个文件，并使用extentreport自动生成测试报告

![](https://ask.qcloudimg.com/http-save/yehe-1456994/crkgw7dihs.png?imageView2/2/w/1620)

### **POM**

dependencies部分就是testng和extentreport的依赖，在此不做拓展，主要讲解plugin pom > build > plugins

```xml
<plugin\>
    <groupId\>org.apache.maven.plugins</groupId\>
    <artifactId\>maven\-surefire\-plugin</artifactId\>
    <version\>2.22.1</version\>
    <configuration\>
        <suiteXmlFiles\>
            <suiteXmlFile\>${project.basedir}/src/test/resources/test/test.xml</suiteXmlFile\>
            <suiteXmlFile\>${project.basedir}/src/test/resources/test/test2.xml</suiteXmlFile\>
        </suiteXmlFiles\>
        <properties\>
            <property\>
                <name\>listener</name\>
                <value\>com.test.config.ExtentTestNGIReporterListener</value\>
            </property\>
        </properties\>
    </configuration\>
</plugin\>
```

suiteXmlFile指定所有需要执行的testng的xml文件 property指定所有监听器，例如ExtentReports 或者 MultiThreadingListener进行压测等

### **ExtentTestNGIReporterListener**

我做了如下两种修改，有需要的同学可以做个参考： 1、测试报告路径：根目录/report 2、测试报告的生成结构，具体修改情况可以查看下方的截图

```java
package com.test.config;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.ResourceCDN;
import com.aventstack.extentreports.Status;
import com.aventstack.extentreports.model.TestAttribute;
import com.aventstack.extentreports.reporter.ExtentHtmlReporter;
import com.aventstack.extentreports.reporter.configuration.ChartLocation;
import com.aventstack.extentreports.reporter.configuration.Theme;
import org.testng.\*;
import org.testng.xml.XmlSuite;

import java.io.File;
import java.util.\*;

public class ExtentTestNGIReporterListener implements IReporter {
    
    private static final String OUTPUT\_FOLDER \= "report/";

    private ExtentReports extent;

    @Override
    public void generateReport(List<XmlSuite\> xmlSuites, List<ISuite\> suites, String outputDirectory) {
        init();
        boolean createSuiteNode \= false;
        if(suites.size()\>1){
            createSuiteNode\=true;
        }
        for (ISuite suite : suites) {
            Map<String, ISuiteResult\> result \= suite.getResults();
            
            if(result.size()\==0){
                continue;
            }
            
            int suiteFailSize\=0;
            int suitePassSize\=0;
            int suiteSkipSize\=0;
            ExtentTest suiteTest\=null;
            
            if(createSuiteNode){
                suiteTest \= extent.createTest(suite.getName()).assignCategory(suite.getName());
            }
            boolean createSuiteResultNode \= false;
            if(result.size()\>1){
                createSuiteResultNode\=true;
            }
            for (ISuiteResult r : result.values()) {
                ExtentTest resultNode;
                ITestContext context \= r.getTestContext();
                if(createSuiteResultNode){
                    
                    if( null \== suiteTest){
                        resultNode \= extent.createTest(r.getTestContext().getName());
                    }else{
                        resultNode \= suiteTest.createNode(r.getTestContext().getName());
                    }
                }else{
                    resultNode \= suiteTest;
                }
                if(resultNode != null){
                    resultNode.getModel().setName(suite.getName()+" : "+r.getTestContext().getName());
                    if(resultNode.getModel().hasCategory()){
                        resultNode.assignCategory(r.getTestContext().getName());
                    }else{
                        resultNode.assignCategory(suite.getName(),r.getTestContext().getName());
                    }
                    resultNode.getModel().setStartTime(r.getTestContext().getStartDate());
                    resultNode.getModel().setEndTime(r.getTestContext().getEndDate());
                    
                    int passSize \= r.getTestContext().getPassedTests().size();
                    int failSize \= r.getTestContext().getFailedTests().size();
                    int skipSize \= r.getTestContext().getSkippedTests().size();
                    suitePassSize += passSize;
                    suiteFailSize += failSize;
                    suiteSkipSize += skipSize;
                    if(failSize\>0){
                        resultNode.getModel().setStatus(Status.FAIL);
                    }
                    resultNode.getModel().setDescription(String.format("Pass: %s ; Fail: %s ; Skip: %s ;",passSize,failSize,skipSize));
                }
                buildTestNodes(resultNode,context.getFailedTests(), Status.FAIL);
                buildTestNodes(resultNode,context.getSkippedTests(), Status.SKIP);
                buildTestNodes(resultNode,context.getPassedTests(), Status.PASS);
            }
            if(suiteTest!= null){
                suiteTest.getModel().setDescription(String.format("Pass: %s ; Fail: %s ; Skip: %s ;",suitePassSize,suiteFailSize,suiteSkipSize));
                if(suiteFailSize\>0){
                    suiteTest.getModel().setStatus(Status.FAIL);
                }
            }

        }

        extent.flush();
    }

    private void init() {
        
        File reportDir\= new File(OUTPUT\_FOLDER);
        if(!reportDir.exists()&& !reportDir .isDirectory()){
            reportDir.mkdir();
        }
        String FILE\_NAME \=  System.currentTimeMillis() + ".html";
        ExtentHtmlReporter htmlReporter \= new ExtentHtmlReporter(OUTPUT\_FOLDER + FILE\_NAME);
        
        
        htmlReporter.config().setResourceCDN(ResourceCDN.EXTENTREPORTS);

        htmlReporter.config().setDocumentTitle("单元测试报告");
        htmlReporter.config().setReportName("单元测试报告");
        htmlReporter.config().setChartVisibilityOnOpen(true);
        htmlReporter.config().setTestViewChartLocation(ChartLocation.TOP);
        htmlReporter.config().setTheme(Theme.STANDARD);
        htmlReporter.config().setCSS(".node.level-1  ul{ display:none;} .node.level-1.active ul{display:block;}");

        extent \= new ExtentReports();
        extent.attachReporter(htmlReporter);
        extent.setReportUsesManualConfiguration(true);
    }

    private void buildTestNodes(ExtentTest extenttest, IResultMap tests, Status status) {
        
        String\[\] categories\=new String\[0\];
        if(extenttest != null ){
            List<TestAttribute\> categoryList \= extenttest.getModel().getCategoryContext().getAll();
            categories \= new String\[categoryList.size()\];
            for(int index\=0;index<categoryList.size();index++){
                categories\[index\] \= categoryList.get(index).getName();
            }
        }

        ExtentTest test;

        if (tests.size() \> 0) {
            
            Set<ITestResult\> treeSet \= new TreeSet<ITestResult\>(new Comparator<ITestResult\>() {
                @Override
                public int compare(ITestResult o1, ITestResult o2) {
                    return o1.getStartMillis()<o2.getStartMillis()?\-1:1;
                }
            });
            treeSet.addAll(tests.getAllResults());
            for (ITestResult result : treeSet) {

                String name \= "";
                if (result.getMethod().getDescription() \== null){
                    name \= result.getMethod().getTestClass().getName();
                } else {
                    name \= result.getMethod().getDescription();
                }
                if(extenttest\==null){
                    test \= extent.createTest(name);
                }else{
                    
                    test \= extenttest.createNode(name).assignCategory(categories);
                }
                for (String group : result.getMethod().getGroups())
                    test.assignCategory(group);

                List<String\> outputList \= Reporter.getOutput(result);
                for(String output:outputList){
                    
                    test.debug(output);
                }
                if (result.getThrowable() != null) {
                    test.log(status, result.getThrowable());
                }
                else {
                    test.log(status, "Test " + status.toString().toLowerCase() + "ed");
                }

                test.getModel().setStartTime(getTime(result.getStartMillis()));
                test.getModel().setEndTime(getTime(result.getEndMillis()));
            }
        }
    }

    private Date getTime(long millis) {
        Calendar calendar \= Calendar.getInstance();
        calendar.setTimeInMillis(millis);
        return calendar.getTime();
    }
}

```

### **test.xml**

![](https://ask.qcloudimg.com/http-save/yehe-1456994/68qqr1ko6j.png?imageView2/2/w/1620)

### **testCase**

![](https://ask.qcloudimg.com/http-save/yehe-1456994/3wrh2qjwc6.png?imageView2/2/w/1620)

![](https://ask.qcloudimg.com/http-save/yehe-1456994/xds4p6vlgo.png?imageView2/2/w/1620)

本文分享自微信公众号 - 软件测试君（backlight2018），作者：糖小幽

原文出处及转载信息见文内详细说明，如有侵权，请联系 yunjia\_community@tencent.com 删除。

原始发表时间：2020-06-03

本文参与[腾讯云自媒体分享计划](https://cloud.tencent.com/developer/support-plan)，欢迎正在阅读的你也加入，一起分享。
